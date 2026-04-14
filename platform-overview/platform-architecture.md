# Arquitetura da Plataforma Hotmart

## 📑 Índice

- [Fluxo principal de deploy](#fluxo-principal-de-deploy)
- [Papel de cada componente](#papel-de-cada-componente)
- [Princípios da plataforma](#princípios-da-plataforma)
- [Referências](#referências)

---

A plataforma DevOps da Hotmart é construída sobre princípios de Infrastructure as Code, GitOps, observabilidade por padrão e automação de deploy. O objetivo é que times de produto consigam fazer deploy de aplicações com segurança, rastreabilidade e sem depender de operações manuais.

---

## Fluxo principal de deploy

```
Developer
    │
    │  push / pull request
    ▼
GitHub Repository
    │
    │  trigger automático
    ▼
GitHub Actions (CI/CD)
    │  build, test, push de imagem para ECR
    │  execução de workflows reutilizáveis da org
    ▼
Base Module (Terraform)
    │  provisionamento de infraestrutura declarativa
    │  ECR, IAM Roles, ALB, Security Groups, Secrets, KMS
    ▼
ArgoCD (GitOps)
    │  detecta mudança no repositório de configuração
    │  sincroniza o estado desejado no cluster
    ▼
EKS Clusters (Kubernetes)
    │  pods rodando a aplicação
    │  Karpenter gerenciando nodes automaticamente
    ▼
Cloudflare → ALB → Ingress Controller → Pods
    │  tráfego externo roteado até a aplicação
    ▼
Observability Stack
       NewRelic / Prometheus / Grafana / PagerDuty
       Datadog (Teachable) · Sentry · Kubecost
```

---

## Papel de cada componente

### GitHub

É onde todo o código vive: tanto o código das aplicações quanto o código de infraestrutura. A organização `Hotmart-Org` no GitHub centraliza todos os repositórios. Mudanças no código disparam automaticamente os pipelines de CI/CD.

### GitHub Actions

Sistema de CI/CD baseado em workflows YAML. Na Hotmart, os workflows são padronizados e reutilizáveis, mantidos em um repositório central da organização. Os runners rodam dentro dos próprios clusters Kubernetes via Actions Runner Controller (ARC), o que garante isolamento e escalabilidade.

### Base Module (Terraform)

O coração do provisionamento de infraestrutura. É um módulo Terraform que recebe um arquivo YAML declarativo e cria automaticamente todos os recursos AWS necessários para uma aplicação: ECR, IAM Roles, ALB, Security Groups, KMS, Secrets Manager e configurações de monitoramento. Os times de produto não precisam escrever Terraform: apenas declaram o que precisam.

### ArgoCD (em implementação)

O ArgoCD está sendo implementado como o novo modelo de deploy contínuo via GitOps da Hotmart, substituindo o fluxo anterior baseado em Helm upgrade/install/rollback. Quando totalmente operacional, o ArgoCD monitorará repositórios de configuração e sincronizará automaticamente o estado dos clusters Kubernetes com o estado declarado no Git, tornando cada mudança no repositório de configuração um deploy automático e rastreável.

### EKS Clusters

Os clusters Kubernetes gerenciados pela AWS (Elastic Kubernetes Service) onde as aplicações rodam. A Hotmart opera múltiplos clusters organizados por domínio, ambiente e criticidade. O Karpenter gerencia o provisionamento automático de nodes conforme a demanda.

### Cloudflare / ALB / Ingress

O tráfego externo passa pelo Cloudflare (proteção DDoS, WAF, CDN), depois pelo Application Load Balancer da AWS, que roteia para o Ingress Controller dentro do cluster, que por sua vez direciona para os pods corretos. O External DNS e o Route53 gerenciam automaticamente os registros DNS.

### Observability Stack

Toda aplicação deployada via base-module recebe monitoramento automático. A stack inclui NewRelic para APM, Prometheus para métricas internas, Grafana para visualização, Sentry para error tracking, Kubecost para monitoramento de custos, Pingdom para uptime externo e JiraOps para alertas e gestão de incidentes (substituindo o PagerDuty). O Datadog é utilizado exclusivamente pela PU Teachable e está sendo removido da plataforma principal.

---

## Princípios da plataforma

### Infrastructure as Code

Toda infraestrutura é definida em código (Terraform, Helm, YAML). Não existe provisionamento manual de recursos. Isso garante reprodutibilidade, rastreabilidade e facilidade de revisão via pull request.

### GitOps

O Git é a fonte da verdade para o estado da infraestrutura e das aplicações. Qualquer mudança passa por um repositório Git, o que garante histórico completo, revisão por pares e rollback simples. O ArgoCD está sendo implementado como a ferramenta que operacionaliza esse modelo na Hotmart, substituindo o fluxo anterior baseado em Helm upgrade/install/rollback.

### Observability by Default

Toda aplicação deployada via base-module recebe automaticamente configurações de monitoramento, alertas e logging. O time de produto não precisa configurar observabilidade manualmente.

### Automação de Deploy

O fluxo de deploy é totalmente automatizado do commit ao cluster. Intervenção manual só é necessária em situações excepcionais. Isso reduz erros humanos e aumenta a velocidade de entrega.

---

## Referências

📄 [`platform-overview/aws-accounts.md`](aws-accounts)
📄 [`platform-overview/eks-clusters.md`](eks-clusters)
📄 [`platform-overview/ci-cd-pipelines.md`](ci-cd-pipelines)
📄 [`platform-overview/argocd-gitops.md`](argocd-gitops)
📄 [`platform-overview/base-module.md`](base-module)
📄 [`platform-overview/observability-stack.md`](observability-stack)
📄 [`platform-overview/networking-ingress.md`](networking-ingress)
