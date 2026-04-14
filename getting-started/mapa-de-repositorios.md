# Mapa de Repositórios

## 📑 Índice

- [Repositórios essenciais](#repositórios-essenciais)
- [Repositórios de infraestrutura (IAC)](#repositórios-de-infraestrutura-iac)
- [Repositórios de ferramentas e automação](#repositórios-de-ferramentas-e-automação)
- [Repositórios de módulos Terraform](#repositórios-de-módulos-terraform)
- [Repositórios de onboarding e documentação](#repositórios-de-onboarding-e-documentação)
- [Como navegar](#como-navegar)

---

Este documento lista os repositórios mais importantes da organização Hotmart-Org que um engenheiro DevOps precisa conhecer. Não é uma lista exaustiva: são os repos que você vai acessar com mais frequência no dia a dia.

---

## Repositórios essenciais

Estes são os repos que você vai usar desde o primeiro dia.

| Repositório | Descrição | Frequência de uso |
|---|---|---|
| [devops-docs](https://github.com/Hotmart-Org/devops-docs) | Documentação oficial do time DevOps. Processos, procedimentos, troubleshooting, arquitetura e decisões técnicas (ADRs) | Diária |
| [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac) | Repositório central de Helm values para todas as aplicações. Onde os deploys são configurados | Diária |
| [terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module) | Módulo Terraform central que provisiona infraestrutura AWS a partir de YAML declarativo | Frequente |
| [actions](https://github.com/Hotmart-Org/actions) | Workflows reutilizáveis e actions do GitHub Actions usados por toda a organização | Frequente |
| [hotctl](https://github.com/Hotmart-Org/hotctl) | CLI interna para interagir com a plataforma: SSO, EKS, operações do dia a dia | Diária |

---

## Repositórios de infraestrutura (IAC)

Cada conta AWS ou domínio tem seu próprio repositório de infraestrutura como código. Os mais relevantes:

| Repositório | Escopo |
|---|---|
| [devops-iac](https://github.com/Hotmart-Org/devops-iac) | Infraestrutura da conta DevOps (runners, ferramentas internas) |
| [buildstaging-iac](https://github.com/Hotmart-Org/buildstaging-iac) | Infraestrutura de staging (CDN, ALB, recursos de teste) |
| [sites-iac](https://github.com/Hotmart-Org/sites-iac) | Infraestrutura de produção principal |
| [auth-iac](https://github.com/Hotmart-Org/auth-iac) | Infraestrutura de autenticação |
| [ai-iac](https://github.com/Hotmart-Org/ai-iac) | Infraestrutura da plataforma de IA |
| [analytics-iac](https://github.com/Hotmart-Org/analytics-iac) | Infraestrutura de analytics |
| [security-iac](https://github.com/Hotmart-Org/security-iac) | Infraestrutura de segurança |
| [secops-iac](https://github.com/Hotmart-Org/secops-iac) | Infraestrutura de SecOps |
| [send-iac](https://github.com/Hotmart-Org/send-iac) | Infraestrutura de envio (email, notificações) |

> Ao revisar PRs de IAC, siga o [checklist de revisão](https://github.com/Hotmart-Org/devops-docs/blob/master/docs/process/pastel/iacs-pr-review.md) do time.

---

## Repositórios de ferramentas e automação

| Repositório | Descrição |
|---|---|
| [docker-images](https://github.com/Hotmart-Org/docker-images) | Imagens Docker base mantidas pelo time DevOps |
| [hotmart-charts](https://github.com/Hotmart-Org/hotmart-charts) | Helm charts internos da Hotmart (incluindo ApplicationSet para ArgoCD) |
| [pagerduty-iac](https://github.com/Hotmart-Org/pagerduty-iac) | Infraestrutura do PagerDuty como código: times, escalas, schedules, services |
| [monitoring](https://github.com/Hotmart-Org/monitoring) | Módulo de monitoramento integrado ao base-module (NewRelic, PagerDuty, Pingdom) |
| [dynamic-agent](https://github.com/Hotmart-Org/dynamic-agent) | Agente dinâmico para automações internas |

---

## Repositórios de módulos Terraform

Módulos Terraform reutilizáveis para recursos AWS específicos:

| Repositório | Recurso |
|---|---|
| [terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module) | Módulo principal (ECR, IAM, ALB, SG, KMS, Secrets) |
| [terraform-base-module-lambda](https://github.com/Hotmart-Org/terraform-base-module-lambda) | Variante para funções Lambda |
| [terraform-base-module-agentcore](https://github.com/Hotmart-Org/terraform-base-module-agentcore) | Variante para agentes |
| [terraform-modules-aws-rds](https://github.com/Hotmart-Org/terraform-modules-aws-rds) | Módulo para RDS |
| [terraform-modules-aws-rds-aurora](https://github.com/Hotmart-Org/terraform-modules-aws-rds-aurora) | Módulo para Aurora |
| [terraform-modules-aws-cloudfront](https://github.com/Hotmart-Org/terraform-modules-aws-cloudfront) | Módulo para CloudFront |
| [terraform-modules-aws-lambda](https://github.com/Hotmart-Org/terraform-modules-aws-lambda) | Módulo para Lambda |
| [terraform-modules-aws-secrets](https://github.com/Hotmart-Org/terraform-modules-aws-secrets) | Módulo para Secrets Manager |
| [terraform-modules-aws-amq](https://github.com/Hotmart-Org/terraform-modules-aws-amq) | Módulo para Amazon MQ |
| [terraform-modules-vault](https://github.com/Hotmart-Org/terraform-modules-vault) | Módulo para Vault |

---

## Repositórios de onboarding e documentação

| Repositório | Descrição |
|---|---|
| [handson_devops](https://github.com/Hotmart-Org/handson_devops) | Projeto prático do Hands-On: deploy de frontend + backend |
| [devops-onboarding](https://github.com/projetos-marcoribeiro/devops-onboarding) | Esta plataforma de onboarding (será migrada para Hotmart-Org) |

---

## Como navegar

- Use a busca do GitHub na organização para encontrar repos específicos
- Os repos de IAC seguem o padrão `<domínio>-iac`
- Os módulos Terraform seguem o padrão `terraform-base-module-*` ou `terraform-modules-aws-*`
- PRs nos repos de IAC e helm-iac são o principal mecanismo de mudança na plataforma
- Ler PRs recentes é uma das melhores formas de entender como o time trabalha

> Explore os repositórios reais enquanto estuda. Ver código real é mais valioso do que exemplos genéricos.

---

## Portais e ferramentas internas

| Recurso | Link | Descrição |
|---|---|---|
| Service Desk | [hotmart.atlassian.net/servicedesk](https://hotmart.atlassian.net/servicedesk) | Solicitações de acesso e tickets de suporte |
| Hotmart AI | [chat.hotmart.ai](https://chat.hotmart.ai) | Assistente de IA interno (prefira ao ChatGPT para dados corporativos) |
| TeamGuide | [login.teamguide.app](https://login.teamguide.app/) | Acompanhamento de desenvolvimento, mentorias e PDI |
| PagerDuty | [hotmart.pagerduty.com](https://hotmart.pagerduty.com/) | Alertas, escalação e rotação de on-call |
| Skill Matrix | [Google Sheets](https://docs.google.com/spreadsheets/d/15WV6mGLRjC6IKTDdeiatntHq4YVk9A-Cnu5cNvQBiYg/edit) | Mapeamento de conhecimento técnico do time |
| Smart Contract | [Google Slides](https://docs.google.com/presentation/d/1SdSBnz-yxGYtGLgD18UdjWT4Pr2WCFMPlwbsW4m8p8I/edit) | Diretrizes e acordos do time DevOps |
| Treinamentos | [Google Drive](https://drive.google.com/drive/folders/1EbEAxfw6X6FBl-ARgiEbTWEOaiItUn_6) | Pasta com gravações de treinamentos internos |
