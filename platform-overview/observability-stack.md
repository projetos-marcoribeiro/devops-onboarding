# Stack de Observabilidade

## 📑 Índice

- [Visão geral da stack](#visão-geral-da-stack)
- [Ferramentas da stack](#ferramentas-da-stack)
- [Integração automática via base-module](#integração-automática-via-base-module)
- [Como usar a stack no dia a dia](#como-usar-a-stack-no-dia-a-dia)
- [Referências](#referências)

---

A observabilidade é um pilar fundamental da plataforma Hotmart. O objetivo é que qualquer problema em produção seja detectado, investigado e resolvido com base em dados: não em suposições.

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
         ├── Error Tracking ► Sentry (erros e exceções)
         │
         ├── Infraestrutura ► Prometheus + Grafana (métricas de cluster)
         │                  ► Datadog (apenas Teachable)
         │
         ├── Custos ────────► Kubecost (custos Kubernetes)
         │
         ├── Logs ──────────► CloudWatch (logs AWS)
         │                ──► NewRelic Logs (correlação com APM)
         │
         ├── Uptime ────────► Pingdom (monitoramento externo)
         │
         └── Alertas / On-call ► JiraOps (incidentes via Jira Service Management)
                              ► PagerDuty (legado, em migração)
```

---

## Ferramentas da stack

### NewRelic: APM e Observabilidade de Aplicação

O NewRelic é a principal ferramenta de APM (Application Performance Monitoring) da Hotmart. Ele fornece visibilidade sobre o comportamento interno das aplicações: tempo de resposta, taxa de erros, traces distribuídos, performance de queries de banco de dados e muito mais.

**Casos de uso:**
- Investigar lentidão em uma API específica
- Identificar queries de banco de dados lentas
- Rastrear uma requisição através de múltiplos serviços (distributed tracing)
- Analisar erros e exceções em produção
- Monitorar SLOs (Service Level Objectives) de aplicações

O agente NewRelic é injetado automaticamente nas aplicações deployadas via base-module.

---

### Datadog: Infraestrutura – Teachable ⚠️ Escopo limitado

> ⚠️ O Datadog é utilizado exclusivamente pela Product Unit **Teachable**. Está sendo removido da plataforma principal (Pyhot / Monitoring / Base-Module). Para as demais PUs, métricas de infraestrutura são coletadas via Prometheus e visualizadas no Grafana.

O Datadog monitora a camada de infraestrutura da Teachable: nodes Kubernetes, containers, uso de CPU/memória, rede e métricas de sistema.

**Casos de uso (Teachable):**
- Monitorar saúde dos nodes EKS da Teachable
- Acompanhar uso de recursos dos pods
- Investigar problemas de rede entre serviços
- Dashboards de capacidade e utilização de infraestrutura

---

### Prometheus: Métricas Internas

O Prometheus coleta métricas expostas pelas aplicações e pelos componentes da plataforma (Kubernetes, Karpenter, ArgoCD, etc.). É a base para alertas internos e dashboards operacionais no Grafana.

**Casos de uso:**
- Métricas customizadas expostas pelas aplicações
- Métricas do Kubernetes (pods, deployments, nodes)
- Alertas baseados em thresholds de métricas
- Base para SLOs e SLAs internos

---

### Grafana: Visualização

O Grafana é a ferramenta de visualização central. Ele agrega dados do Prometheus, CloudWatch e outras fontes em dashboards unificados.

**Casos de uso:**
- Dashboards operacionais do time DevOps
- Visualização de métricas de aplicações
- Acompanhamento de SLOs em tempo real
- Dashboards de capacidade de infraestrutura
- Métricas de Karpenter, ARC e clusters EKS

---

### Sentry: Error Tracking

O Sentry captura erros e exceções em aplicações em tempo real, com contexto completo: stack trace, breadcrumbs, informações do ambiente e do usuário afetado.

**Casos de uso:**
- Rastreamento de erros e exceções em produção
- Análise de stack traces com contexto completo
- Monitoramento de regressões após deploys
- Complementa o NewRelic na camada de error tracking

---

### Kubecost: Monitoramento de Custos

O Kubecost fornece visibilidade sobre os custos reais de workloads em clusters Kubernetes, permitindo identificar desperdícios e otimizar a alocação de recursos.

**Casos de uso:**
- Monitorar custo por namespace, deployment e PU
- Identificar workloads com recursos superprovisionados
- Acompanhar tendências de custo ao longo do tempo
- Gerar relatórios de custo por time ou produto

---

### Pingdom: Uptime Externo

O Pingdom monitora a disponibilidade das aplicações a partir de múltiplas regiões geográficas, simulando o comportamento de um usuário real.

**Casos de uso:**
- Detectar indisponibilidade antes que usuários reportem
- Medir tempo de resposta de endpoints críticos a partir de diferentes regiões
- Alertas de uptime integrados ao JiraOps (via Grafana)

---

### PagerDuty / JiraOps: Alertas e On-call

> ⚠️ O PagerDuty está sendo substituído pelo JiraOps (Jira Service Management Operations). Os alertas agora são roteados via Grafana para o JiraOps, que abre incidentes automaticamente no Jira.

O JiraOps é a plataforma de gerenciamento de alertas e on-call que está substituindo o PagerDuty na Hotmart. Alertas do Prometheus (via Grafana), NewRelic, Pingdom e Datadog (Teachable) são roteados para o JiraOps, que gerencia a abertura de incidentes e a notificação das pessoas certas.

**Casos de uso:**
- Receber alertas de incidentes em produção via Jira Service Management
- Gerenciar a rotação de on-call do time
- Escalar incidentes para o nível correto
- Registrar e acompanhar o histórico de incidentes com rastreabilidade completa no Jira

---

### CloudWatch: Métricas AWS

O CloudWatch é o serviço nativo de monitoramento da AWS. É utilizado principalmente para métricas de serviços AWS gerenciados (RDS, SQS, Lambda, ALB, etc.) e para logs de serviços que não têm integração direta com as outras ferramentas.

---

## Integração automática via base-module

Uma das vantagens centrais do base-module é que toda aplicação deployada através dele recebe configurações de observabilidade automaticamente, sem que o time de produto precise configurar nada manualmente.

O que é provisionado automaticamente:

```
Base-module detecta monitoring: enabled: true
         │
         ├── Configura injeção do agente NewRelic
         ├── Cria ServiceMonitor para o Prometheus
         ├── Cria dashboard padrão no Grafana
         ├── Configura alertas básicos via JiraOps
         └── Configura check no Pingdom (se ingress habilitado)
```

> ⚠️ O registro automático no Datadog foi removido do Base Module. O Datadog é configurado separadamente apenas para aplicações da PU Teachable.

---

## Como usar a stack no dia a dia

Durante o troubleshooting de um incidente, o fluxo típico é:

1. **JiraOps** dispara o alerta: você recebe a notificação via Jira Service Management
2. **Grafana**: visão geral do que está acontecendo com métricas
3. **NewRelic**: investigar o comportamento da aplicação, traces e erros
4. **Datadog** *(apenas Teachable)*: verificar saúde da infraestrutura (nodes, containers)
5. **Kubecost**: verificar custos e alocação de recursos se relevante
6. **CloudWatch**: verificar logs e métricas de serviços AWS envolvidos
7. **kubectl**: inspecionar pods, eventos e logs diretamente no cluster

Cada ferramenta tem seu papel. Saber qual usar em cada situação é uma habilidade que se desenvolve com a prática.

---

## Referências

📄 [`platform-overview/platform-architecture.md`](platform-architecture)
📄 [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow)
📄 [`incident-management/alert-sources.md`](../incident-management/alert-sources)
📄 [`oncall-readiness/incident-investigation-guide.md`](../oncall-readiness/incident-investigation-guide)
