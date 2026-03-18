# Solicitando Acessos via Service Desk

## 📑 Índice

- [Por que usar o Service Desk](#por-que-usar-o-service-desk)
- [Passo a passo para solicitar um acesso](#passo-a-passo-para-solicitar-um-acesso)
- [Após submeter a solicitação](#após-submeter-a-solicitação)
- [Tempo de aprovação](#tempo-de-aprovação)
- [Dicas](#dicas)

---

O Service Desk é o portal oficial para solicitação de acessos a sistemas e ferramentas na Hotmart. Todos os acessos devem ser solicitados por aqui — não existe provisionamento manual fora desse fluxo.

URL: [https://hotmart.atlassian.net/servicedesk](https://hotmart.atlassian.net/servicedesk)

---

## Por que usar o Service Desk

Além de ser o processo oficial, usar o Service Desk garante rastreabilidade: toda solicitação fica registrada com data, responsável pela aprovação e justificativa. Isso é importante tanto para auditoria quanto para facilitar a vida de quem vai aprovar o seu acesso.

---

## Passo a passo para solicitar um acesso

### 1. Acessar o portal

Abra o navegador e acesse [https://hotmart.atlassian.net/servicedesk](https://hotmart.atlassian.net/servicedesk).

Faça login com suas credenciais corporativas. Se for seu primeiro acesso ao portal, pode ser necessário aceitar os termos de uso.

### 2. Abrir uma solicitação de acesso

Na tela inicial, procure pela categoria de solicitações relacionadas a acessos. O nome pode variar, mas geralmente está em algo como **"Acesso a Sistemas"** ou **"Access Request"**.

Clique em **"Criar solicitação"** ou **"New Request"**.

### 3. Selecionar o sistema

No formulário que abrir, selecione o sistema ou ferramenta para o qual você está solicitando acesso. Exemplos:

- AWS
- JIRA
- Vault
- SonarQube
- GitHub

Se o sistema que você precisa não aparecer na lista, procure por uma opção genérica como "Outro sistema" e descreva no campo de detalhes.

### 4. Escolher o perfil de acesso

Muitos sistemas têm diferentes níveis de acesso (perfis). Selecione o perfil adequado para sua função. Em caso de dúvida, consulte seu mentor ou tech lead antes de submeter a solicitação.

Exemplos de perfis comuns:
- `ReadOnly` — apenas leitura, sem permissão para modificar recursos
- `Developer` — acesso padrão para desenvolvimento
- `DevOps` — acesso operacional para times de infraestrutura

### 5. Justificar o acesso

Este campo é obrigatório e importante. Uma boa justificativa acelera a aprovação. Explique:

- Quem você é e qual é o seu time
- Para que você precisa do acesso
- Referência a um colega cujo acesso pode ser espelhado (quando aplicável)

Veja exemplos de justificativas no documento [`access-requests-examples.md`](access-requests-examples).

---

## Após submeter a solicitação

Você receberá uma confirmação por e-mail com o número do ticket. Guarde esse número para acompanhar o status.

O fluxo de aprovação geralmente envolve:

1. Triagem automática pelo sistema
2. Aprovação do gestor direto
3. Aprovação do responsável pelo sistema (quando necessário)
4. Provisionamento do acesso

---

## Tempo de aprovação

| Tipo de acesso | Tempo estimado |
|---|---|
| Acessos padrão (ReadOnly, Developer) | 1 dia útil |
| Acessos com permissões elevadas | 2 dias úteis |
| Acessos a sistemas críticos (produção, Vault) | 2–3 dias úteis |

Se o prazo passar sem resposta, você pode adicionar um comentário no ticket solicitando atualização de status.

---

## Dicas

- Solicite todos os acessos necessários de uma vez, logo no primeiro dia. Isso evita ficar bloqueado por falta de acesso durante o onboarding.
- Consulte o documento [`access-requests-examples.md`](access-requests-examples) para ver exemplos de solicitações já preenchidas.
- Se tiver dúvida sobre qual perfil solicitar, pergunte ao seu mentor antes de submeter.
