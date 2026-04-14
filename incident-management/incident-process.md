# Processo de Gerenciamento de Incidentes

## 📑 Índice

- [Fluxo principal](#fluxo-principal)
- [Etapas do processo](#etapas-do-processo)
- [Severidade de incidentes](#severidade-de-incidentes)
- [Princípios do processo](#princípios-do-processo)

---

Um incidente é qualquer evento que cause degradação ou indisponibilidade de um serviço em produção. O processo de gerenciamento de incidentes da Hotmart existe para minimizar o impacto no usuário final, garantir rastreabilidade de todas as ações tomadas e criar aprendizado contínuo a partir de cada evento.

---

## Fluxo principal

```
Produção degrada
         │
         ▼
Ferramentas de monitoramento detectam o problema
(Pingdom, NewRelic, Datadog/Teachable, Prometheus, CloudWatch)
         │
         ▼
PagerDuty cria o incidente e notifica o on-call
         │
         ▼
On-call recebe a notificação (app, SMS, ligação)
         │
         ▼
Triagem: confirmar o problema e avaliar impacto
         │
         ▼
Investigação e resposta
         │  logs, métricas, traces, kubectl
         │  comunicação no canal de incidentes
         ▼
Resolução do problema
         │  rollback, fix forward, scaling, correção de config
         ▼
Encerramento do incidente
         │  comunicação de resolução
         │  ticket atualizado com timeline e ações
         ▼
Postmortem (para incidentes P1 e P2)
         análise de causa raiz, melhorias, prevenção
```

---

## Etapas do processo

### 1. Detecção

O incidente começa quando uma ferramenta de monitoramento detecta uma condição anormal: queda de uptime, aumento de taxa de erro, latência elevada, pod em crash, métrica de infraestrutura fora do threshold, etc.

A detecção pode ser automática (alerta disparado por ferramenta) ou manual (usuário reporta problema, engenheiro identifica durante trabalho operacional).

### 2. Notificação via PagerDuty

O alerta é roteado para o PagerDuty, que cria um incidente e notifica o engenheiro de plantão (on-call) conforme a política de escalação configurada. A notificação chega via app mobile, SMS ou ligação telefônica, dependendo da urgência e do tempo sem resposta.

### 3. Triagem

O on-call recebe a notificação e faz a triagem inicial:

- O problema é real ou é um falso positivo?
- Qual é o impacto? Quantos usuários são afetados?
- Qual é a severidade? (P1, P2, P3...)
- É necessário acionar mais pessoas?

A triagem deve ser rápida: o objetivo é entender o escopo do problema, não resolvê-lo ainda.

### 4. Investigação e resposta

Com o escopo definido, começa a investigação. O on-call usa as ferramentas de observabilidade para identificar a causa raiz: dashboards no Grafana, traces no NewRelic, métricas no Prometheus (ou Datadog para Teachable), logs no CloudWatch, estado dos pods via kubectl.

Durante a investigação, toda ação tomada deve ser comunicada no canal de incidentes e registrada no ticket. Isso garante que outros membros do time que entrem para ajudar tenham contexto imediato.

### 5. Resolução

Com a causa raiz identificada (ou com hipótese suficientemente forte), a ação corretiva é executada: rollback de deploy, scaling de pods, correção de configuração, fix forward com novo deploy, etc.

A resolução é confirmada quando as métricas voltam ao normal e o serviço está operando dentro dos parâmetros esperados.

### 6. Encerramento

O incidente é encerrado no PagerDuty e o ticket é atualizado com:
- Timeline completa das ações tomadas
- Causa raiz identificada (ou hipótese mais provável)
- Ação corretiva aplicada
- Impacto estimado (duração, usuários afetados)

Uma comunicação de resolução é enviada para os stakeholders relevantes.

### 7. Postmortem

Para incidentes de alta severidade (P1 e P2), um postmortem é conduzido nos dias seguintes. O objetivo é aprender com o incidente e evitar recorrência.

📄 [`incident-management/postmortem-process.md`](postmortem-process)

---

## Severidade de incidentes

| Severidade | Descrição | Exemplo |
|---|---|---|
| P1 | Impacto crítico em produção, serviço indisponível | Checkout fora do ar |
| P2 | Degradação significativa, funcionalidade core afetada | Lentidão severa no login |
| P3 | Impacto parcial, workaround disponível | Feature secundária com erro |
| P4 | Impacto mínimo, sem urgência | Alerta de threshold baixo |

---

## Princípios do processo

**Minimize o impacto primeiro.** Antes de entender completamente a causa raiz, avalie se existe uma ação rápida que reduz o impacto (rollback, scaling, roteamento de tráfego). Às vezes é melhor agir rápido e investigar depois.

**Comunique continuamente.** Stakeholders e o time precisam saber o que está acontecendo. Atualize o canal de incidentes a cada 15 minutos enquanto o incidente estiver ativo.

**Documente tudo.** Cada ação tomada, cada hipótese testada, cada comando executado. Essa documentação é essencial para o postmortem e para o aprendizado do time.

**Não trabalhe sozinho em P1/P2.** Acione outros membros do time. Dois pares de olhos investigam mais rápido e reduzem o risco de erro sob pressão.
