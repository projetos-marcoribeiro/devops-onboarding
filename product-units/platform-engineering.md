# Platform Engineering

## 📑 Índice

- [Missão](#missão)
- [Responsabilidades](#responsabilidades)
- [Componentes centrais mantidos pelo Platform Engineering](#componentes-centrais-mantidos-pelo-platform-engineering)
- [Repositórios principais](#repositórios-principais)
- [Ferramentas da plataforma](#ferramentas-da-plataforma)
- [Infraestrutura compartilhada](#infraestrutura-compartilhada)
- [Contatos](#contatos)

---

A equipe de **Platform Engineering** é responsável pela infraestrutura compartilhada, pelas ferramentas DevOps e pela experiência do desenvolvedor dentro da Hotmart. Enquanto outras PUs constroem produtos para usuários externos, o Platform Engineering constrói a plataforma que os times de engenharia usam para construir e operar seus produtos.

---

## Missão

Reduzir a carga cognitiva dos times de produto ao abstrair a complexidade de infraestrutura, padronizar processos de deploy e fornecer ferramentas que aumentam a produtividade e a confiabilidade de toda a organização de engenharia.

---

## Responsabilidades

### Infraestrutura compartilhada

- Operação e evolução dos clusters EKS utilizados por todos os times
- Gestão das contas AWS da organização e das políticas de segurança
- Networking, VPCs, roteamento e conectividade entre serviços
- Gestão de certificados TLS e DNS via Route53

### Ferramentas DevOps

- Desenvolvimento e manutenção do Base Module (terraform-base-module)
- Manutenção dos workflows reutilizáveis de CI/CD (GitHub Actions)
- Operação do ArgoCD e do modelo GitOps da plataforma
- Gestão dos runners de CI/CD via Actions Runner Controller (ARC)

### Automações de deploy

- Padronização do fluxo de deploy para todos os times
- Automação de provisionamento de infraestrutura via Base Module
- Integração entre pipelines de CI/CD e deploy via ArgoCD
- Ferramentas de rollback e gestão de versões

### Observabilidade da plataforma

- Operação da stack de observabilidade (NewRelic, Datadog, Prometheus, Grafana)
- Configuração de alertas e integração com PagerDuty
- Dashboards operacionais para o time DevOps e para os times de produto
- Definição de padrões de SLO e SLA

### Developer Experience (DevEx)

- Portal Backstage para criação e gestão de aplicações
- Documentação técnica da plataforma (este repositório)
- Ferramentas internas como o hotctl
- Redução de fricção no fluxo de trabalho dos times de produto

---

## Componentes centrais mantidos pelo Platform Engineering

| Componente | Repositório | Descrição |
|---|---|---|
| Base Module | [terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module) | Módulo Terraform para provisionamento declarativo de infraestrutura |
| GitHub Actions | [actions](https://github.com/Hotmart-Org/actions) | Workflows reutilizáveis de CI/CD |
| Helm Infrastructure | [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac) | Helm charts e values de infraestrutura |
| hotctl | [hotctl](https://github.com/Hotmart-Org/hotctl) | CLI interna para operações na plataforma |
| DevOps Docs | [devops-docs](https://github.com/Hotmart-Org/devops-docs) | Documentação técnica da plataforma |

---

## Repositórios principais

> Lista completa de repositórios mantidos pelo Platform Engineering.

| Repositório | Descrição |
|---|---|
| [terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module) | Base Module: provisionamento de infraestrutura |
| [actions](https://github.com/Hotmart-Org/actions) | Workflows reutilizáveis de GitHub Actions |
| [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac) | Helm charts de infraestrutura |
| [hotctl](https://github.com/Hotmart-Org/hotctl) | CLI interna da plataforma |
| [devops-docs](https://github.com/Hotmart-Org/devops-docs) | Documentação DevOps |

---

## Ferramentas da plataforma

| Ferramenta | Descrição | Acesso |
|---|---|---|
| ArgoCD | GitOps e deploy contínuo | Via Okta |
| Grafana | Dashboards de observabilidade | Via Okta |
| Backstage | Portal de developer experience | Via Okta |
| PagerDuty | Alertas e on-call | Via Okta |

---

## Infraestrutura compartilhada

| Componente | Descrição |
|---|---|
| Clusters EKS | Clusters Kubernetes gerenciados para todos os times |
| Karpenter | Provisionamento automático de nodes |
| Actions Runner Controller | Runners self-hosted de CI/CD |
| External DNS | Gestão automática de registros DNS |
| AWS Load Balancer Controller | Provisionamento automático de ALBs |
| Cert Manager | Gestão de certificados TLS |

---

## Contatos

> Engenheiros de referência e canais de comunicação do Platform Engineering.

| Papel | Contato |
|---|---|
| Tech Lead | - |
| Canal no Google Chat | - |

---

> O Platform Engineering é o time que constrói e mantém a plataforma que você usa todos os dias. Se você identificar algo que pode ser melhorado: uma ferramenta que poderia ser mais simples, um processo que poderia ser automatizado, uma documentação que está desatualizada: traga para o time. Melhorias na plataforma beneficiam toda a organização de engenharia.
