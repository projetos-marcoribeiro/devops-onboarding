# DevOps Onboarding – Hotmart

Seja bem-vindo ao time DevOps da Hotmart.

A plataforma tem muitas camadas e leva tempo para entender tudo: isso é normal e esperado. Este material foi construído para tornar esse processo mais suave, mas o aprendizado real vem da prática e das conversas com o time.

Não hesite em perguntar. Seu fellow está aqui para ajudar, e o time tem uma cultura de compartilhamento de conhecimento. Dúvidas não resolvidas viram gaps que aparecem na hora errada: traga-as à tona cedo.

Este repositório é sincronizado automaticamente com o portal de documentação **TechDeck** e serve como fonte da verdade para o onboarding de novos engenheiros e como referência contínua para todo o time.

---

## 🚀 Começar agora

Se você acabou de entrar no time, comece por aqui:

- ➡️ **[Getting Started](getting-started/introduction)**: acessos, ferramentas e setup do ambiente
- ➡️ **[Platform Overview](platform-overview/platform-architecture)**: arquitetura, AWS, EKS, CI/CD e GitOps
- ➡️ **[Training Camp](training-camp/training-overview)**: trilha de aprendizado guiado e skill matrix
- ➡️ **[Hands-On Project](hands-on/hands-on-overview)**: deploy completo supervisionado
- ➡️ **[Operations](operations/jira-workflows)**: trabalho operacional do dia a dia
- ➡️ **[Incident Management](incident-management/incident-process)**: resposta a incidentes e postmortem
- ➡️ **[On-call Readiness](oncall-readiness/oncall-readiness-checklist)**: checklist de prontidão para o plantão

### Outros recursos úteis

- 📖 **[DevOps Journey](devops-journey/devops-journey-map)**: plano 30-60-90 dias e expectativas trimestrais
- 🔧 **[DevOps Tools](devops-tools/kubectl)**: kubectl, AWS CLI, hotctl, Docker
- 👥 **[Team Resources](team-resources/teamguide)**: mentoria, comunicação e TeamGuide
- 🏢 **[Product Units](product-units/overview)**: recursos específicos de cada PU
- 📘 **[Glossário](glossary/devops-glossary)**: termos e siglas da plataforma
- 📊 **[Diagramas](diagrams/devops-journey-map)**: arquitetura, deploy e resposta a incidentes em Mermaid

---

## 📚 Estrutura da Documentação

```
devops-onboarding/
│
├── devops-journey/          # Jornada, plano 30-60-90 dias, checklist de progresso
├── getting-started/         # Acessos, ferramentas, FAQ e links essenciais
├── platform-overview/       # Arquitetura: AWS, EKS, CI/CD, GitOps, observabilidade
├── training-camp/           # Trilha de aprendizado, vídeos, skill matrix
├── hands-on/                # Projeto prático: deploy frontend + backend
├── operations/              # Jira, Service Desk, tarefas do dia a dia
├── incident-management/     # Incidentes, PagerDuty, troubleshooting, postmortem
├── oncall-readiness/        # Checklist de prontidão, investigação, escalation
├── devops-tools/            # kubectl, AWS CLI, hotctl, Docker, Rancher Desktop
├── team-resources/          # TeamGuide, mentoria, comunicação
├── product-units/           # Recursos específicos de cada Product Unit
├── glossary/                # Glossário de termos e siglas
└── diagrams/                # Diagramas Mermaid: arquitetura, deploy, incidentes
```

Siga o fluxo da jornada e consulte as seções conforme avança. Não precisa ler tudo de uma vez.

---

## Jornada do Onboarding

```
👤 Novo Engenheiro DevOps
         │
         ▼
📋 Getting Started
   Acessos · Ferramentas · Setup do ambiente
         │
         ▼
🏗️ Platform Overview
   Arquitetura · AWS · EKS · CI/CD · GitOps · Observabilidade
         │
         ▼
🎓 Training Camp
   Base Module · Monitoramento · Secrets · Troubleshooting
         │
         ▼
🛠️ Hands-on Project
   Deploy completo supervisionado · Frontend + Backend
         │
         ▼
⚙️ Operations
   Tickets · Deploys · Service Desk · Tarefas operacionais
         │
         ▼
🚨 Incident Management
   Resposta a incidentes · Investigação · Postmortem
         │
         ▼
📟 On-call Readiness
   Checklist · Runbooks · Escalation · Plantão
         │
         ▼
📅 Quarter Projects / POCs
   Iniciativas trimestrais · Novas tecnologias
         │
         ▼
🚀 Engenheiro Maduro na Plataforma
```

---

## Tempo Estimado

O onboarding completo dura aproximadamente **3 meses**, ajustado conforme o background de cada engenheiro.

| Período | Foco |
|---|---|
| Dia 1 | Kickoff, setup de acessos, abertura de tickets no Service Desk |
| Semana 1 | Training Camp (~4 dias): vídeos, slides, Skill Matrix |
| Semana 2-3 | Hands-On Project: deploy real de frontend + backend |
| Mês 2 | Trabalho operacional: tickets, deploys, acompanhamento de incidentes |
| Mês 3 | Autonomia parcial, preparação para on-call, projetos trimestrais |

📄 [Plano detalhado: 30-60-90 dias](devops-journey/30-60-90-days)
📄 [Checklist de progresso](devops-journey/onboarding-checklist)

---

## Principais Ferramentas

| Ferramenta | Papel na plataforma |
|---|---|
| GitHub | Repositório central de código e configuração |
| GitHub Actions | CI/CD com workflows reutilizáveis e runners self-hosted via ARC |
| AWS | Infraestrutura cloud. Múltiplas contas organizadas por domínio |
| Kubernetes (EKS) | Orquestração de containers. Todos os workloads rodam em EKS |
| ArgoCD | Deploy contínuo via GitOps (em implementação) |
| Terraform (Base Module) | Provisionamento declarativo de infraestrutura AWS via YAML |
| NewRelic | APM: traces distribuídos, taxa de erro e latência |
| Datadog | Monitoramento de infraestrutura (Teachable) |
| Kubecost | Monitoramento de custos em Kubernetes |
| Sentry | Rastreamento de erros e exceções |
| PagerDuty | Alertas e rotação de on-call |
| hotctl | CLI interna para interagir com a plataforma |
| Vault | Gerenciamento de secrets |
| Cloudflare | Proteção DDoS, WAF, CDN |

---

## Links Importantes

| Recurso | Link |
|---|---|
| Service Desk | [hotmart.atlassian.net/servicedesk](https://hotmart.atlassian.net/servicedesk) |
| DevOps Documentation | [github.com/Hotmart-Org/devops-docs](https://github.com/Hotmart-Org/devops-docs) |
| Terraform Base Module | [github.com/Hotmart-Org/terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module) |
| GitHub Actions | [github.com/Hotmart-Org/actions](https://github.com/Hotmart-Org/actions) |
| Helm Infrastructure | [github.com/Hotmart-Org/devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac) |
| Hotmart AI | [chat.hotmart.ai](https://chat.hotmart.ai) |
| TeamGuide | [login.teamguide.app](https://login.teamguide.app/) |

---

## Contribuindo

A documentação é um produto do time. Todo engenheiro DevOps é encorajado a contribuir:

- Abra um Pull Request com a melhoria ou correção
- Atualize a documentação quando houver mudanças na plataforma
- Se resolveu um problema que não estava documentado, documente
- Se encontrar algo errado mas não tiver tempo de corrigir, abra uma Issue

A documentação evolui junto com a plataforma. Manter esse repositório atualizado é responsabilidade coletiva.
