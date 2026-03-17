# Checklist de Prontidão para On-Call

## 📑 Índice

- [Ferramentas e acessos](#ferramentas-e-acessos)
- [Conhecimento da plataforma](#conhecimento-da-plataforma)
- [Observabilidade](#observabilidade)
- [Kubernetes](#kubernetes)
- [Deploy e rollback](#deploy-e-rollback)
- [Processo de incidente](#processo-de-incidente)
- [Avaliação de prontidão](#avaliação-de-prontidão)

---

Entrar na rotação de on-call é uma responsabilidade significativa. Significa que, durante o seu período de plantão, você é a primeira linha de resposta para qualquer incidente em produção — a qualquer hora do dia ou da noite.

Este checklist existe para garantir que você tenha o conhecimento e as ferramentas necessárias antes de assumir essa responsabilidade. Não é uma formalidade — é uma proteção para você e para a plataforma.

O mentor (fellow) e o coordenador do time avaliam em conjunto quando o engenheiro está pronto para entrar na rotação. Use este documento como guia de preparação e como base para essa conversa.

---

## Ferramentas e acessos

Antes de qualquer coisa, confirme que você tem acesso a todas as ferramentas necessárias para responder a um incidente:

- [ ] PagerDuty configurado com app mobile instalado e notificações ativas
- [ ] NewRelic acessível via Okta
- [ ] Datadog acessível via Okta
- [ ] Grafana acessível
- [ ] ArgoCD acessível
- [ ] Console AWS com acesso às contas relevantes
- [ ] kubectl configurado e com acesso aos clusters de produção via hotctl
- [ ] GitHub com acesso aos repositórios de infraestrutura e configuração
- [ ] Canal de incidentes no Google Chat configurado e com notificações ativas

---

## Conhecimento da plataforma

### Arquitetura geral

- [ ] Consigo explicar o fluxo completo de uma requisição do usuário até o pod
- [ ] Entendo como as contas AWS estão organizadas e para que serve cada uma
- [ ] Sei identificar qual conta e qual cluster é responsável por um serviço específico
- [ ] Conheço os principais componentes: Cloudflare, ALB, Ingress, EKS, ArgoCD

### Fluxo de deploy

- [ ] Entendo o fluxo completo: commit → GitHub Actions → ECR → ArgoCD → EKS
- [ ] Sei onde verificar o status de um pipeline no GitHub Actions
- [ ] Sei acompanhar o status de sincronização de uma aplicação no ArgoCD
- [ ] Consigo identificar se um deploy recente pode estar relacionado a um incidente

### Base Module

- [ ] Entendo o que o Base Module provisiona automaticamente para uma aplicação
- [ ] Sei onde encontrar o YAML de configuração de uma aplicação
- [ ] Consigo identificar os recursos AWS criados pelo Base Module para um serviço

### ArgoCD

- [ ] Sei verificar o status de saúde de uma aplicação no ArgoCD
- [ ] Consigo executar um sync manual quando necessário
- [ ] Sei executar um rollback via ArgoCD para uma versão anterior

---

## Observabilidade

### NewRelic

- [ ] Sei navegar até o dashboard de uma aplicação específica
- [ ] Consigo identificar aumento de taxa de erro e latência elevada
- [ ] Sei abrir e interpretar um trace distribuído
- [ ] Consigo filtrar logs por serviço e período de tempo
- [ ] Sei interpretar o Apdex score e o que significa quando está baixo

### Datadog

- [ ] Sei verificar uso de CPU e memória dos nodes EKS
- [ ] Consigo identificar containers reiniciando com frequência
- [ ] Sei navegar nos dashboards de infraestrutura do time

### Grafana

- [ ] Sei localizar os dashboards operacionais do time DevOps
- [ ] Consigo interpretar os principais painéis de métricas da plataforma

### Alertas

- [ ] Entendo de onde cada tipo de alerta vem (Pingdom, NewRelic, Datadog, CloudWatch)
- [ ] Sei fazer acknowledge de um alerta no PagerDuty
- [ ] Sei resolver um incidente no PagerDuty após a resolução

---

## Kubernetes

Estes são os comandos que você deve executar com confiança durante um incidente:

### Verificação de estado

```bash
# Listar pods de um namespace
kubectl get pods -n <namespace>

# Ver pods com problema (não Running)
kubectl get pods -n <namespace> | grep -v Running

# Verificar status de um deployment
kubectl get deployment <nome> -n <namespace>

# Ver todos os deployments de um namespace
kubectl get deployments -n <namespace>
```

- [ ] Sei listar pods e identificar os que estão com problema
- [ ] Consigo verificar o status de deployments e replica sets
- [ ] Sei identificar pods em CrashLoopBackOff, OOMKilled e Pending

### Logs e diagnóstico

```bash
# Logs em tempo real
kubectl logs -f <pod-name> -n <namespace>

# Logs do container anterior (pod que reiniciou)
kubectl logs <pod-name> --previous -n <namespace>

# Detalhes completos do pod (inclui eventos)
kubectl describe pod <pod-name> -n <namespace>

# Eventos recentes do namespace
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Uso de recursos
kubectl top pods -n <namespace>
kubectl top nodes
```

- [ ] Sei acessar logs de um pod em tempo real
- [ ] Sei acessar logs de um pod que reiniciou (--previous)
- [ ] Sei usar kubectl describe para investigar um pod com problema
- [ ] Sei verificar eventos do namespace para entender o que aconteceu

### Namespaces e serviços

```bash
# Listar namespaces
kubectl get namespaces

# Listar serviços de um namespace
kubectl get services -n <namespace>

# Verificar ingress
kubectl get ingress -n <namespace>
```

- [ ] Entendo o conceito de namespaces e como os serviços estão organizados
- [ ] Sei verificar serviços e ingress de um namespace

---

## Deploy e rollback

- [ ] Sei identificar o deploy mais recente de uma aplicação via ArgoCD e GitHub Actions
- [ ] Consigo executar rollback via ArgoCD:
  ```bash
  argocd app rollback <app-name>
  ```
- [ ] Consigo executar rollback via kubectl:
  ```bash
  kubectl rollout undo deployment/<nome> -n <namespace>
  ```
- [ ] Sei acompanhar o status de um rollout:
  ```bash
  kubectl rollout status deployment/<nome> -n <namespace>
  ```
- [ ] Sei escalar um deployment manualmente em caso de necessidade:
  ```bash
  kubectl scale deployment/<nome> --replicas=<n> -n <namespace>
  ```

---

## Processo de incidente

- [ ] Conheço o fluxo completo de resposta a incidentes do time
- [ ] Sei quando e como escalar para outros membros do time
- [ ] Sei comunicar o status de um incidente no canal correto
- [ ] Conheço a política de escalação no PagerDuty
- [ ] Li pelo menos 3 postmortems recentes do time

---

## Avaliação de prontidão

Quando você considerar que está pronto, agende uma conversa com seu mentor para revisar este checklist juntos. A avaliação não é uma prova — é uma conversa para garantir que você se sente confiante e que o time se sente confortável com você de plantão.

Itens que você ainda não domina completamente não são bloqueadores automáticos — o mentor vai ajudar a definir se o nível atual é suficiente ou se vale mais um tempo de preparação.
