# Diagrama: Arquitetura da Plataforma

Este diagrama representa a arquitetura geral da plataforma DevOps da Hotmart, mostrando como os componentes se conectam desde o código do desenvolvedor até a aplicação em produção com monitoramento ativo.

---

## Arquitetura Geral

```mermaid
flowchart TD
    DEV(["👨‍💻 Developer"])

    subgraph SCM ["Controle de Versão"]
        GH["GitHub\nRepositório de código\ne configuração"]
    end

    subgraph CICD ["CI/CD"]
        GA["GitHub Actions\nPipelines reutilizáveis\nRunners self-hosted via ARC"]
    end

    subgraph INFRA ["Provisionamento de Infraestrutura"]
        BM["Base Module\nTerraform declarativo\nECR · IAM · ALB · KMS · Secrets"]
    end

    subgraph GITOPS ["GitOps"]
        ARGO["ArgoCD\nDeploy contínuo\nMultiCluster · Self-heal"]
    end

    subgraph RUNTIME ["Runtime: AWS EKS"]
        EKS["Clusters EKS\nKarpenter · Pod Identity\nNamespaces · Workloads"]
    end

    subgraph NETWORK ["Rede e Tráfego"]
        CF["Cloudflare (legado)\nDDoS · WAF · CDN"]
        ALB["AWS ALB\nLoad Balancer"]
        ING["Ingress Controller\nRoteamento interno"]
    end

    subgraph OBS ["Observabilidade"]
        NR["NewRelic\nAPM · Traces"]
        DD["Datadog\nInfraestrutura (Teachable)"]
        PROM["Prometheus\nMétricas"]
        GRAF["Grafana\nDashboards"]
        PD["JiraOps\nAlertas · On-call"]
    end

    DEV -->|"push / PR"| GH
    GH -->|"trigger"| GA
    GA -->|"terraform apply"| BM
    GA -->|"docker build + push ECR\natualiza values"| ARGO
    BM -->|"provisiona recursos AWS"| EKS
    ARGO -->|"sincroniza estado"| EKS
    EKS --> ING
    ING --> ALB
    ALB --> CF
    EKS -->|"métricas · logs · traces"| NR
    EKS -->|"métricas de infra"| DD
    EKS -->|"métricas internas"| PROM
    PROM --> GRAF
    NR --> PD
    DD --> PD
    PROM --> PD

    style DEV fill:#4A90D9,color:#fff,stroke:#2c6fad
    style GH fill:#24292E,color:#fff,stroke:#000
    style GA fill:#2088FF,color:#fff,stroke:#1a6fd4
    style BM fill:#7B42BC,color:#fff,stroke:#5e3190
    style ARGO fill:#EF7B4D,color:#fff,stroke:#d4622e
    style EKS fill:#FF9900,color:#fff,stroke:#cc7a00
    style CF fill:#F48120,color:#fff,stroke:#c4661a
    style ALB fill:#FF9900,color:#fff,stroke:#cc7a00
    style ING fill:#326CE5,color:#fff,stroke:#2456b8
    style NR fill:#1CE783,color:#000,stroke:#17b869
    style DD fill:#632CA6,color:#fff,stroke:#4e2285
    style PROM fill:#E6522C,color:#fff,stroke:#b84023
    style GRAF fill:#F46800,color:#fff,stroke:#c35300
    style PD fill:#06AC38,color:#fff,stroke:#058a2d
```

---

## Papel de cada componente

| Componente | Papel na plataforma |
|---|---|
| Developer | Escreve código, abre PRs e dispara o fluxo de deploy via commit |
| GitHub | Repositório central de código e configuração. Fonte da verdade para o estado das aplicações |
| GitHub Actions | Executa pipelines de CI/CD: build, testes, push de imagem e atualização de configuração. Runners self-hosted via ARC no próprio cluster |
| Base Module | Módulo Terraform que provisiona automaticamente toda a infraestrutura AWS necessária para uma aplicação a partir de um YAML declarativo |
| ArgoCD | Implementa o modelo GitOps: monitora repositórios de configuração e sincroniza o estado dos clusters com o estado declarado no Git |
| EKS | Clusters Kubernetes gerenciados pela AWS onde os workloads rodam. Karpenter gerencia o provisionamento automático de nodes |
| Cloudflare (legado) | Primeira camada de proteção: DDoS, WAF, CDN e terminação TLS para tráfego externo |
| ALB | Application Load Balancer da AWS que recebe o tráfego do Cloudflare (legado) e distribui para os pods |
| Ingress Controller | Gerencia o roteamento interno do cluster com base nos recursos Ingress do Kubernetes |
| NewRelic | APM e observabilidade de aplicação: traces distribuídos, taxa de erro, latência e performance |
| Datadog | Monitoramento de infraestrutura: nodes, containers, uso de recursos e rede *(exclusivo Teachable; em remoção da plataforma principal)* |
| Prometheus | Coleta métricas internas do cluster e das aplicações para alertas e dashboards |
| Grafana | Visualização de métricas em dashboards operacionais |
| JiraOps | Recebe alertas de todas as ferramentas de monitoramento e gerencia notificações e rotação de on-call |

---

## Referências

📄 [`platform-overview/platform-architecture.md`](../platform-overview/platform-architecture)
📄 [`platform-overview/eks-clusters.md`](../platform-overview/eks-clusters)
📄 [`platform-overview/observability-stack.md`](../platform-overview/observability-stack)
