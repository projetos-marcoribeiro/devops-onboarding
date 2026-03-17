# Skill Matrix

## 📑 Índice

- [Para que serve](#para-que-serve)
- [Como usar durante o onboarding](#como-usar-durante-o-onboarding)
- [Escala de avaliação](#escala-de-avaliação)
- [Áreas avaliadas](#áreas-avaliadas)
- [Usando a Skill Matrix com o mentor](#usando-a-skill-matrix-com-o-mentor)

---

A Skill Matrix é a ferramenta utilizada pelo time DevOps para mapear o nível de conhecimento de cada engenheiro nas tecnologias e práticas utilizadas na plataforma Hotmart.

🔗 [Skill Matrix — Google Sheets](https://docs.google.com/spreadsheets/d/15WV6mGLRjC6IKTDdeiatntHq4YVk9A-Cnu5cNvQBiYg/edit)

---

## Para que serve

A Skill Matrix não é uma avaliação de desempenho — é uma ferramenta de desenvolvimento. Ela serve para:

- **Identificar lacunas de conhecimento** antes que elas se tornem problemas operacionais
- **Direcionar treinamentos** para as áreas onde o engenheiro mais precisa evoluir
- **Planejar a evolução técnica** ao longo do tempo, com metas claras
- **Facilitar conversas com o mentor** sobre prioridades de aprendizado
- **Dar visibilidade ao time** sobre as competências disponíveis e onde há gaps coletivos

---

## Como usar durante o onboarding

No início do onboarding, o engenheiro preenche a Skill Matrix com uma autoavaliação honesta do seu nível atual em cada área. Não existe resposta certa ou errada — o objetivo é ter um ponto de partida realista.

A partir dessa avaliação, o mentor ajuda a definir quais áreas priorizar durante o Training Camp e o Hands-on Project.

A Skill Matrix deve ser revisitada ao final de cada fase do onboarding e periodicamente ao longo da jornada na empresa.

---

## Escala de avaliação

| Nível | Descrição |
|---|---|
| 0 — Sem conhecimento | Nunca trabalhou com o tema |
| 1 — Iniciante | Conhecimento teórico básico, pouca ou nenhuma prática |
| 2 — Básico | Consegue executar tarefas simples com orientação |
| 3 — Intermediário | Consegue trabalhar de forma autônoma em tarefas do dia a dia |
| 4 — Avançado | Domina o tema, consegue resolver problemas complexos e orientar outros |
| 5 — Especialista | Referência no time, contribui com evolução da plataforma nessa área |

---

## Áreas avaliadas

### Kubernetes

Conhecimento de orquestração de containers com Kubernetes, incluindo conceitos fundamentais e operação prática no ambiente EKS da Hotmart.

Exemplos de tópicos avaliados:
- Pods, Deployments, Services, Ingress, ConfigMaps, Secrets
- Namespaces e RBAC
- Karpenter e autoscaling
- Troubleshooting de workloads
- EKS Pod Identity

### Terraform

Conhecimento de Infrastructure as Code com Terraform, com foco no Base Module da Hotmart.

Exemplos de tópicos avaliados:
- Sintaxe HCL e estrutura de módulos
- State management e backends remotos
- Base Module e YAML declarativo
- Boas práticas de IaC

### Observability

Capacidade de monitorar, investigar e interpretar o comportamento de sistemas em produção.

Exemplos de tópicos avaliados:
- NewRelic (APM, traces, alertas)
- Datadog (infraestrutura, containers)
- Prometheus e Grafana
- PagerDuty (alertas e on-call)
- Definição e acompanhamento de SLOs

### CI/CD

Conhecimento de pipelines de integração e entrega contínua.

Exemplos de tópicos avaliados:
- GitHub Actions (workflows, jobs, steps)
- Workflows reutilizáveis da organização
- Actions Runner Controller (ARC)
- OIDC para autenticação AWS
- ArgoCD e GitOps

### AWS Infrastructure

Conhecimento dos serviços AWS utilizados na plataforma Hotmart.

Exemplos de tópicos avaliados:
- EKS, EC2, S3, RDS, SQS, Lambda
- IAM (roles, policies, trust relationships)
- VPC, Security Groups, ALB
- Secrets Manager, KMS
- Multi-account e AWS Organizations

### Security

Conhecimento de práticas e ferramentas de segurança aplicadas à infraestrutura.

Exemplos de tópicos avaliados:
- Secrets management (Vault, Secrets Manager)
- IAM least privilege
- Segurança em containers e Kubernetes (RBAC, Pod Security)
- Cloudflare (WAF, DDoS protection)
- SIEM e auditoria

---

## Usando a Skill Matrix com o mentor

A conversa com o mentor sobre a Skill Matrix deve acontecer:

1. **No início do onboarding** — para definir o ponto de partida e as prioridades do Training Camp
2. **Ao final do Training Camp** — para avaliar o progresso e ajustar o foco do Hands-on Project
3. **Ao final dos 90 dias** — para ter uma visão clara da evolução e planejar os próximos passos
4. **Periodicamente** — como parte do desenvolvimento contínuo no time

A Skill Matrix é uma ferramenta de conversa, não um formulário burocrático. Use-a para ter discussões honestas sobre onde você está e para onde quer ir.
