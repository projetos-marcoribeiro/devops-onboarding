# PagerDuty e On-Call

## 📑 Índice

- [O que o PagerDuty faz na plataforma](#o-que-o-pagerduty-faz-na-plataforma)
- [Escalation Policy](#escalation-policy)
- [Fontes de acionamento](#fontes-de-acionamento)
- [Como usar o PagerDuty no dia a dia](#como-usar-o-pagerduty-no-dia-a-dia)
- [Preparação para o on-call](#preparação-para-o-on-call)
- [Referências](#referências)

---

O PagerDuty é a plataforma de gerenciamento de alertas e on-call utilizada na Hotmart. Ele é o ponto central de convergência de todos os alertas de monitoramento e o mecanismo que garante que a pessoa certa seja notificada quando algo dá errado em produção.

---

## O que o PagerDuty faz na plataforma

### Gerenciamento de alertas

O PagerDuty recebe alertas de múltiplas fontes (Pingdom, NewRelic, Datadog, CloudWatch) e os consolida em um único lugar. Isso evita que o on-call precise monitorar múltiplas ferramentas simultaneamente — o PagerDuty agrega e notifica.

Alertas podem ser agrupados em incidentes para evitar flood de notificações quando múltiplos alertas relacionados disparam ao mesmo tempo.

### Notificação de incidentes

Quando um alerta dispara, o PagerDuty notifica o engenheiro de plantão através de múltiplos canais, em ordem de urgência:

```
Alerta dispara no PagerDuty
         │
         ▼
Notificação via app mobile (push notification)
         │  sem resposta em X minutos
         ▼
Notificação via SMS
         │  sem resposta em X minutos
         ▼
Ligação telefônica
         │  sem resposta
         ▼
Escalação para o próximo nível
```

O engenheiro deve fazer o **acknowledge** do alerta no PagerDuty assim que receber a notificação, mesmo que ainda não tenha começado a investigar. O acknowledge sinaliza que alguém está ciente do problema e evita escalação desnecessária.

### Rotação de plantão (on-call)

O PagerDuty gerencia a rotação de on-call do time. A rotação define quem é o engenheiro primário de plantão em cada período, garantindo cobertura 24/7 sem sobrecarregar ninguém.

A rotação é configurada com antecedência e todos os membros do time podem visualizar quem está de plantão em cada momento.

### Escalação automática

Se o on-call primário não responder dentro do tempo configurado, o PagerDuty escala automaticamente para o próximo nível da política de escalação.

---

## Escalation Policy

A escalation policy define a cadeia de notificação para cada tipo de incidente. A estrutura típica na Hotmart segue quatro camadas:

```
Nível 1 — On-call primário
         │  engenheiro de plantão no momento
         │  sem resposta em 5 minutos
         ▼
Nível 2 — Segundo engenheiro
         │  outro membro do time DevOps
         │  sem resposta em 10 minutos
         ▼
Nível 3 — Liderança técnica
         │  tech lead ou engenheiro sênior
         │  sem resposta em 15 minutos
         ▼
Nível 4 — Último recurso
         │  gestão ou time de suporte crítico
```

A escalação automática existe como rede de segurança — não como punição. Se você está de plantão e não consegue responder (problema de conectividade, emergência pessoal), o sistema garante que alguém será notificado.

---

## Fontes de acionamento

Incidentes no PagerDuty podem ser acionados por diferentes fontes de monitoramento:

### Monitoramento de aplicações

- **NewRelic:** alertas de taxa de erro acima do threshold, latência elevada, Apdex score abaixo do mínimo, anomalias detectadas por IA
- **Statping:** verificações de disponibilidade de endpoints internos

### Métricas de infraestrutura

- **Datadog:** CPU ou memória de nodes acima do limite, containers reiniciando com frequência, disco cheio, problemas de rede
- **CloudWatch:** métricas de serviços AWS (RDS, SQS, Lambda, ALB) fora dos parâmetros normais

### Verificações de disponibilidade

- **Pingdom:** endpoint externo indisponível ou com tempo de resposta acima do threshold, verificado de múltiplas regiões geográficas

---

## Como usar o PagerDuty no dia a dia

### Acessando o PagerDuty

O acesso ao PagerDuty é feito via SSO pelo Okta. Instale também o app mobile do PagerDuty no seu celular — é essencial para receber notificações de on-call.

### Verificando quem está de plantão

```
PagerDuty → On-Call → My On-Call Shifts
```

Você também pode verificar via integração no Google Chat ou Slack, dependendo da configuração do time.

### Fazendo acknowledge de um alerta

Quando receber uma notificação, faça o acknowledge imediatamente pelo app mobile ou pela interface web. Isso para a escalação automática e sinaliza que você está ciente.

### Resolvendo um incidente

Após resolver o problema, marque o incidente como **Resolved** no PagerDuty e adicione uma nota com o resumo do que foi feito. Isso fecha o ciclo e registra a resolução.

---

## Preparação para o on-call

Antes de entrar na rotação de on-call, o engenheiro deve completar o checklist de prontidão. Entrar de plantão sem estar preparado é arriscado para você e para a plataforma.

📄 [`oncall-readiness/oncall-readiness-checklist.md`](../oncall-readiness/oncall-readiness-checklist)

---

## Referências

📄 [`incident-management/incident-process.md`](incident-process)
📄 [`incident-management/alert-sources.md`](alert-sources)
📄 [`oncall-readiness/escalation-policies.md`](../oncall-readiness/escalation-policies)
📄 [`platform-overview/observability-stack.md`](../platform-overview/observability-stack)
