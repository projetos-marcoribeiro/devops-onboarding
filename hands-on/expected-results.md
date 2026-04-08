# Resultados Esperados

## 📑 Índice

- [Checklist de entrega](#checklist-de-entrega)
- [O que demonstrar ao mentor](#o-que-demonstrar-ao-mentor)
- [O que vem depois](#o-que-vem-depois)

---

Este documento descreve o que é esperado ao final do projeto Hands-On. Use como checklist para validar se o projeto está completo e se você está pronto para começar a atuar em tarefas reais do time DevOps.

---

## Checklist de entrega

### Repositório e código

- [ ] Branch `handson/{SEU_NOME}` criada no repositório [handson_devops](https://github.com/Hotmart-Org/handson_devops)
- [ ] `backend/index.js` editado com seu login no path da API
- [ ] `frontend/index.html` editado com seu login na chamada AJAX

### CDN e Frontend

- [ ] PR aberto no repositório buildstaging-iac com behavior no `terraform/cdn/devops.tf`
- [ ] Behavior configurado com path `/SEU_LOGIN/api*`
- [ ] Arquivos estáticos do frontend enviados ao S3 (`handson-devops.buildstaging.com`)
- [ ] Frontend acessível via CloudFront (`E2CJ6PTRE33WP2`)

### Backend e Kubernetes

- [ ] Dockerfile criado usando base image interna (`315120000506.dkr.ecr.us-east-1.amazonaws.com/...`)
- [ ] Backend respondendo em `handson-devops.buildstaging.com/SEU_LOGIN/api`
- [ ] Pod rodando no cluster EKS de staging
- [ ] Helm values configurados no repositório [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac)

### Pipeline GitHub Actions

- [ ] Pipeline criado usando os workflows reutilizáveis da organização
- [ ] Base-Module Action (`Hotmart-Org/actions/base-module@master`) configurado
- [ ] Docker Action (`Hotmart-Org/actions/docker@master`) configurado
- [ ] Helm Action (`Hotmart-Org/actions/helm@master`) configurado
- [ ] S3 Action (`Hotmart-Org/actions/s3@master`) configurado
- [ ] Runner correto de staging utilizado (`buildstaging-iac`)

### Processo

- [ ] Subtask criada no Jira com descrição e critérios de conclusão
- [ ] Pull Request aberto com descrição clara e evidências do deploy
- [ ] PR revisado e aprovado pelo mentor

---

## O que demonstrar ao mentor

Ao apresentar o projeto, o engenheiro deve ser capaz de demonstrar:

### Entendimento do fluxo de deploy

Explicar o que acontece desde o `git push` até a aplicação estar rodando no cluster. Não apenas "funcionou" — mas por quê funcionou e o que cada peça faz.

Perguntas que o mentor pode fazer:
- "O que acontece se o health check do pod falhar?"
- "Como eu faço rollback se esse deploy causar um problema?"
- "Onde eu vejo os logs dessa aplicação em produção?"
- "O que o Base Module criou de infraestrutura para essa aplicação?"

### Capacidade de navegar nos repositórios da plataforma

Demonstrar que consegue encontrar informações nos repositórios da organização:
- Localizar o workflow que fez o build da imagem
- Encontrar o Helm values da aplicação no devops-helm-iac
- Navegar no ArgoCD e identificar o estado da aplicação
- Encontrar os recursos criados pelo Base Module no console AWS

### Compreensão básica da infraestrutura Kubernetes

Usar o kubectl para verificar o estado da aplicação no cluster:

```bash
# Verificar pods rodando
kubectl get pods -n handson

# Ver logs da aplicação
kubectl logs -f deployment/handson-backend -n handson

# Verificar o ingress configurado
kubectl get ingress -n handson

# Descrever o deployment
kubectl describe deployment handson-backend -n handson
```

---

## O que vem depois

Completar o Hands-On significa que você entende o ciclo completo de deploy da plataforma e está pronto para começar a atuar em tarefas reais do time DevOps.

A partir daqui, você vai:

- Participar do trabalho operacional do dia a dia (tickets, deploys, suporte)
- Atender solicitações no Service Desk com orientação do time
- Participar de incidentes como observador, aprendendo como o time responde
- Gradualmente ganhar autonomia para executar tarefas sem supervisão constante
- Avançar em direção à preparação para on-call

O Hands-On não é o fim do aprendizado — é o começo do trabalho real. A plataforma tem muitas camadas e a profundidade vem com a prática. O importante é que agora você tem o contexto necessário para aprender fazendo.

📄 [`operations/operational-tasks.md`](../operations/operational-tasks)
📄 [`incident-management/incident-process.md`](../incident-management/incident-process)
📄 [`oncall-readiness/oncall-readiness-checklist.md`](../oncall-readiness/oncall-readiness-checklist)
