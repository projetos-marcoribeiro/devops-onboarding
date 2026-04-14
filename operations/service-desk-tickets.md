# Service Desk Tickets

## 📑 Índice

- [Para que o Service Desk é usado](#para-que-o-service-desk-é-usado)
- [Fluxo de atendimento](#fluxo-de-atendimento)
- [Exemplos de tickets comuns](#exemplos-de-tickets-comuns)
- [Como atender bem um ticket](#como-atender-bem-um-ticket)
- [SLA de atendimento](#sla-de-atendimento)

---

O Service Desk é o canal oficial de suporte do time DevOps para o restante da empresa. É por aqui que chegam solicitações de acesso, problemas de infraestrutura, pedidos de configuração e suporte técnico interno.

🔗 [https://hotmart.atlassian.net/servicedesk](https://hotmart.atlassian.net/servicedesk)

---

## Para que o Service Desk é usado

O Service Desk centraliza todas as demandas externas ao time DevOps, garantindo rastreabilidade e evitando que solicitações se percam em mensagens de chat ou e-mail.

**Tipos de solicitação que chegam pelo Service Desk:**

- **Solicitações de acesso**: pedidos de acesso a sistemas, ferramentas, contas AWS, clusters Kubernetes, etc.
- **Suporte técnico interno**: times de produto com problemas em pipelines, deploys ou infraestrutura
- **Problemas com infraestrutura**: indisponibilidade de serviços, erros em ambientes de staging/produção
- **Solicitações de configuração**: novos ambientes, ajustes em pipelines, criação de recursos, mudanças de configuração

---

## Fluxo de atendimento

```
Usuário abre ticket no Service Desk
         │
         ▼
Ticket aparece na fila do DevOps
(https://hotmart.atlassian.net/jira/servicedesk/projects/TICKET/queues)
         │
         ▼
DevOps faz triagem da solicitação
         │  verifica se está claro, se tem informações suficientes
         │  prioriza conforme urgência e impacto
         ▼
DevOps analisa e executa a tarefa
         │  atualiza o ticket com progresso
         │  solicita informações adicionais se necessário
         ▼
Ticket resolvido
         │  comentário com o que foi feito
         │  evidências quando aplicável
         ▼
Usuário é notificado automaticamente
```

---

## Exemplos de tickets comuns

### Solicitação de acesso

```
Título: Acesso ao AWS Console: conta hotpay
Descrição: Preciso de acesso ReadOnly à conta AWS hotpay para
           acompanhar métricas de infraestrutura do produto.
           Referência: pode espelhar o acesso do usuário [colega].
Prioridade: Normal
```

**Como atender:** verificar se a solicitação está completa, validar com o gestor se necessário, provisionar o acesso via IAM/SSO e confirmar no ticket.

### Problema em pipeline de CI/CD

```
Título: Pipeline falhando no build da imagem Docker: repositório payments-api
Descrição: Desde ontem o pipeline está falhando no step de build.
           Erro: "no space left on device" no runner.
           Link do workflow com falha: [link]
Prioridade: Alta
```

**Como atender:** verificar os logs do runner, identificar a causa (disco cheio, problema de configuração, etc.), corrigir e confirmar que o pipeline voltou a funcionar.

### Solicitação de novo ambiente

```
Título: Criação de ambiente de staging para o projeto X
Descrição: O time de produto Y precisa de um ambiente de staging
           separado para o projeto X. Precisamos de:
           - Namespace no cluster de staging
           - Pipeline de deploy configurado
           - Variáveis de ambiente configuradas
Prioridade: Normal
```

**Como atender:** verificar se há um YAML de Base Module para o projeto, criar o namespace, configurar o pipeline e validar o deploy.

### Ajuste de recursos de um pod

```
Título: Pod da aplicação Z reiniciando por OOMKilled
Descrição: O pod da aplicação Z está sendo morto por falta de memória.
           Namespace: payments / Deployment: payments-worker
           Logs: [link para logs no NewRelic]
Prioridade: Alta
```

**Como atender:** verificar o uso real de memória no Grafana/Datadog, ajustar os limits no Helm values, abrir PR e fazer deploy.

---

## Como atender bem um ticket

**Leia com atenção antes de agir.** Muitos tickets parecem simples mas têm detalhes importantes. Entenda o que está sendo pedido antes de começar.

**Peça informações quando necessário.** Se o ticket não tem informações suficientes para você agir, comente pedindo o que falta. Não tente adivinhar.

**Comunique o progresso.** Atualize o ticket com o que está sendo feito. O solicitante não sabe o que está acontecendo se você não comunicar.

**Documente o que foi feito.** Ao resolver, adicione um comentário explicando o que foi feito. Isso é útil para o histórico e para outros membros do time que possam ter dúvidas similares no futuro.

**Não resolva fora do ticket.** Se alguém te pedir algo no chat, peça para abrir um ticket. Isso garante rastreabilidade e evita que o trabalho se perca.

---

## SLA de atendimento

Os tickets têm SLAs definidos conforme a prioridade. Consulte o Smart Contract do time para os SLAs atualizados.

📄 [`operations/smart-contract.md`](smart-contract)

Como regra geral:
- Tickets de alta prioridade (impacto em produção) devem ser triados imediatamente
- Tickets normais devem ser triados em até 1 dia útil
- Tickets de baixa prioridade entram no backlog e são priorizados conforme capacidade
