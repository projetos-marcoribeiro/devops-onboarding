# Alertas e On-Call

## 📑 Índice

- [Migração: PagerDuty para JiraOps](#migração-pagerduty-para-jiraops)
- [Como funciona o JiraOps](#como-funciona-o-jiraops)
- [Fluxo de alertas via Grafana](#fluxo-de-alertas-via-grafana)
- [Escalation Policy](#escalation-policy)
- [Fontes de acionamento](#fontes-de-acionamento)
- [Como responder a um alerta](#como-responder-a-um-alerta)
- [Preparação para o on-call](#preparação-para-o-on-call)
- [Referências](#referências)

---

A gestão de alertas e on-call da Hotmart está em processo de migração do PagerDuty para o JiraOps (Jira Service Management Operations). Este documento descreve o novo modelo e o que muda na prática.

---

## Migração: PagerDuty para JiraOps

O PagerDuty foi a plataforma de alertas e on-call da Hotmart por vários anos. A migração para o JiraOps centraliza a gestão de incidentes dentro do ecossistema Atlassian (Jira), eliminando a dependência de uma ferramenta externa.

O que já foi migrado:
- Os módulos base (terraform-base-module, terraform-base-module-lambda, terraform-base-module-nomad) já removeram a integração com o PagerDuty
- O módulo de monitoramento (monitoring, pyhot) já foi atualizado para usar JiraOps
- Os módulos de infraestrutura (terraform-modules-aws-rds, terraform-modules-aws-sqs, terraform-modules-aws-amq, terraform-modules-aws-documentdb) já foram atualizados
- Os alertas de componentes EKS (ARC, CoreDNS, Karpenter) já são roteados via Grafana para o JiraOps

O repositório [pagerduty-iac](https://github.com/Hotmart-Org/pagerduty-iac) ainda existe como legado durante a transição.

---

## Como funciona o JiraOps

O JiraOps (Jira Service Management Operations) integra a gestão de incidentes diretamente com o Jira Service Management. Os alertas são recebidos via Grafana e abrem incidentes automaticamente no Jira.

### Contact Points no Grafana

O Grafana é configurado com contact points que definem para onde os alertas são enviados:

- `devops-jiraops`: integração com JiraOps para abertura automática de incidentes (prioridade padrão P3)
- `devops-notification`: envio de alertas para o Google Chat "DevOps Notification"

Contact points adicionais podem ser configurados para chats específicos de PUs ou integrações com o Jira, no arquivo [contact-points.yaml](https://github.com/Hotmart-Org/devops-helm-iac/blob/master/accounts/devops/helm/helmfiles/grafana/config/contact-points.yaml).

### Vantagens do JiraOps

- Centralização: alertas, incidentes e tickets no mesmo ecossistema (Jira)
- Rastreabilidade: cada alerta gera um ticket no Jira Service Management com histórico completo
- Integração nativa com o fluxo de trabalho do time (sprints, boards, SLAs)
- Redução de custos: elimina a assinatura do PagerDuty

---

## Fluxo de alertas via Grafana

```
Aplicação em produção
         │
         ├── Métricas ──────► Prometheus ──► Grafana
         ├── APM ───────────► NewRelic
         ├── Infraestrutura ► CloudWatch
         │
         ▼
Grafana avalia regras de alerta
         │
         ├── Alerta disparado
         │
         ▼
Contact Points do Grafana
         │
         ├── devops-jiraops ──► JiraOps abre incidente no Jira
         │                      (prioridade P3 por padrão)
         │
         └── devops-notification ──► Google Chat do time
```

---

## Escalation Policy

A política de escalação define quem é notificado e em qual ordem quando um incidente é aberto:

```
Dias úteis (08:00-18:00):
  Alertas notificam o email do time DevOps

Dias úteis (18:00-08:00):
  Alertas são escalados para o gestor

Finais de semana (08:00-18:00):
  O plantonista recebe os alertas

Finais de semana (18:00-08:00):
  Alertas são escalados para o gestor
```

A escala de plantão é definida como código no repositório [pagerduty-iac](https://github.com/Hotmart-Org/pagerduty-iac) (legado, em migração).

> Feriados não são cobertos automaticamente pela escalation policy. O plantão em feriados é lançado manualmente.

---

## Fontes de acionamento

Incidentes podem ser acionados por diferentes fontes de monitoramento:

| Fonte | Tipo de alerta |
|---|---|
| NewRelic | Taxa de erro, latência elevada, Apdex score baixo, anomalias |
| Prometheus/Grafana | Métricas de infraestrutura (Karpenter, ARC, CoreDNS, cluster metrics) |
| Datadog (Teachable) | CPU/memória de nodes, containers reiniciando |
| CloudWatch | Métricas de serviços AWS (RDS, SQS, Lambda, ALB) |
| Pingdom | Endpoint externo indisponível ou lento |
| Statping | Disponibilidade de endpoints internos |
| Desenvolvedores | Problemas reportados via Google Chat nos grupos das PUs |

---

## Como responder a um alerta

1. Verifique o incidente no Jira Service Management
2. Confirme se o problema é real ou falso positivo
3. Comunique no canal de incidentes do Google Chat que está investigando
4. Use as ferramentas de observabilidade para investigar (Grafana, NewRelic, kubectl)
5. Execute a ação corretiva (rollback, scaling, correção de config)
6. Atualize o ticket no Jira com a timeline e a resolução
7. Para incidentes P1/P2, um postmortem será conduzido nos dias seguintes

---

## Preparação para o on-call

Antes de entrar na rotação de on-call, complete o checklist de prontidão. O plantão ocorre nos finais de semana e é atribuído a engenheiros com mais experiência na plataforma. Durante o onboarding, o foco é acompanhar incidentes como observador.

📄 [`oncall-readiness/oncall-readiness-checklist.md`](../oncall-readiness/oncall-readiness-checklist)

---

## Referências

📄 [`incident-management/incident-process.md`](incident-process)
📄 [`incident-management/alert-sources.md`](alert-sources)
📄 [`oncall-readiness/escalation-policies.md`](../oncall-readiness/escalation-policies)
📄 [`platform-overview/observability-stack.md`](../platform-overview/observability-stack)
