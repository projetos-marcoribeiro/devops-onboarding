# Projeto Hands-On

## 📑 Índice

- [Arquitetura da aplicação](#arquitetura-da-aplicação)
- [Etapas do projeto](#etapas-do-projeto)
- [Dicas para não travar](#dicas-para-não-travar)

---

O projeto Hands-On consiste no deploy de uma aplicação simples com frontend e backend, utilizando os padrões reais da plataforma Hotmart. O foco não é a aplicação em si, mas o processo de colocá-la em produção do jeito certo.

🔗 Repositório: [https://github.com/Hotmart-Org/handson_devops](https://github.com/Hotmart-Org/handson_devops)

---

## Arquitetura da aplicação

```
Usuário
    │
    ├──► Frontend (S3 + CloudFront)
    │         arquivos estáticos servidos via CDN
    │
    └──► Backend (EKS)
              API containerizada rodando em Kubernetes
              exposta via ALB + Ingress
```

### Frontend

O frontend é uma aplicação estática (HTML, CSS, JavaScript) hospedada no Amazon S3 e servida via CloudFront. Essa é a arquitetura padrão para frontends estáticos na Hotmart.

**O que você vai configurar:**
- Bucket S3 para hospedar os arquivos estáticos
- Distribuição CloudFront apontando para o bucket
- Pipeline de CI/CD para fazer deploy automático dos arquivos ao S3

### Backend

O backend é uma API simples containerizada que roda em Kubernetes (EKS). Ele representa o padrão de deploy de serviços na plataforma Hotmart.

**O que você vai configurar:**
- `Dockerfile` para containerizar a aplicação
- Repositório ECR para armazenar a imagem Docker
- Infraestrutura via Base Module (IAM Role, ALB, Security Groups)
- Helm values para configurar o deploy no Kubernetes
- Pipeline de CI/CD para build, push e deploy automático

---

## Etapas do projeto

### 1. Fork e setup do repositório

Faça fork do repositório [handson_devops](https://github.com/Hotmart-Org/handson_devops) para sua conta ou para a organização, conforme orientação do mentor.

```bash
git clone https://github.com/Hotmart-Org/handson_devops.git
cd handson_devops
```

### 2. Criação do Dockerfile

Escreva o `Dockerfile` para o backend. Siga as boas práticas:

```dockerfile
# Exemplo de Dockerfile para uma API Node.js
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .
EXPOSE 8080
USER node
CMD ["node", "src/index.js"]
```

Pontos importantes:
- Use multi-stage build para reduzir o tamanho da imagem final
- Não rode o processo como root dentro do container
- Exponha apenas a porta necessária

### 3. Configuração do pipeline GitHub Actions

Configure o workflow de CI/CD referenciando os workflows reutilizáveis da organização:

```yaml
# .github/workflows/deploy.yaml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    uses: Hotmart-Org/actions/.github/workflows/docker.yaml@main
    with:
      image-name: handson-backend
      environment: staging
    secrets: inherit

  deploy-frontend:
    uses: Hotmart-Org/actions/.github/workflows/s3.yaml@main
    with:
      bucket: handson-frontend
      source-path: ./frontend/dist
    secrets: inherit
```

### 4. Configuração do Base Module

Declare a infraestrutura necessária para o backend no YAML do Base Module:

```yaml
app:
  name: handson-backend
  team: devops-onboarding
  environment: staging

container:
  port: 8080
  replicas: 1
  resources:
    requests:
      cpu: "100m"
      memory: "128Mi"
    limits:
      cpu: "250m"
      memory: "256Mi"

ingress:
  enabled: true
  host: handson-backend.staging.hotmart.com
  tls: true

monitoring:
  enabled: true
```

### 5. Configuração dos Helm values

Configure os values do Helm para o deploy no Kubernetes. Os values ficam no repositório [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac):

```yaml
# apps/handson-backend/staging/values.yaml
image:
  repository: <account-id>.dkr.ecr.us-east-1.amazonaws.com/handson-backend
  tag: latest
  pullPolicy: Always

replicaCount: 1

service:
  port: 8080

env:
  - name: ENVIRONMENT
    value: staging
```

### 6. Criação de subtask no Jira

Crie uma subtask no Jira para rastrear o progresso do projeto Hands-On. Isso faz parte do processo de trabalho do time e ajuda a praticar o fluxo de trabalho com tickets.

O ticket deve conter:
- Descrição do que está sendo feito
- Link para o repositório
- Critérios de conclusão

### 7. Abertura de Pull Request

Ao finalizar as configurações, abra um Pull Request no repositório. O PR deve:
- Ter uma descrição clara do que foi feito
- Incluir evidências de que o deploy funcionou (screenshots, logs)
- Estar pronto para revisão do mentor

---

## Dicas para não travar

- **Pipeline falhando?** Verifique os logs do GitHub Actions step a step. A maioria dos erros tem mensagem clara.
- **Pod não sobe?** Use `kubectl describe pod` e `kubectl logs` para investigar.
- **ArgoCD não sincroniza?** Verifique se o YAML de values está correto e se o ArgoCD tem acesso ao repositório.
- **Dúvida sobre o Base Module?** Consulte a documentação e pergunte ao mentor antes de tentar adivinhar.

O mentor está disponível para ajudar em qualquer etapa. Não trave sozinho por mais de 30 minutos sem pedir ajuda.
