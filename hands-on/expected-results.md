# Resultados Esperados

## 📑 Índice

- [Checklist de entrega](#checklist-de-entrega)
- [O que demonstrar ao mentor](#o-que-demonstrar-ao-mentor)
- [O que vem depois](#o-que-vem-depois)

---

Este documento descreve o que é esperado ao final do projeto Hands-On. Use como checklist para validar se o projeto está completo e se você está pronto para começar a atuar em tarefas reais do time DevOps.

---

## Checklist de entrega

### Infraestrutura

- [ ] Infraestrutura provisionada via Base Module (ECR, IAM Role, ALB, Security Groups)
- [ ] Bucket S3 criado para o frontend
- [ ] Distribuição CloudFront configurada e apontando para o S3
- [ ] Certificado TLS provisionado via ACM
- [ ] Registro DNS configurado via Route53

### Aplicação

- [ ] Frontend acessível via URL do CloudFront
- [ ] Backend acessível via endpoint configurado (ALB + Ingress)
- [ ] Health check do backend respondendo corretamente
- [ ] Comunicação entre frontend e backend funcionando

### Pipeline

- [ ] Pipeline de CI/CD configurado e executando sem erros
- [ ] Build da imagem Docker funcionando
- [ ] Push para ECR funcionando
- [ ] Deploy automático disparado a cada push na branch principal

### GitOps

- [ ] Deploy gerenciado pelo ArgoCD
- [ ] Aplicação com status `Healthy` e `Synced` no ArgoCD
- [ ] Helm values configurados corretamente no repositório devops-helm-iac

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

📄 [`operations/operational-tasks.md`](#/operations/operational-tasks)
📄 [`incident-management/incident-process.md`](#/incident-management/incident-process)
📄 [`oncall-readiness/oncall-readiness-checklist.md`](#/oncall-readiness/oncall-readiness-checklist)
