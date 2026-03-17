# Jira Workflows

## 📑 Índice

- [Tipos de fluxo de trabalho](#tipos-de-fluxo-de-trabalho)
- [Boards por Product Unit](#boards-por-product-unit)
- [Como navegar nos boards](#como-navegar-nos-boards)
- [Boas práticas com tickets](#boas-práticas-com-tickets)
- [Referências](#referências)

---

O Jira é a ferramenta central de gestão de trabalho do time DevOps da Hotmart. Todo trabalho — seja uma solicitação de suporte, uma melhoria de infraestrutura ou um projeto técnico — é rastreado no Jira. Entender como navegar nos boards e acompanhar tickets é parte essencial do trabalho operacional.

---

## Tipos de fluxo de trabalho

O time DevOps opera em dois fluxos principais no Jira, cada um com propósito e dinâmica diferentes.

### Service Desk

O Service Desk é o canal de suporte para o restante da empresa. Colaboradores de qualquer time podem abrir solicitações para o DevOps através do portal de Service Desk, e essas solicitações aparecem na fila do time para atendimento.

**Tipos de solicitação que chegam pelo Service Desk:**
- Solicitações de acesso a sistemas e ferramentas
- Problemas com infraestrutura que afetam times de produto
- Solicitações de configuração (novos ambientes, ajustes de pipeline, etc.)
- Suporte técnico interno para times de desenvolvimento

🔗 [Service Desk Queue](https://hotmart.atlassian.net/jira/servicedesk/projects/TICKET/queues)

### DevOps Backlog

O backlog interno do time DevOps contém as tarefas que o próprio time gera: melhorias de infraestrutura, projetos técnicos, débitos técnicos, iniciativas trimestrais e qualquer trabalho que não veio de uma solicitação externa.

**Tipos de tarefa no backlog:**
- Atualizações de versão (EKS, Helm Charts, ferramentas)
- Melhorias de observabilidade e alertas
- Automações de tarefas operacionais
- Projetos estruturais da plataforma
- Otimizações de custo AWS

🔗 [DevOps Backlog](https://hotmart.atlassian.net/browse/DEVOPS)

---

## Boards por Product Unit

Além dos boards centrais do time DevOps, cada Product Unit (PU) possui seus próprios boards no Jira para rastrear demandas específicas do seu domínio. O time DevOps interage com esses boards quando há trabalho de infraestrutura relacionado a uma PU específica.

Exemplos de Product Units com boards próprios:
- Content Flow
- Teachable
- Financial
- Platform Engineering

Quando uma demanda de infraestrutura vem de uma PU específica, ela pode aparecer tanto no board da PU quanto no Service Desk ou backlog DevOps, dependendo de como foi originada.

---

## Como navegar nos boards

### Service Desk Queue

Na fila do Service Desk, os tickets são organizados por status e prioridade. Os principais status são:

| Status | Significado |
|---|---|
| `Aguardando triagem` | Ticket recém-aberto, ainda não analisado |
| `Em andamento` | Alguém do time está trabalhando nele |
| `Aguardando solicitante` | Precisamos de mais informação do usuário |
| `Resolvido` | Tarefa concluída |

Para pegar um ticket da fila, atribua-o a você mesmo antes de começar a trabalhar. Isso evita que duas pessoas trabalhem no mesmo ticket simultaneamente.

### DevOps Backlog

O backlog segue o fluxo padrão de Scrum/Kanban:

```
Backlog → To Do → In Progress → In Review → Done
```

Tickets no backlog têm estimativas de esforço, épicos associados e critérios de aceitação. Antes de começar uma tarefa, leia os critérios de aceitação com atenção para entender o que significa "concluído".

---

## Boas práticas com tickets

- **Sempre atualize o status** do ticket conforme você avança. Um ticket parado em "In Progress" por dias sem atualização gera ruído e confusão.
- **Adicione comentários** quando houver progresso relevante, bloqueios ou decisões tomadas. O histórico do ticket é valioso.
- **Não feche um ticket sem evidência.** Inclua um comentário com o que foi feito, links para PRs ou comandos executados.
- **Se travar, comente no ticket** explicando o bloqueio e peça ajuda. Não deixe o ticket parado em silêncio.
- **Vincule PRs ao ticket.** Quando abrir um Pull Request relacionado a um ticket, adicione o link no ticket e vice-versa.

---

## Referências

📄 [`operations/service-desk-tickets.md`](./service-desk-tickets.md)
📄 [`operations/operational-tasks.md`](./operational-tasks.md)
📄 [`operations/smart-contract.md`](./smart-contract.md)
