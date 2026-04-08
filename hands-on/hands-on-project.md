# Projeto Hands-On

## 📑 Índice

- [Overview](#overview)
- [Tecnologias utilizadas](#tecnologias-utilizadas)
- [Setup](#setup)
- [Parte 1 — Personalizar o backend](#parte-1--personalizar-o-backend)
- [Parte 2 — Configurar o CDN (behavior)](#parte-2--configurar-o-cdn-behavior)
- [Parte 3 — Dockerfile e Frontend](#parte-3--dockerfile-e-frontend)
- [Parte 4 — Helm values (ambiente)](#parte-4--helm-values-ambiente)
- [Parte 5 — Pipeline GitHub Actions](#parte-5--pipeline-github-actions)
- [Resultado esperado](#resultado-esperado)
- [Dicas para não travar](#dicas-para-não-travar)

---

O projeto Hands-On consiste no deploy de uma aplicação simples com frontend e backend, utilizando os padrões reais da plataforma Hotmart. O foco não é a aplicação em si, mas o processo de colocá-la em produção do jeito certo.

🔗 Repositório: [https://github.com/Hotmart-Org/handson_devops](https://github.com/Hotmart-Org/handson_devops)

```text
Info
- Goal: deploy de uma aplicação em staging e produção
- Tempo estimado: 1-2 semanas
- Checkpoint: após semana 1 + após semana 2
```

---

## Overview

Nesta prática você vai subir um frontend (em um CDN pré-existente via S3 + CloudFront) e um backend (em EKS). O código de ambos já está pronto no repositório — é algo simples com o objetivo de demonstrar o funcionamento de ambas as aplicações e como provisioná-las no ambiente AWS.

```
Usuário
    │
    ├──► Frontend (S3 + CloudFront)
    │         arquivos estáticos servidos via CDN
    │         Destino S3: handson-devops.buildstaging.com
    │         CloudFront: E2CJ6PTRE33WP2
    │
    └──► Backend (EKS)
              API Node.js containerizada (porta 3000)
              exposta em: handson-devops.buildstaging.com/SEU_LOGIN/api
```

---

## Tecnologias utilizadas

| Tecnologia | Papel no projeto |
|---|---|
| EKS | Cluster Kubernetes onde o backend roda |
| Helm | Configuração do deploy no Kubernetes ([hotmart-charts](https://github.com/Hotmart-Org/hotmart-charts)) |
| Base Module | Provisionamento de infraestrutura AWS |
| Docker | Containerização do backend |
| S3 | Hospedagem dos arquivos estáticos do frontend |
| CloudFront | CDN para servir o frontend |
| GitHub Actions | Pipeline de CI/CD |

---

## Setup

### 1. Clonar o repositório e criar branch

```bash
git clone https://github.com/Hotmart-Org/handson_devops.git
cd handson_devops
git checkout -b handson/{SEU_NOME}
```

> Padrão de branch: `handson/{SEU_NOME}` (ex: `handson/marco-ribeiro`)

### 2. Obter credenciais AWS

Use o [hotctl](https://github.com/Hotmart-Org/hotctl) para configurar o acesso:

```bash
hotctl cluster use <cluster-staging>
```

### 3. Verificar acesso ao Kubernetes

```bash
kubectl get nodes
kubectl get namespaces
```

> Referência: [Kubernetes cluster access & kubectl commands](https://backstage.hotmart.dev/docs/default/component/devops-docs/tools/kubernetes/access-and-commands/)

---

## Parte 1 — Personalizar o backend

Edite o arquivo `backend/index.js` substituindo `<<YOUR_LOGIN>>` pelo seu login:

```javascript
// backend/index.js
var express = require('express');
var app = express();

app.get('/SEU_LOGIN/api', function (req, res) {
  res.send('pong server ' + process.env.HOSTNAME + '!');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

Faça o mesmo no `frontend/index.html`, substituindo `<<SEU_LOGIN>>` pelo seu login na URL da chamada AJAX.

---

## Parte 2 — Configurar o CDN (behavior)

Abra um PR no repositório **buildstaging-iac**, incluindo seu behavior no arquivo `terraform/cdn/devops.tf`.

O behavior deve ter apenas a rota para a API:

```
path: /SEU_LOGIN/api*
```

> Peça ajuda ao fellow se não souber como estruturar o behavior no Terraform.

---

## Parte 3 — Dockerfile e Frontend

### Backend — Dockerfile

Crie o Dockerfile para o backend. Use as Internal Base-Images da Hotmart:

```dockerfile
FROM 315120000506.dkr.ecr.us-east-1.amazonaws.com/hotmart/alpine/node/12

COPY backend/ $HOME
```

> Repositório de base images: [docker-images](https://github.com/Hotmart-Org/docker-images)
> Padrão: `315120000506.dkr.ecr.us-east-1.amazonaws.com/${NAME}`

Dica: preste atenção nas **Container Ports** — o backend escuta na porta 3000.

### Frontend — S3 + CloudFront

O pipeline do frontend envia os arquivos estáticos para o S3 e são acessados via CloudFront:

| Recurso | Valor |
|---|---|
| Destino S3 | `handson-devops.buildstaging.com` |
| CloudFront Distribution | `E2CJ6PTRE33WP2` |

---

## Parte 4 — Helm values (ambiente)

Crie o arquivo `[environments].yaml` para configurar o ambiente da aplicação no Kubernetes.

Referência de Helm charts: [hotmart-charts](https://github.com/Hotmart-Org/hotmart-charts)

Escolha o DNS usando o padrão Hands On:

| Contexto | DNS |
|---|---|
| Hotmart (Privado/Público) | `${SEU_NOME}.buildstaging.com` |

---

## Parte 5 — Pipeline GitHub Actions

Crie o pipeline usando GitHub Actions. Use o runner correto de staging:

| Contexto | Runner |
|---|---|
| Hotmart | `buildstaging-iac` |
| Teachable | `staging-iac` |

Actions reutilizáveis que você deve usar:

```yaml
# Base Module — provisionamento de infraestrutura
- uses: Hotmart-Org/actions/base-module@master

# Docker — build e push da imagem
- uses: Hotmart-Org/actions/docker@master

# Helm — deploy no Kubernetes
- uses: Hotmart-Org/actions/helm@master

# S3 — deploy do frontend
- uses: Hotmart-Org/actions/s3@master
```

> Repositório de actions: [Hotmart-Org/actions](https://github.com/Hotmart-Org/actions)

---

## Resultado esperado

Ao final do projeto, você deve ter:

- [ ] Branch `handson/{SEU_NOME}` criada no repositório
- [ ] Backend respondendo em `handson-devops.buildstaging.com/SEU_LOGIN/api`
- [ ] Frontend acessível via CloudFront
- [ ] Pipeline GitHub Actions rodando com sucesso (Base Module + Docker + Helm + S3)
- [ ] PR aberto no buildstaging-iac com o behavior do CDN
- [ ] Helm values configurados para o ambiente
- [ ] Subtask criada no Jira para rastrear o progresso

---

## Dicas para não travar

- **Pipeline falhando?** Verifique os logs do GitHub Actions step a step. A maioria dos erros tem mensagem clara.
- **Pod não sobe?** Use `kubectl describe pod` e `kubectl logs` para investigar.
- **Não sabe qual runner usar?** Hotmart usa `buildstaging-iac`, Teachable usa `staging-iac`.
- **Dúvida sobre o Base Module?** Consulte a documentação e pergunte ao mentor antes de tentar adivinhar.
- **Dockerfile não builda?** Confirme que está usando a base image correta do ECR interno.
- **Frontend não aparece?** Verifique se o path no S3 e o behavior no CloudFront estão corretos.

O mentor está disponível para ajudar em qualquer etapa. Não trave sozinho por mais de 30 minutos sem pedir ajuda.
