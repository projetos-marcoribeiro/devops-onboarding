# Guia de Investigação de Incidentes

> 📌 **Este documento é o guia prático de plantão** — comandos prontos e passos para seguir durante um incidente real. Para a metodologia geral de troubleshooting (estudo e referência), consulte o [Fluxo de Troubleshooting](../incident-management/troubleshooting-flow).

## 📑 Índice

- [Fluxo de investigação](#fluxo-de-investigação)
- [Passo 1 — Acknowledge e leitura do alerta](#passo-1--acknowledge-e-leitura-do-alerta)
- [Passo 2 — Verificar dashboards de observabilidade](#passo-2--verificar-dashboards-de-observabilidade)
- [Passo 3 — Verificar logs da aplicação](#passo-3--verificar-logs-da-aplicação)
- [Passo 4 — Verificar estado do cluster Kubernetes](#passo-4--verificar-estado-do-cluster-kubernetes)
- [Passo 5 — Verificar deploy recente](#passo-5--verificar-deploy-recente)
- [Passo 6 — Identificar causa raiz](#passo-6--identificar-causa-raiz)
- [Ações corretivas](#ações-corretivas)
- [Comunicação durante o incidente](#comunicação-durante-o-incidente)
- [Quando escalar](#quando-escalar)
- [Referências](#referências)

---

Este guia é uma referência rápida para usar durante um plantão. Quando um alerta chega às 3 da manhã, você não quer ter que lembrar de cabeça qual é o próximo passo — você quer ter um fluxo claro para seguir.

O objetivo é investigar com método, não com pânico.

> Para o fluxo geral de troubleshooting (fora do contexto de plantão), consulte:
> 📄 [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow)

---

## Fluxo de investigação

```
Alerta recebido no PagerDuty
         │
         ▼
Fazer acknowledge imediatamente
         │  para a escalação automática
         │  sinaliza que você está ciente
         ▼
Ler o alerta com atenção
         │  qual serviço? qual métrica? desde quando?
         │  há outros alertas relacionados?
         ▼
Verificar dashboard de observabilidade
         │  Grafana, NewRelic, Datadog
         │  confirmar o problema e entender o escopo
         ▼
Verificar logs da aplicação
         │  NewRelic Logs, kubectl logs, CloudWatch
         │  há erros? qual padrão? desde quando?
         ▼
Verificar estado do cluster Kubernetes
         │  kubectl get pods, describe, events
         │  pods rodando? reiniciando? pendentes?
         ▼
Verificar deploy recente
         │  ArgoCD, GitHub Actions
         │  houve mudança nas últimas horas?
         ▼
Identificar causa raiz
         │  correlacionar evidências
         │  confirmar hipótese com dados
         ▼
Executar ação corretiva
         │  rollback / scaling / fix forward / correção de config
         ▼
Confirmar resolução e comunicar
         monitorar por alguns minutos
         atualizar canal de incidentes e ticket
```

---

## Passo 1 — Acknowledge e leitura do alerta

Faça o acknowledge no PagerDuty assim que receber a notificação. Isso para a escalação automática e sinaliza que você está ciente.

Antes de abrir qualquer ferramenta, leia o alerta com atenção:
- Qual serviço está afetado?
- Qual métrica disparou? (taxa de erro, latência, uptime, CPU...)
- Desde quando o alerta está ativo?
- Há outros alertas ativos simultaneamente que possam estar relacionados?

Múltiplos alertas simultâneos geralmente indicam um problema maior ou uma causa raiz comum.

---

## Passo 2 — Verificar dashboards de observabilidade

### NewRelic

Acesse o dashboard do serviço afetado e verifique:
- Taxa de erro: está elevada? Desde quando?
- Latência: p95 e p99 estão acima do normal?
- Throughput: houve queda ou pico de tráfego?
- Apdex: está abaixo do threshold?

Abra os traces do período do incidente para identificar qual operação está com problema.

### Datadog

Verifique a saúde da infraestrutura:
- Nodes com CPU ou memória elevada?
- Containers reiniciando?
- Algum node foi removido ou adicionado recentemente?

### Grafana

Verifique os dashboards operacionais do time para ter uma visão geral da plataforma no período do incidente.

---

## Passo 3 — Verificar logs da aplicação

```bash
# Logs em tempo real
kubectl logs -f deployment/<nome> -n <namespace>

# Logs de um pod específico
kubectl logs <pod-name> -n <namespace>

# Logs do container anterior (pod que reiniciou)
kubectl logs <pod-name> --previous -n <namespace>
```

No NewRelic Logs, filtre por serviço e período:
```
service.name = "<nome-do-servico>" AND level = "ERROR"
```

No CloudWatch, verifique os log groups do serviço afetado para erros de serviços AWS (RDS, Lambda, etc.).

**O que procurar nos logs:**
- Mensagens de erro repetidas
- Stack traces de exceções
- Timeouts de conexão com dependências (banco, fila, API externa)
- Erros de autenticação ou autorização
- Mensagens de out of memory

---

## Passo 4 — Verificar estado do cluster Kubernetes

```bash
# Ver pods com problema
kubectl get pods -n <namespace> | grep -v Running

# Detalhes de um pod específico
kubectl describe pod <pod-name> -n <namespace>

# Eventos recentes do namespace
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Uso de recursos
kubectl top pods -n <namespace>
kubectl top nodes
```

**Sinais de alerta e o que significam:**

| Status | Possível causa |
|---|---|
| `CrashLoopBackOff` | Aplicação crashando na inicialização — verificar logs |
| `OOMKilled` | Pod morto por falta de memória — aumentar limits ou investigar leak |
| `Pending` | Pod não consegue ser agendado — verificar recursos dos nodes |
| `ImagePullBackOff` | Problema ao baixar imagem — verificar ECR e credenciais |
| `Error` | Erro genérico — verificar logs e eventos |

---

## Passo 5 — Verificar deploy recente

A maioria dos incidentes em produção está relacionada a uma mudança recente. Sempre verifique.

```bash
# Histórico de deploys no ArgoCD
argocd app history <app-name>

# Histórico de rollout no Kubernetes
kubectl rollout history deployment/<nome> -n <namespace>
```

Verifique também a aba Actions do repositório no GitHub para ver se houve pipeline executado recentemente.

**Pergunta-chave:** o problema começou no mesmo momento de um deploy? Se sim, o rollback é provavelmente a ação mais rápida.

---

## Passo 6 — Identificar causa raiz

Com as evidências coletadas, correlacione as informações:

**Causas comuns e como identificar:**

### Deploy recente
- Problema começou junto com um deploy
- Logs mostram erros que não existiam antes
- Ação: rollback

### Erro de aplicação
- Logs mostram exceções ou erros de código
- Problema em pods específicos, não em toda a infraestrutura
- Ação: investigar o erro, fix forward ou rollback

### Problema de infraestrutura
- Nodes com recursos esgotados
- Pods sendo evictados ou não conseguindo ser agendados
- Ação: scaling de nodes, ajuste de recursos, investigar Karpenter

### Dependência externa
- Logs mostram timeout ou erro de conexão com banco, fila ou API externa
- O problema não está no código da aplicação
- Ação: verificar saúde da dependência, implementar circuit breaker se necessário

---

## Ações corretivas

### Rollback de deploy

```bash
# Via ArgoCD (recomendado)
argocd app rollback <app-name>

# Via kubectl
kubectl rollout undo deployment/<nome> -n <namespace>

# Acompanhar o rollback
kubectl rollout status deployment/<nome> -n <namespace>
```

### Scaling da aplicação

```bash
# Aumentar réplicas para absorver carga
kubectl scale deployment/<nome> --replicas=5 -n <namespace>
```

> Lembre de reverter o scaling manual depois ou atualizar o valor no repositório de configuração.

### Correção de configuração

Corrija no repositório de configuração (devops-helm-iac) e faça sync no ArgoCD:

```bash
argocd app sync <app-name>
```

### Novo deploy com correção (fix forward)

Quando o rollback não é viável, corrija o código, abra PR, faça merge e deixe o pipeline fazer o deploy da versão corrigida.

---

## Comunicação durante o incidente

Mantenha o canal de incidentes atualizado a cada 15 minutos enquanto o incidente estiver ativo:

```
[14:32] Investigando alerta de taxa de erro elevada no serviço payments-api.
        Impacto inicial: checkout afetado. Verificando logs e métricas.

[14:47] Atualização: identificado aumento de erros de conexão com o banco.
        Verificando RDS e logs de conexão. Sem resolução ainda.

[14:55] Causa identificada: pool de conexões esgotado após deploy das 14:20.
        Executando rollback agora.

[15:03] Rollback concluído. Serviço restaurado. Monitorando por 10 minutos.

[15:13] Serviço estável. Incidente encerrado. Postmortem será agendado.
```

Comunicação clara reduz ansiedade do time e dos stakeholders e evita que outras pessoas comecem a investigar o mesmo problema em paralelo.

---

## Quando escalar

Escale para outro membro do time quando:
- Você investigou por 15-20 minutos sem identificar a causa
- O impacto é P1 e você precisa de mais pares de olhos
- A causa raiz está em uma área que você não domina
- Você precisa executar uma ação de alto risco e quer uma segunda opinião

Escalar não é fraqueza — é responsabilidade. Em incidentes críticos, dois engenheiros investigando juntos são muito mais eficientes do que um investigando sozinho.

📄 [`oncall-readiness/escalation-policies.md`](escalation-policies)

---

## Referências

📄 [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow)
📄 [`incident-management/incident-process.md`](../incident-management/incident-process)
📄 [`incident-management/postmortem-process.md`](../incident-management/postmortem-process)
📄 [`oncall-readiness/escalation-policies.md`](escalation-policies)
📄 [`oncall-readiness/oncall-readiness-checklist.md`](oncall-readiness-checklist)
