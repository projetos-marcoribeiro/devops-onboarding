# Fontes de Alertas

## 📑 Índice

- [Visão geral das fontes](#visão-geral-das-fontes)
- [Pingdom: Monitoramento Externo de Uptime](#pingdom--monitoramento-externo-de-uptime)
- [Statping: Monitoramento Interno](#statping--monitoramento-interno)
- [NewRelic: APM e Performance de Aplicação](#newrelic--apm-e-performance-de-aplicação)
- [Datadog: Infraestrutura e Containers](#datadog--infraestrutura-e-containers)
- [CloudWatch: Métricas AWS Nativas](#cloudwatch--métricas-aws-nativas)
- [Runbooks associados a alertas](#runbooks-associados-a-alertas)
- [Referências](#referências)

---

Os alertas que chegam ao PagerDuty vêm de múltiplas ferramentas de monitoramento, cada uma cobrindo uma camada diferente da plataforma. Entender de onde cada alerta vem e o que ele significa é fundamental para responder a incidentes com eficiência.

---

## Visão geral das fontes

```
Plataforma Hotmart
         │
         ├── Pingdom ──────────► uptime externo (usuário real)
         ├── Statping ─────────► uptime interno (endpoints internos)
         ├── NewRelic ─────────► APM, performance de aplicação
         ├── Datadog ──────────► infraestrutura, containers, rede
         └── CloudWatch ───────► serviços AWS nativos
                   │
                   ▼
              PagerDuty
         (consolida e notifica o on-call)
```

---

## Pingdom: Monitoramento Externo de Uptime

O Pingdom verifica a disponibilidade dos endpoints públicos da Hotmart a partir de múltiplas regiões geográficas, simulando o comportamento de um usuário real acessando o serviço.

**O que monitora:**
- Disponibilidade de URLs públicas (HTTP/HTTPS)
- Tempo de resposta de endpoints críticos
- Verificações de conteúdo (a página retornou o conteúdo esperado?)

**Quando alerta:**
- Endpoint indisponível (status != 200)
- Tempo de resposta acima do threshold configurado
- Conteúdo da resposta diferente do esperado

**Por que é importante:** o Pingdom detecta problemas do ponto de vista do usuário final. Um serviço pode estar "funcionando" internamente mas inacessível externamente por problema de DNS, Cloudflare ou ALB: o Pingdom captura isso.

---

## Statping: Monitoramento Interno

O Statping realiza verificações de disponibilidade de endpoints internos da plataforma, complementando o Pingdom com visibilidade sobre serviços que não são expostos publicamente.

**O que monitora:**
- Endpoints internos de APIs e serviços
- Verificações de conectividade entre componentes da plataforma

**Quando alerta:**
- Endpoint interno indisponível
- Tempo de resposta fora do esperado

---

## NewRelic: APM e Performance de Aplicação

O NewRelic é a principal fonte de alertas relacionados ao comportamento das aplicações em produção. Ele instrumenta as aplicações e coleta dados de performance em tempo real.

**O que monitora:**

| Métrica | Descrição |
|---|---|
| Taxa de erro | Percentual de requisições com erro (4xx, 5xx) |
| Latência (p95, p99) | Tempo de resposta nos percentis 95 e 99 |
| Apdex score | Índice de satisfação do usuário com a performance |
| Throughput | Volume de requisições por minuto |
| Erros de aplicação | Exceções e erros não tratados no código |

**Quando alerta:**
- Taxa de erro acima do threshold (ex: > 1% por 5 minutos)
- Latência p99 acima do SLO definido
- Apdex score abaixo do mínimo aceitável
- Anomalia detectada pelo motor de IA do NewRelic

**Dica de investigação:** quando um alerta do NewRelic chega, o primeiro passo é abrir o trace distribuído do período do alerta para identificar qual serviço ou operação está causando o problema.

---

## Datadog: Infraestrutura e Containers

O Datadog monitora a camada de infraestrutura: nodes Kubernetes, containers, uso de recursos e rede.

**O que monitora:**

| Métrica | Descrição |
|---|---|
| CPU dos nodes | Uso de CPU dos nodes EKS |
| Memória dos nodes | Uso de memória dos nodes EKS |
| Containers reiniciando | Pods em CrashLoopBackOff ou com restarts frequentes |
| Disco | Uso de disco nos nodes |
| Rede | Latência e erros de rede entre pods |

**Quando alerta:**
- CPU ou memória de node acima de 85% por período sustentado
- Pod reiniciando mais de X vezes em Y minutos
- Disco acima de 80% de utilização
- Métricas de rede fora do padrão

**Dica de investigação:** alertas do Datadog geralmente indicam problema de capacidade ou de configuração de recursos. Verifique `kubectl top nodes` e `kubectl top pods` para confirmar o que o Datadog está reportando.

---

## CloudWatch: Métricas AWS Nativas

O CloudWatch coleta métricas dos serviços AWS gerenciados. É a fonte de alertas para problemas em RDS, SQS, Lambda, ALB e outros serviços que não são diretamente instrumentados pelas outras ferramentas.

**O que monitora:**

| Serviço | Métricas relevantes |
|---|---|
| RDS | CPU, conexões, latência de queries, storage |
| SQS | Profundidade da fila, mensagens antigas, erros |
| Lambda | Erros, throttling, duração de execução |
| ALB | Taxa de erro 5xx, latência, conexões ativas |
| EKS | Métricas do control plane |

**Quando alerta:**
- RDS com CPU acima de 80% ou storage crítico
- Fila SQS com mensagens acumulando (possível consumidor com problema)
- Lambda com taxa de erro elevada ou throttling
- ALB com aumento de erros 5xx

---

## Runbooks associados a alertas

Muitos alertas possuem runbooks associados que descrevem o passo a passo de investigação e as ações corretivas mais comuns para aquele tipo de problema.

Quando um alerta chega no PagerDuty, verifique se há um runbook associado antes de começar a investigar do zero. O runbook pode economizar minutos preciosos durante um incidente.

Os runbooks estão disponíveis na documentação interna do time DevOps. Se você resolver um incidente e não encontrar um runbook para ele, considere criar um: é uma contribuição valiosa para o time.

📄 [`incident-management/troubleshooting-flow.md`](troubleshooting-flow)

---

## Referências

📄 [`incident-management/incident-process.md`](incident-process)
📄 [`incident-management/pagerduty-oncall.md`](pagerduty-oncall)
📄 [`platform-overview/observability-stack.md`](../platform-overview/observability-stack)
