# Comunicação do Time

A comunicação eficiente é tão importante quanto o conhecimento técnico no trabalho DevOps. Durante um incidente, uma informação mal comunicada pode custar minutos preciosos. No dia a dia, a falta de contexto compartilhado gera retrabalho e decisões ruins.

Este documento descreve como a comunicação acontece dentro do time DevOps da Hotmart e as boas práticas que o time segue.

---

## Ferramentas de comunicação

### Google Chat

O Google Chat é a principal ferramenta de comunicação em tempo real do time. É onde acontecem as conversas do dia a dia, as notificações automáticas de sistema e a coordenação durante incidentes.

**Como é usado:**

- Comunicação rápida entre membros do time
- Canais temáticos por área (incidentes, deploys, alertas, geral)
- Notificações automáticas de deploy via integração com GitHub Actions e ArgoCD
- Alertas de monitoramento integrados (PagerDuty, NewRelic, Datadog)
- Comunicação com outros times da empresa

**Boas práticas no Google Chat:**

- Use o canal correto para cada tipo de mensagem. Mensagens no canal errado se perdem.
- Para solicitações ao time DevOps, use o Service Desk — não mensagem direta. Isso garante rastreabilidade.
- Em incidentes, use o canal de incidentes para comunicar status. Não fragmente a comunicação em DMs.
- Reaja com emoji para confirmar que leu uma mensagem importante. Isso evita que o remetente fique sem saber se a mensagem chegou.

### Jira

O Jira é a ferramenta de gestão de trabalho do time. É onde o trabalho é planejado, rastreado e documentado.

**Como é usado:**

- Acompanhamento de tarefas do backlog DevOps
- Gestão da fila do Service Desk
- Rastreamento de incidentes e postmortems
- Planejamento de sprints e iniciativas trimestrais
- Documentação de decisões e histórico de tickets

**Boas práticas no Jira:**

- Mantenha o status dos seus tickets atualizado. Um ticket parado em "In Progress" por dias sem atualização gera confusão.
- Adicione comentários com progresso relevante, bloqueios e decisões. O histórico do ticket é valioso.
- Vincule PRs e documentos relacionados ao ticket.
- Não resolva um ticket sem evidência do que foi feito.

---

## Comunicação durante incidentes

Durante incidentes e operações críticas, a comunicação rápida e clara é essencial. O time segue um padrão específico para garantir que todos tenham o contexto necessário sem precisar perguntar.

### Canal de incidentes

Todo incidente P1 e P2 tem comunicação centralizada no canal de incidentes do Google Chat. O on-call é responsável por manter o canal atualizado a cada 15 minutos enquanto o incidente estiver ativo.

**Formato de atualização:**

```
[HH:MM] STATUS: <Investigando / Identificado / Resolvendo / Resolvido>
Serviço afetado: <nome do serviço>
Impacto: <descrição do impacto>
Ação atual: <o que está sendo feito agora>
Próxima atualização: <horário>
```

**Exemplo:**

```
[14:35] STATUS: Investigando
Serviço afetado: payments-api
Impacto: Checkout com taxa de erro elevada (~15%). Usuários não conseguem finalizar compras.
Ação atual: Verificando logs e traces no NewRelic. Deploy recente às 14:20 é suspeito.
Próxima atualização: 14:50
```

### Comunicação de resolução

Quando o incidente é resolvido, comunique claramente:

```
[15:08] STATUS: Resolvido
Serviço afetado: payments-api
Duração: ~36 minutos (14:32 - 15:08)
Causa: Deploy das 14:20 introduziu bug de pool de conexões com o banco.
Ação: Rollback executado para a versão anterior.
Próximos passos: Postmortem agendado para amanhã às 10h.
```

---

## Boas práticas gerais de comunicação

### Mantenha o contexto claro

Quando iniciar uma conversa ou abrir um ticket, forneça contexto suficiente para que a outra pessoa entenda o problema sem precisar fazer perguntas básicas:

- O que você está tentando fazer
- O que aconteceu (comportamento observado)
- O que você esperava que acontecesse
- O que você já tentou
- Links relevantes (ticket, PR, log, screenshot)

Uma mensagem com contexto claro resolve em uma troca o que poderia levar cinco.

### Compartilhe informações relevantes proativamente

Se você descobriu algo que pode ser útil para o time — um bug, uma limitação de ferramenta, uma mudança de comportamento de um serviço — compartilhe no canal apropriado sem esperar que alguém pergunte. Informação retida é informação perdida.

### Documente decisões importantes

Decisões técnicas relevantes não devem ficar apenas na memória de quem participou da conversa. Documente no ticket do Jira, no PR, no documento de arquitetura ou no canal do time, conforme o contexto.

Perguntas do tipo "por que fizemos isso dessa forma?" devem ter uma resposta acessível — não depender de quem estava presente na reunião há seis meses.

### Prefira comunicação assíncrona para o que não é urgente

Nem tudo precisa de resposta imediata. Para dúvidas não urgentes, perguntas sobre processos e solicitações que podem esperar, prefira canais assíncronos (Jira, comentários em PR, mensagem no chat sem expectativa de resposta imediata).

Reserve a comunicação síncrona (chamada, mensagem urgente) para situações que realmente precisam de resposta rápida.

### Durante o onboarding

Nos primeiros meses, não hesite em perguntar. O time prefere responder dúvidas a ter um engenheiro travado em silêncio. Perguntar no canal do time (em vez de só para o mentor) também tem o benefício de que outros membros podem contribuir com perspectivas diferentes — e a resposta fica registrada para quem tiver a mesma dúvida no futuro.
