# Dia 1: Passo a Passo

## 📑 Índice

- [Antes de tudo](#antes-de-tudo)
- [Manhã: Kickoff e apresentações](#manhã-kickoff-e-apresentações)
- [Após o Kickoff: Setup de acessos](#após-o-kickoff-setup-de-acessos)
- [Instalação de ferramentas](#instalação-de-ferramentas)
- [Configuração do ambiente](#configuração-do-ambiente)
- [Antes de ir embora](#antes-de-ir-embora)

---

Este documento consolida tudo que precisa ser feito no primeiro dia, na ordem certa. Siga passo a passo sem pular etapas.

---

## Antes de tudo

Certifique-se de que você já passou pelo onboarding geral da Hotmart (normalmente ocorre na primeira semana na empresa). O onboarding DevOps começa depois disso.

---

## Manhã: Kickoff e apresentações

1. Participar da apresentação geral do time DevOps
   - Estrutura e hierarquia do time
   - Escopo de trabalho do DevOps na Hotmart
   - Conhecer seu fellow (mentor)
2. Conhecer sua Product Unit (PU)
   - Cada PU tem um DevOps associado
   - Entender qual é o escopo da sua PU

📊 [Exemplo de apresentação: DevOps Experience - PU Lead Solutions](https://docs.google.com/presentation/d/1OY9C5nSgSHQRzHNgOyXRl8rwey-7Xrdnua_rzQr5GXc/edit?usp=sharing)

---

## Após o Kickoff: Setup de acessos

Abra TODOS os tickets de acesso de uma vez. Não espere um ser aprovado para abrir o próximo.

### Passo 1: Configurar Okta e MFA

O Okta é o portal de acesso a todas as ferramentas. Sem ele, nada funciona.

📄 [Guia de acesso ao Okta](okta-access)

### Passo 2: Abrir tickets no Service Desk

Acesse o [Service Desk: Portal DevOps](https://hotmart.atlassian.net/servicedesk/customer/portal/4/group/315) e abra um ticket para cada acesso abaixo. Use o formulário "Concessão de acesso a sistema".

| # | Acesso | Prioridade |
|---|---|---|
| 1 | AWS: Amazon Q Developer | Alta |
| 2 | AWS: Contas principais (hotmart-org, usefedora, notas-org) com perfil Devops-ReadOnly | Alta |
| 3 | GitHub: Organizações (Hotmart-Org, usefedora, notas-org) | Alta |
| 4 | JIRA: DevOps Components | Alta |
| 5 | 1Password: DevOps Vault | Alta |
| 6 | Vault (HashiCorp): vault_admins | Média |
| 7 | SonarQube: sonar-administrators | Média |
| 8 | New Relic | Média |
| 9 | Spot | Média |
| 10 | Email group: devops-sre@hotmart.com | Alta |

> Os botões com justificativas pré-preenchidas estão no [Pack Completo de Acessos](access-requests-examples). Use-os para agilizar.

> Para o GitHub, crie um usuário corporativo seguindo o padrão: `<email sem ponto>-hotmart` (ex: `marcoribeiro-hotmart`)

### Passo 3: Criar sua Issue de progresso

Abra uma Issue no repositório do onboarding para acompanhar seu progresso. Seu fellow e gestor acompanham por lá.

1. Clique em [Abrir minha Issue](https://github.com/projetos-marcoribeiro/devops-onboarding/issues/new?template=onboarding-checklist.md&labels=onboarding)
2. Atribua a si mesmo no campo Assignees
3. Adicione seu gestor também como Assignee

📄 [Instruções detalhadas](../devops-journey/onboarding-checklist)

---

## Instalação de ferramentas

Instale as ferramentas obrigatórias enquanto aguarda os acessos serem aprovados.

| Ferramenta | Para que serve | Guia |
|---|---|---|
| kubectl | Interagir com clusters Kubernetes | [Instalação](https://kubernetes.io/docs/tasks/tools/) |
| AWS CLI | Interagir com serviços AWS | [Instalação](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) |
| hotctl | CLI interna da Hotmart para EKS e SSO | [Repositório](https://github.com/Hotmart-Org/hotctl) |
| Rancher Desktop | Docker local sem Docker Desktop | [Guia](https://github.com/Hotmart-Org/devops-docs/blob/master/docs/tools/rancher-desktop/rancher-desktop.md) |

📄 [Guia completo de ferramentas](required-tools)

---

## Configuração do ambiente

Quando os acessos AWS e GitHub forem aprovados:

### Configurar hotctl e acesso aos clusters

```bash
# Configurar SSO
hotctl sso init

# Login via Okta
hotctl sso login

# Aplicar perfil de uma conta
hotctl sso accounts apply -n <PROFILE>

# Configurar contexto EKS
hotctl eks context --profile <PROFILE>
```

Onde `<PROFILE>` é o alias da conta: `buildstaging`, `vulcano`, `devops`, `hotpay`, etc.

### Verificar acesso ao cluster

```bash
# Listar pods de um namespace
kubectl get pods -n <namespace>

# Listar nodes do cluster
kubectl get nodes
```

Se os comandos funcionarem, seu ambiente está configurado.

---

## Antes de ir embora

Checklist de fim do primeiro dia:

- [ ] Participei do kickoff e conheço meu fellow
- [ ] Conheço minha PU
- [ ] Okta configurado com MFA
- [ ] Todos os tickets de acesso abertos no Service Desk
- [ ] Issue de progresso criada no GitHub
- [ ] Ferramentas instaladas (kubectl, AWS CLI, hotctl, Rancher Desktop)
- [ ] Li o README do onboarding
- [ ] Sei onde encontrar a documentação

Se algum acesso já foi aprovado, configure-o. Se não, não se preocupe: os acessos levam 1-2 dias úteis.

Amanhã, comece pela leitura do [Platform Overview](../platform-overview/platform-architecture) e inicie o [Training Camp](../training-camp/training-overview).

---

## Referências

📄 [Introduction](introduction)
📄 [Okta Access](okta-access)
📄 [Required Tools](required-tools)
📄 [Access Pack: Solicitações](access-requests-examples)
📄 [Onboarding Checklist](../devops-journey/onboarding-checklist)
📄 [Platform Architecture](../platform-overview/platform-architecture)
