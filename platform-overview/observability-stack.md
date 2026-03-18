# Stack de Observabilidade

## 📑 Índice

- [Visão geral da stack](#visão-geral-da-stack)
- [Ferramentas da stack](#ferramentas-da-stack)
- [Integração automática via base-module](#integração-automática-via-base-module)
- [Como usar a stack no dia a dia](#como-usar-a-stack-no-dia-a-dia)
- [Referências](#referências)

---

A observabilidade é um pilar fundamental da plataforma Hotmart. O objetivo é que qualquer problema em produção seja detectado, investigado e resolvido com base em dados — não em suposições.

A stack de observabilidade da Hotmart cobre os três pilares clássicos: métricas, logs e traces. Cada ferramenta tem um papel específico e elas se complementam.

---

## Visão geral da stack

```
Aplicação em produção
         │
         ├── Métricas ──────► Prometheus ──► Grafana (visualização)
         │                                ──► Alertmanager (alertas internos)
         │
         ├── APM / Traces ──► NewRelic (performance de aplicação)
         │
         ├── Infraestrutura ► Datadog (hosts, containers, rede)
         │
         ├── Logs ──────────► CloudWatch (logs AWS)
         │                ──► NewRelic Logs (correlação com APM)
         │
         ├── Uptime ────────► Pingdom (monitoramento externo)
         │
         └── Alertas / On-call ► PagerDuty (notificações e escalação)
```

---

## Ferramentas da stack

### NewRelic — APM e Observabilidade de Aplicação

O NewRelic é a principal ferramenta de APM (Application Performance Monitoring) da Hotmart. Ele fornece visibilidade sobre o comportamento interno das aplicações: tempo de resposta, taxa de erros, traces distribuídos, performance de queries de banco de dados e muito mais.

**Casos de uso:**
- Investigar lentidão em uma API específica
- Identificar queries de banco de dados lentas
- Rastrear uma requisição através de múltiplos serviços (distributed tracing)
- Analisar erros e exceções em produção
- Monitorar SLOs (Service Level Objectives) de aplicações

O agente NewRelic é injetado automaticamente nas aplicações deployadas via base-module.

---

### Datadog — Infraestrutura e Containers

O Datadog é utilizado para monitoramento de infraestrutura: nodes Kubernetes, containers, uso de CPU/memória, rede e métricas de sistema.

**Casos de uso:**
- Monitorar saúde dos nodes EKS
- Acompanhar uso de recursos dos pods
- Investigar problemas de rede entre serviços
- Monitorar métricas de sistema dos runners de CI/CD
- Dashboards de capacidade e utilização de infraestrutura

---

### Prometheus — Métricas Internas

O Prometheus coleta métricas expostas pelas aplicações e pelos componentes da plataforma (Kubernetes, Karpenter, ArgoCD, etc.). É a base para alertas internos e dashboards operacionais no Grafana.

**Casos de uso:**
- Métricas customizadas expostas pelas aplicações
- Métricas do Kubernetes (pods, deployments, nodes)
- Alertas baseados em thresholds de métricas
- Base para SLOs e SLAs internos

---

### Grafana — Visualização

O Grafana é a ferramenta de visualização central. Ele agrega dados do Prometheus, Datadog e outras fontes em dashboards unificados.

**Casos de uso:**
- Dashboards operacionais do time DevOps
- Visualização de métricas de aplicações
- Acompanhamento de SLOs em tempo real
- Dashboards de capacidade de infraestrutura

---

### Pingdom — Uptime Externo

O Pingdom monitora a disponibilidade das aplicações a partir de múltiplas regiões geográficas, simulando o comportamento de um usuário real.

**Casos de uso:**
- Detectar indisponibilidade antes que usuários reportem
- Medir tempo de resposta de endpoints críticos a partir de diferentes regiões
- Alertas de uptime integrados ao PagerDuty

---

### PagerDuty — Alertas e On-call

O PagerDuty é a plataforma de gerenciamento de alertas e on-call. Alertas do Prometheus, NewRelic, Datadog e Pingdom são roteados para o PagerDuty, que gerencia a escalação e notificação das pessoas certas.

**Casos de uso:**
- Receber alertas de incidentes em produção
- Gerenciar a rotação de on-call do time
- Escalar incidentes para o nível correto
- Registrar e acompanhar o histórico de incidentes

---

### CloudWatch — Métricas AWS

O CloudWatch é o serviço nativo de monitoramento da AWS. É utilizado principalmente para métricas de serviços AWS gerenciados (RDS, SQS, Lambda, ALB, etc.) e para logs de serviços que não têm integração direta com as outras ferramentas.

---

## Integração automática via base-module

Uma das vantagens centrais do base-module é que toda aplicação deployada através dele recebe configurações de observabilidade automaticamente, sem que o time de produto precise configurar nada manualmente.

O que é provisionado automaticamente:

```
Base-module detecta monitoring: enabled: true
         │
         ├── Configura injeção do agente NewRelic
         ├── Registra a aplicação no Datadog
         ├── Cria ServiceMonitor para o Prometheus
         ├── Cria dashboard padrão no Grafana
         ├── Configura alertas básicos no PagerDuty
         └── Configura check no Pingdom (se ingress habilitado)
```

---

## Como usar a stack no dia a dia

Durante o troubleshooting de um incidente, o fluxo típico é:

1. **PagerDuty** dispara o alerta — você recebe a notificação
2. **Grafana** — visão geral do que está acontecendo com métricas
3. **NewRelic** — investigar o comportamento da aplicação, traces e erros
4. **Datadog** — verificar saúde da infraestrutura (nodes, containers)
5. **CloudWatch** — verificar logs e métricas de serviços AWS envolvidos
6. **kubectl** — inspecionar pods, eventos e logs diretamente no cluster

Cada ferramenta tem seu papel. Saber qual usar em cada situação é uma habilidade que se desenvolve com a prática.

---

## Referências

📄 [`platform-overview/platform-architecture.md`](platform-architecture)
📄 [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow)
📄 [`incident-management/alert-sources.md`](../incident-management/alert-sources)
📄 [`oncall-readiness/incident-investigation-guide.md`](../oncall-readiness/incident-investigation-guide)
