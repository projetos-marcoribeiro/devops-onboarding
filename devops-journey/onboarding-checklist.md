# Checklist de Progresso do Onboarding

---

> ℹ️ Este repositório será migrado futuramente para a organização Hotmart-Org. Quando isso acontecer, atualize o link da Issue abaixo.

Este checklist acompanha seu progresso ao longo do onboarding. No primeiro dia, abra uma **Issue** no repositório do onboarding com o conteúdo abaixo para acompanhar seu progresso de forma visível.

> Link atual: [devops-onboarding](https://github.com/projetos-marcoribeiro/devops-onboarding)
> Link futuro (após migração): `https://github.com/Hotmart-Org/devops-onboarding`

---

## Como usar

1. Abra uma Issue no repositório com o título: `Onboarding: [Seu Nome]`
2. Cole o checklist abaixo no corpo da Issue
3. Marque os itens conforme for completando
4. Seu fellow e coordenador podem acompanhar o progresso pela Issue

---

## Template da Issue

Copie e cole o conteúdo abaixo ao criar sua Issue:

```markdown
## 📋 Onboarding Checklist: [Seu Nome]

**Fellow:** [nome do fellow]
**Data de início:** [data]
**PU:** [sua Product Unit]

> Após criar esta Issue: 1) Atribua a si mesmo no campo **Assignees** (clique na engrenagem à direita). 2) Adicione seu gestor também como Assignee para que ele acompanhe o progresso.

---

### Semana 1: Kickoff + Training Camp (~4 dias)

**Dia 1: Setup**
- [ ] Participar da apresentação geral do time DevOps
- [ ] Conhecer a Product Unit (PU)
- [ ] Seguir o [Dia 1: Passo a Passo](https://projetos-marcoribeiro.github.io/devops-onboarding/#/getting-started/dia-1-passo-a-passo)
- [ ] Abrir TODOS os tickets de acesso no [Service Desk](https://hotmart.atlassian.net/servicedesk/customer/portal/4/group/315)
- [ ] Instalar ferramentas: kubectl, AWS CLI, hotctl, Rancher Desktop
- [ ] Configurar Okta e MFA
- [ ] Ler o README do onboarding
- [ ] Explorar o [Mapa de Repositórios](https://projetos-marcoribeiro.github.io/devops-onboarding/#/getting-started/mapa-de-repositorios)

**Training Camp**
- [ ] Preencher Skill Matrix com o fellow
- [ ] Assistir: IAC + Base Module
- [ ] Assistir: Monitoring Applications
- [ ] Assistir: KEDA
- [ ] Assistir: Vault
- [ ] Assistir: SIEM
- [ ] Assistir: Cloudflare
- [ ] Assistir: Troubleshooting
- [ ] Ler: Platform Architecture
- [ ] Ler: EKS Clusters
- [ ] Ler: CI/CD Pipelines
- [ ] Ler: ArgoCD & GitOps

**Checkpoint:** reunião com fellow ao final da semana

---

### Semana 2-3: Hands-On (1-2 semanas)

- [ ] Clonar repositório handson_devops
- [ ] Criar branch handson/{SEU_NOME}
- [ ] Parte 1: Personalizar backend (index.js)
- [ ] Parte 2: PR no buildstaging-iac (CDN behavior)
- [ ] Parte 3: Dockerfile + Frontend (S3)
- [ ] Parte 4: Helm values
- [ ] Parte 5: Pipeline GitHub Actions
- [ ] Backend respondendo na URL de staging
- [ ] Frontend acessível via CloudFront
- [ ] PR aberto e revisado pelo fellow
- [ ] Subtask criada no Jira

**Checkpoint:** reunião com fellow após semana 1 + após semana 2

---

### Semana 4-5: Operacional (1-2 semanas)

- [ ] Ler Smart Contract
- [ ] Acompanhar fila do Service Desk da PU
- [ ] Resolver pelo menos 2 tickets simples com orientação
- [ ] Acompanhar o board DEVOPS no Jira
- [ ] Participar de pelo menos 1 deploy real como observador
- [ ] Ler documentação de Incident Management

**Checkpoint:** reunião com fellow após semana 2

---

### Mês 2-3: Day by Day + On-Call Prep

- [ ] Atuar em tickets operacionais com autonomia crescente
- [ ] Participar de pelo menos 1 incidente como observador
- [ ] Ler pelo menos 3 postmortems recentes
- [ ] Completar checklist de prontidão para on-call
- [ ] Configurar alertas no JiraOps (Jira Service Management Operations)
- [ ] Revisar Skill Matrix com fellow (progresso)
- [ ] Reunião com coordenador sobre expectativas de on-call

**Checkpoint:** quinzenal com fellow + trimestral com coordenador

---

### Conclusão

- [ ] Skill Matrix revisada e atualizada
- [ ] Fellow confirma prontidão para operação autônoma
- [ ] Coordenador confirma prontidão para rotação de on-call
```

---

> Mantenha a Issue atualizada. Ela é a fonte da verdade do seu progresso e facilita a conversa com o fellow nos checkpoints.

---

## Acompanhamento e conclusão

A Issue do GitHub é o registro oficial do progresso do onboarding.

**Como funciona:**

1. Ao criar a Issue, atribua a si mesmo e ao seu gestor no campo **Assignees**
2. A label `onboarding` já é aplicada automaticamente
3. Ao longo do onboarding, marque os itens conforme for completando
4. Ao final, adicione um comentário com a data de conclusão e observações
5. O gestor acompanha o progresso diretamente pela Issue, sem necessidade de relatórios separados

A Issue fica registrada no GitHub com histórico completo: quem marcou cada item, quando foi marcado e os comentários do fellow ao longo do processo.
