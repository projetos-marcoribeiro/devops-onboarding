# Fluxo de Troubleshooting

## 📑 Índice

- [Fluxo de investigação](#fluxo-de-investigação)
- [Passo a passo detalhado](#passo-a-passo-detalhado)
- [Ações corretivas](#ações-corretivas)
- [Ferramentas de referência rápida](#ferramentas-de-referência-rápida)
- [Comunicação durante o troubleshooting](#comunicação-durante-o-troubleshooting)
- [Referências](#referências)

---

Este documento descreve o processo de investigação durante um incidente. Ter um fluxo estruturado é fundamental para investigar com eficiência sob pressão — sem um método, é fácil perder tempo testando hipóteses aleatórias enquanto o impacto em produção continua.

> Se você está em plantão ativo e precisa de um guia mais detalhado com comandos prontos e padrões de comunicação, consulte também:
> 📄 [`oncall-readiness/incident-investigation-guide.md`](../oncall-readiness/incident-investigation-guide)

---

## Fluxo de investigação

```
Alerta recebido
         │
         ▼
Fazer acknowledge no PagerDuty
         │  sinaliza que você está ciente
         ▼
Verificar dashboards de observabilidade
         │  Grafana, NewRelic, Datadog
         │  qual serviço? qual métrica? desde quando?
         ▼
Verificar logs da aplicação
         │  NewRelic Logs, CloudWatch, kubectl logs
         │  há erros? qual a mensagem? qual o padrão?
         ▼
Verificar estado dos pods Kubernetes
         │  kubectl get pods, describe, events
         │  pods rodando? reiniciando? pendentes?
         ▼
Verificar deploy recente
         │  ArgoCD, GitHub Actions
         │  houve deploy nas últimas horas?
         ▼
Identificar causa raiz
         │  correlacionar evidências
         │  confirmar hipótese com dados
         ▼
Executar ação corretiva
         rollback / scaling / fix forward / correção de config
```

---

## Passo a passo detalhado

### 1. Acknowledge e contexto inicial

Antes de qualquer coisa, faça o acknowledge no PagerDuty e leia o alerta com atenção:
- Qual serviço está afetado?
- Qual métrica disparou o alerta?
- Desde quando o alerta está ativo?
- Há outros alertas relacionados ativos simultaneamente?

### 2. Verificar dashboards de observabilidade

Abra o Grafana e o NewRelic para ter uma visão geral do que está acontecendo.

**Grafana:**
- Verifique o dashboard do serviço afetado
- Observe o momento exato em que as métricas saíram do normal
- Correlacione com outros serviços que possam estar relacionados

**NewRelic:**
- Verifique a taxa de erro e a latência do serviço
- Abra os traces do período do incidente
- Identifique qual operação ou dependência está causando o problema

**Datadog:**
- Verifique o uso de recursos dos nodes e pods
- Confirme se há containers reiniciando

### 3. Verificar logs da aplicação

Logs são frequentemente a fonte mais direta de informação sobre o que está errado.

```bash
# Logs em tempo real de um deployment
kubectl logs -f deployment/<nome-do-deployment> -n <namespace>

# Logs de um pod específico
kubectl logs <pod-name> -n <namespace>

# Logs do container anterior (útil quando o pod reiniciou)
kubectl logs <pod-name> --previous -n <namespace>

# Filtrar por nível de erro
kubectl logs deployment/<nome> -n <namespace> | grep -i error
```

No NewRelic Logs, use queries para filtrar por serviço e período:
```
service.name = "meu-servico" AND level = "ERROR"
```

### 4. Verificar estado dos pods Kubernetes

```bash
# Ver todos os pods do namespace
kubectl get pods -n <namespace>

# Pods com problema (não Running ou com restarts)
kubectl get pods -n <namespace> | grep -v Running

# Detalhes de um pod específico (inclui eventos)
kubectl describe pod <pod-name> -n <namespace>

# Eventos recentes do namespace
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Uso de recursos
kubectl top pods -n <namespace>
kubectl top nodes
```

**Sinais de alerta comuns:**
- `CrashLoopBackOff` — pod reiniciando repetidamente, geralmente erro na aplicação
- `OOMKilled` — pod morto por falta de memória, aumentar limits ou investigar memory leak
- `Pending` — pod não consegue ser agendado, verificar recursos disponíveis nos nodes
- `ImagePullBackOff` — problema ao baixar a imagem Docker, verificar ECR e credenciais

### 5. Verificar deploy recente

A maioria dos incidentes em produção está relacionada a uma mudança recente. Verificar se houve deploy nas últimas horas é sempre um dos primeiros passos.

**Via ArgoCD:**
```bash
# Ver histórico de deploys de uma aplicação
argocd app history <app-name>

# Ver diferença entre versão atual e anterior
argocd app diff <app-name>
```

**Via GitHub Actions:**
Acesse a aba Actions do repositório e verifique os workflows executados recentemente.

**Via kubectl:**
```bash
# Ver histórico de rollout
kubectl rollout history deployment/<nome> -n <namespace>
```

### 6. Identificar causa raiz

Com as evidências coletadas, correlacione as informações para identificar a causa raiz:

- O problema começou junto com um deploy? → provável causa: mudança no código ou configuração
- O problema é em todos os pods ou apenas alguns? → pode indicar problema de node específico
- O problema é em um serviço específico ou em toda a plataforma? → ajuda a isolar o escopo
- Há aumento de tráfego? → pode ser problema de capacidade
- Há erro de dependência externa (banco, fila, API terceira)? → problema pode não ser no seu serviço

---

## Ações corretivas

### Rollback de deploy

Quando o problema está claramente relacionado a um deploy recente, o rollback é geralmente a ação mais rápida para restaurar o serviço.

```bash
# Rollback via ArgoCD (para a versão anterior)
argocd app rollback <app-name>

# Rollback via kubectl
kubectl rollout undo deployment/<nome> -n <namespace>

# Rollback para uma revisão específica
kubectl rollout undo deployment/<nome> --to-revision=3 -n <namespace>
```

### Scaling da aplicação

Quando o problema é de capacidade (muitas requisições, pods sobrecarregados), aumentar o número de réplicas pode aliviar o impacto enquanto a causa raiz é investigada.

```bash
# Aumentar réplicas manualmente
kubectl scale deployment/<nome> --replicas=5 -n <namespace>
```

> Lembre-se de reverter o scaling manual depois, ou atualizar o valor no repositório de configuração para que o ArgoCD não reverta a mudança.

### Correção de configuração

Quando o problema é uma configuração incorreta (variável de ambiente errada, secret inválido, limite de recurso muito baixo), corrija no repositório de configuração e faça deploy via ArgoCD.

```bash
# Verificar variáveis de ambiente de um pod
kubectl exec -it <pod-name> -n <namespace> -- env

# Verificar secrets montados
kubectl exec -it <pod-name> -n <namespace> -- cat /path/to/secret
```

### Fix forward com novo deploy

Quando o rollback não é viável (por exemplo, há migração de banco de dados envolvida) ou quando a correção é simples e rápida, pode ser mais adequado corrigir o código e fazer um novo deploy.

---

## Ferramentas de referência rápida

| Ferramenta | Acesso | Uso principal |
|---|---|---|
| kubectl | Terminal | Estado dos pods, logs, eventos |
| NewRelic | Via Okta | APM, traces, logs de aplicação |
| Datadog | Via Okta | Métricas de infraestrutura |
| CloudWatch | Console AWS | Logs e métricas de serviços AWS |
| ArgoCD | Interface web | Status de deploy, rollback |
| Grafana | Via Okta | Dashboards operacionais |

---

## Comunicação durante o troubleshooting

Enquanto investiga, mantenha o canal de incidentes atualizado:

- Ao começar: "Estou investigando o alerta X. Impacto inicial: Y."
- A cada 15 minutos: "Atualização: investigando hipótese Z. Ainda sem resolução."
- Ao identificar causa: "Causa identificada: W. Executando ação corretiva."
- Ao resolver: "Serviço restaurado. Causa: W. Ação: V. Monitorando."

Comunicação clara durante um incidente reduz ansiedade do time e dos stakeholders e evita que outras pessoas comecem a investigar o mesmo problema em paralelo sem saber.

---

## Referências

📄 [`incident-management/incident-process.md`](incident-process)
📄 [`incident-management/alert-sources.md`](alert-sources)
📄 [`oncall-readiness/incident-investigation-guide.md`](../oncall-readiness/incident-investigation-guide)
📄 [`oncall-readiness/escalation-policies.md`](../oncall-readiness/escalation-policies)
