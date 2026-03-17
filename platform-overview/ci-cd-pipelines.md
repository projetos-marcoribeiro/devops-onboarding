# CI/CD Pipelines

## 📑 Índice

- [GitHub Actions](#github-actions)
- [Runners Self-Hosted com ARC](#runners-self-hosted-com-arc)
- [Workflows Reutilizáveis](#workflows-reutilizáveis)
- [Autenticação com OIDC](#autenticação-com-oidc)
- [Fluxo completo de um pipeline típico](#fluxo-completo-de-um-pipeline-típico)
- [Boas práticas](#boas-práticas)
- [Referências](#referências)

---

A Hotmart utiliza GitHub Actions como plataforma de CI/CD. Os pipelines são padronizados através de workflows reutilizáveis mantidos centralmente pela organização, o que garante consistência entre os times e reduz a duplicação de código de pipeline.

---

## GitHub Actions

O GitHub Actions é o sistema de automação nativo do GitHub. Workflows são definidos em arquivos YAML dentro do diretório `.github/workflows/` de cada repositório.

Na Hotmart, os times de produto raramente escrevem workflows do zero. Em vez disso, eles referenciam workflows reutilizáveis mantidos pelo time DevOps no repositório central:

🔗 [https://github.com/Hotmart-Org/actions](https://github.com/Hotmart-Org/actions)

---

## Runners Self-Hosted com ARC

Os jobs dos pipelines não rodam nos runners gerenciados pelo GitHub (GitHub-hosted runners). Na Hotmart, os runners são **self-hosted** e rodam dentro dos próprios clusters Kubernetes via **Actions Runner Controller (ARC)**.

**Por que self-hosted runners:**
- Controle total sobre o ambiente de execução
- Acesso direto à rede interna da AWS sem precisar de VPN ou exposição pública
- Melhor performance para builds que precisam de recursos específicos
- Custo previsível e otimizável
- Isolamento por namespace ou cluster conforme necessário

**Como o ARC funciona:**

```
GitHub Actions dispara um job
         │
         ▼
ARC (rodando no cluster) detecta o job pendente
         │
         ▼
ARC provisiona um pod runner no cluster
         │
         ▼
Pod executa o job do pipeline
         │
         ▼
Pod é destruído ao final da execução
```

Cada job roda em um pod efêmero, o que garante isolamento completo entre execuções.

---

## Workflows Reutilizáveis

O repositório central de actions contém workflows reutilizáveis para os casos de uso mais comuns. Os times referenciam esses workflows nos seus próprios pipelines usando a sintaxe `uses`:

```yaml
jobs:
  build:
    uses: Hotmart-Org/actions/.github/workflows/docker.yaml@main
    with:
      image-name: minha-aplicacao
    secrets: inherit
```

### Principais workflows disponíveis

| Workflow | Descrição |
|---|---|
| `base.yaml` | Workflow base com steps comuns a todos os pipelines (checkout, setup, autenticação) |
| `argocd.yaml` | Atualiza a imagem no repositório de configuração do ArgoCD para disparar o deploy |
| `docker.yaml` | Build e push de imagem Docker para o ECR |
| `s3.yaml` | Deploy de assets estáticos para S3 |
| `rollback.yaml` | Rollback de deploy para uma versão anterior |
| `scaling.yaml` | Ajuste manual de réplicas de um deployment |

---

## Autenticação com OIDC

Os pipelines precisam de permissões na AWS para executar operações como push de imagem para o ECR, deploy via ArgoCD e acesso a secrets. Na Hotmart, isso é feito via **OIDC (OpenID Connect)**, sem uso de credenciais estáticas (access key + secret key).

**Como funciona:**

```
GitHub Actions inicia o job
         │
         ▼
GitHub gera um token OIDC assinado
para aquele repositório e workflow
         │
         ▼
Workflow usa o token para assumir
uma IAM Role na AWS via sts:AssumeRoleWithWebIdentity
         │
         ▼
AWS valida o token com o GitHub
e retorna credenciais temporárias
         │
         ▼
Job executa com as permissões
da IAM Role por tempo limitado
```

**Configuração no workflow:**

```yaml
permissions:
  id-token: write   # necessário para OIDC
  contents: read

steps:
  - name: Configure AWS credentials
    uses: aws-actions/configure-aws-credentials@v4
    with:
      role-to-assume: arn:aws:iam::ACCOUNT_ID:role/GitHubActionsRole
      aws-region: us-east-1
```

As IAM Roles usadas pelos pipelines são criadas e gerenciadas pelo base-module, com as permissões mínimas necessárias para cada repositório.

---

## Fluxo completo de um pipeline típico

```
Developer faz push para branch main
         │
         ▼
GitHub Actions dispara o workflow
         │
         ├── Checkout do código
         ├── Build da imagem Docker
         ├── Execução de testes
         ├── Push da imagem para ECR (via OIDC)
         │
         ▼
Workflow argocd.yaml atualiza o tag da imagem
no repositório de configuração do ArgoCD
         │
         ▼
ArgoCD detecta a mudança e sincroniza o cluster
         │
         ▼
Nova versão da aplicação está em produção
```

---

## Boas práticas

- Nunca coloque credenciais AWS hardcoded em workflows. Use sempre OIDC.
- Use `secrets: inherit` com cuidado — passe apenas os secrets necessários para cada workflow reutilizável.
- Mantenha os workflows dos repositórios de produto simples, delegando a lógica para os workflows reutilizáveis centrais.
- Para adicionar um novo workflow reutilizável ou modificar um existente, abra um PR no repositório `Hotmart-Org/actions` e envolva o time DevOps na revisão.

---

## Referências

📄 [`platform-overview/platform-architecture.md`](#/platform-overview/platform-architecture)
📄 [`platform-overview/argocd-gitops.md`](#/platform-overview/argocd-gitops)
📄 [`platform-overview/base-module.md`](#/platform-overview/base-module)
