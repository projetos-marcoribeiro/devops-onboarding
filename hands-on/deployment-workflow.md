# Workflow de Deploy

## 📑 Índice

- [Visão geral do fluxo](#visão-geral-do-fluxo)
- [Passo a passo detalhado](#passo-a-passo-detalhado)
- [Runners self-hosted e GitOps](#runners-self-hosted-e-gitops)
- [Acompanhando um deploy](#acompanhando-um-deploy)
- [Rollback](#rollback)

---

Este documento descreve o fluxo completo de deploy utilizado na plataforma Hotmart. Entender esse fluxo é fundamental para operar com autonomia — seja fazendo um deploy novo, investigando uma falha ou executando um rollback.

---

## Visão geral do fluxo

```
Developer faz commit e push
         │
         ▼
GitHub (repositório da aplicação)
         │  trigger automático via webhook
         ▼
GitHub Actions
         │  runners self-hosted no Kubernetes (ARC)
         │  build da imagem Docker
         │  push para ECR
         │  atualiza tag no repositório de configuração
         ▼
Base Module (Terraform)
         │  provisionamento de infraestrutura
         │  ECR, IAM Roles, ALB, Security Groups, DNS
         ▼
ArgoCD
         │  detecta mudança no repositório de configuração
         │  compara estado desejado (Git) com estado atual (cluster)
         │  sincroniza o cluster
         ▼
Kubernetes (EKS)
         │  novo pod é criado com a imagem atualizada
         │  health checks são executados
         │  tráfego é roteado para o novo pod
         ▼
Aplicação em produção
```

---

## Passo a passo detalhado

### 1. Developer faz push no GitHub

Tudo começa com um commit. Quando o código é enviado para a branch principal (geralmente `main`), o GitHub dispara automaticamente o pipeline de CI/CD via webhook.

```bash
git add .
git commit -m "feat: adiciona endpoint de health check"
git push origin main
```

### 2. GitHub Actions inicia o pipeline

O GitHub Actions recebe o evento de push e inicia o workflow definido em `.github/workflows/`. Os jobs são executados nos runners self-hosted que rodam dentro do próprio cluster Kubernetes via Actions Runner Controller (ARC).

Cada job roda em um pod efêmero — criado no início do job e destruído ao final. Isso garante isolamento completo entre execuções.

```yaml
# Exemplo de trigger no workflow
on:
  push:
    branches:
      - main
```

### 3. Pipeline executa o Base Module

Se houver mudanças na configuração de infraestrutura (YAML do Base Module), o pipeline executa o Terraform para provisionar ou atualizar os recursos necessários.

```bash
# Executado pelo pipeline internamente
terraform init
terraform plan -var-file=staging.tfvars
terraform apply -auto-approve
```

O Base Module cria ou atualiza automaticamente: ECR, IAM Roles, ALB, Security Groups, registros DNS e certificados TLS.

### 4. Infraestrutura é provisionada via Terraform

O Terraform aplica as mudanças de infraestrutura de forma incremental — apenas o que mudou é atualizado. O state é armazenado remotamente no S3 com lock via DynamoDB para evitar conflitos.

### 5. Docker image é construída e enviada ao ECR

O pipeline faz o build da imagem Docker e envia para o ECR (Elastic Container Registry) da conta AWS correspondente.

```bash
# Executado pelo workflow docker.yaml
docker build -t $ECR_REGISTRY/$IMAGE_NAME:$COMMIT_SHA .
docker push $ECR_REGISTRY/$IMAGE_NAME:$COMMIT_SHA
```

A imagem é tagueada com o SHA do commit, garantindo rastreabilidade total: sempre é possível saber exatamente qual código está rodando em produção.

### 6. ArgoCD detecta mudanças

Após o push da imagem, o pipeline atualiza o arquivo de values no repositório de configuração ([devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac)) com o novo tag da imagem.

O ArgoCD monitora esse repositório e detecta a mudança automaticamente (polling a cada ~3 minutos ou via webhook imediato).

```yaml
# O pipeline atualiza este valor automaticamente
image:
  tag: "abc1234"  # SHA do commit
```

### 7. Deploy é realizado no cluster Kubernetes

O ArgoCD compara o estado desejado (definido no Git) com o estado atual do cluster e aplica as diferenças. O Kubernetes então:

1. Cria novos pods com a imagem atualizada
2. Aguarda os health checks passarem (readiness probe)
3. Redireciona o tráfego para os novos pods
4. Remove os pods antigos

Esse processo é chamado de **rolling update** e garante zero downtime durante deploys normais.

---

## Runners self-hosted e GitOps

### Runners self-hosted

Os pipelines rodam em runners self-hosted gerenciados pelo Actions Runner Controller (ARC) dentro dos clusters EKS. Isso significa que os jobs têm acesso direto à rede interna da AWS, sem precisar de exposição pública ou VPN.

Cada runner é um pod efêmero: nasce quando o job começa e morre quando termina. Isso garante ambiente limpo para cada execução.

### Modelo GitOps

O deploy segue o modelo GitOps: o Git é a única fonte da verdade para o estado das aplicações. Isso significa:

- **Nenhum deploy manual via kubectl em produção.** Toda mudança passa por um commit.
- **Rastreabilidade total.** Cada deploy tem um commit associado com autor, data e mensagem.
- **Rollback simples.** Reverter um deploy é reverter um commit.
- **Self-healing.** Se alguém modificar algo diretamente no cluster, o ArgoCD reverte automaticamente para o estado declarado no Git.

---

## Acompanhando um deploy

### Via GitHub Actions

Acesse a aba **Actions** do repositório no GitHub para ver o status do pipeline em tempo real, os logs de cada step e o resultado final.

### Via ArgoCD

Acesse a interface web do ArgoCD para ver o status da sincronização, o health da aplicação e o histórico de deploys.

```bash
# Via CLI do ArgoCD
argocd app get handson-backend
argocd app history handson-backend
```

### Via kubectl

```bash
# Acompanhar o rollout em tempo real
kubectl rollout status deployment/handson-backend -n handson

# Ver os pods sendo substituídos
kubectl get pods -n handson -w

# Ver eventos do namespace
kubectl get events -n handson --sort-by='.lastTimestamp'
```

---

## Rollback

Se um deploy causar problemas, o rollback pode ser feito de duas formas:

**Via Git (recomendado):**
```bash
git revert HEAD
git push origin main
# O pipeline executa normalmente e deploya a versão anterior
```

**Via ArgoCD:**
```bash
# Rollback para a versão anterior
argocd app rollback handson-backend

# Rollback para uma versão específica
argocd app rollback handson-backend --revision 5
```

**Via workflow de rollback:**
```yaml
# Usando o workflow reutilizável de rollback
uses: Hotmart-Org/actions/.github/workflows/rollback.yaml@main
with:
  app-name: handson-backend
  revision: "abc1234"
```
