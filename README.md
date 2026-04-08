# DevOps Onboarding – Hotmart

> 📅 Última atualização: Abril 2026

## Boas-vindas

Seja bem-vindo ao time DevOps da Hotmart.

A plataforma tem muitas camadas e leva tempo para entender tudo — isso é normal e esperado. Este material foi construído para tornar esse processo mais suave, mas o aprendizado real vem da prática e das conversas com o time.

Não hesite em perguntar. Seu fellow está aqui para ajudar, e o time tem uma cultura de compartilhamento de conhecimento. Dúvidas não resolvidas viram gaps que aparecem na hora errada — traga-as à tona cedo.

Esperamos que este material ajude você a entender melhor a plataforma e a se integrar rapidamente ao time. Caso tenha dúvidas, procure seu fellow ou qualquer outro membro do time. Estamos todos aqui para ajudar.

## 📑 Índice

- [🚀 Começar agora](#-começar-agora)
- [📚 Estrutura da Documentação](#-estrutura-da-documentação)
- [Objetivo do Repositório](#objetivo-do-repositório)
- [Jornada do Onboarding](#jornada-do-onboarding)
- [Tempo Estimado do Onboarding](#tempo-estimado-do-onboarding)
- [Principais Ferramentas Utilizadas](#principais-ferramentas-utilizadas)
- [Links Importantes](#links-importantes)
- [Contribuindo com a Documentação](#contribuindo-com-a-documentação)
- [Boas-vindas](#boas-vindas)

---

Bem-vindo ao repositório de onboarding do time DevOps da Hotmart. Aqui você encontra toda a documentação necessária para entender a plataforma, as ferramentas e os processos da empresa — do primeiro dia até a autonomia operacional completa.

Este repositório é sincronizado automaticamente com o portal de documentação **TechDeck** e serve como fonte da verdade para o onboarding de novos engenheiros e como referência contínua para todo o time.

---

## 🚀 Começar agora

Se você acabou de entrar no time, comece por aqui:

- ➡️ **[Getting Started](getting-started/introduction)** — acessos, ferramentas e setup do ambiente
- ➡️ **[Platform Overview](platform-overview/platform-architecture)** — arquitetura, AWS, EKS, CI/CD e GitOps
- ➡️ **[Training Camp](training-camp/training-overview)** — trilha de aprendizado guiado e skill matrix
- ➡️ **[Hands-On Project](hands-on/hands-on-overview)** — deploy completo supervisionado
- ➡️ **[Operations](operations/jira-workflows)** — trabalho operacional do dia a dia
- ➡️ **[Incident Management](incident-management/incident-process)** — resposta a incidentes e postmortem
- ➡️ **[On-call Readiness](oncall-readiness/oncall-readiness-checklist)** — checklist de prontidão para o plantão

### Outros recursos úteis

- 📖 **[DevOps Journey](devops-journey/devops-journey-map)** — plano 30-60-90 dias e expectativas trimestrais
- 🔧 **[DevOps Tools](devops-tools/kubectl)** — kubectl, AWS CLI, hotctl, Docker
- 👥 **[Team Resources](team-resources/teamguide)** — mentoria, comunicação e TeamGuide
- 🏢 **[Product Units](product-units/overview)** — recursos específicos de cada PU
- 📘 **[Glossário](glossary/devops-glossary)** — termos e siglas da plataforma
- 📊 **[Diagramas](diagrams/devops-journey-map)** — arquitetura, deploy e resposta a incidentes em Mermaid

---

## 📚 Estrutura da Documentação

Cada pasta cobre uma parte específica da jornada de onboarding DevOps:

```
devops-onboarding/
│
├── devops-journey/          # Jornada de evolução, plano 30-60-90 dias e expectativas trimestrais
├── getting-started/         # Primeiros passos: acessos, ferramentas e links essenciais
├── platform-overview/       # Arquitetura da plataforma: AWS, EKS, CI/CD, GitOps, observabilidade
├── training-camp/           # Trilha de aprendizado guiado e skill matrix
├── hands-on/                # Projeto prático supervisionado de deploy completo
├── operations/              # Trabalho operacional: Jira, Service Desk, tarefas do dia a dia
├── incident-management/     # Processo de incidentes, PagerDuty, troubleshooting e postmortem
├── oncall-readiness/        # Checklist de prontidão, guia de investigação e escalation policies
├── devops-tools/            # Documentação das ferramentas: kubectl, AWS CLI, hotctl, Docker
├── team-resources/          # TeamGuide, processo de mentoria e comunicação do time
├── product-units/           # Recursos específicos de cada Product Unit
├── glossary/                # Glossário de termos e siglas da plataforma
└── diagrams/                # Diagramas Mermaid da arquitetura, deploy e resposta a incidentes
```

A documentação segue a ordem natural do onboarding: cada pasta representa uma etapa da jornada, do setup inicial até a maturidade operacional. Você não precisa ler tudo de uma vez — siga o fluxo da jornada e consulte as seções conforme avança.

---

## Objetivo do Repositório

Este repositório existe para:

- Acelerar o onboarding de novos engenheiros DevOps, reduzindo o tempo até a primeira contribuição real
- Documentar a arquitetura da plataforma de forma clara e acessível
- Explicar as ferramentas utilizadas pelo time e como elas se integram
- Registrar processos operacionais, fluxos de incidente e boas práticas
- Servir como referência contínua para o time DevOps, não apenas para quem está chegando

---

## Jornada do Onboarding

O onboarding DevOps na Hotmart é estruturado em etapas progressivas. Cada fase constrói a base para a próxima.

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
   Iniciativas trimestrais · Novas tecnologias · Evolução da plataforma
         │
         ▼
🚀 Engenheiro Maduro na Plataforma
```

---

## Tempo Estimado do Onboarding

O onboarding completo tem duração aproximada de **3 meses**. O ritmo é ajustado conforme o background de cada engenheiro, sempre com suporte do mentor (fellow).

| Período | Foco |
|---|---|
| Primeiras semanas | Setup de ambiente, solicitação de acessos, leitura do Platform Overview e início do Training Camp |
| Primeiro mês | Conclusão do Training Camp e execução do Hands-on Project supervisionado |
| Segundo mês | Participação em tarefas operacionais reais, acompanhamento de incidentes e primeiros tickets no Service Desk |
| Terceiro mês | Autonomia parcial nas operações, preparação para on-call e início de contribuição em projetos trimestrais |

Para mais detalhes, consulte o plano completo:
📄 [`devops-journey/30-60-90-days.md`](devops-journey/30-60-90-days)

---

## Principais Ferramentas Utilizadas

| Ferramenta | Papel na plataforma |
|---|---|
| GitHub | Repositório central de código e configuração. Fonte da verdade para o estado das aplicações |
| GitHub Actions | CI/CD com workflows reutilizáveis e runners self-hosted via ARC no próprio cluster |
| AWS | Infraestrutura cloud onde toda a plataforma roda. Múltiplas contas organizadas por domínio |
| Kubernetes (EKS) | Orquestração de containers. Todos os workloads rodam em clusters EKS gerenciados |
| ArgoCD | Deploy contínuo via GitOps. Sincroniza o estado dos clusters com o estado declarado no Git |
| Terraform (Base Module) | Provisionamento declarativo de infraestrutura AWS via YAML. Componente central da plataforma |
| NewRelic | APM e observabilidade de aplicação: traces distribuídos, taxa de erro e latência |
| Datadog | Monitoramento de infraestrutura: nodes, containers e uso de recursos |
| PagerDuty | Gerenciamento de alertas e rotação de on-call. Ponto central de notificação de incidentes |
| hotctl | CLI interna para interagir com a infraestrutura da plataforma Hotmart |
| Vault | Gerenciamento de secrets e credenciais sensíveis |
| Cloudflare | Proteção DDoS, WAF, CDN e controle de tráfego externo |

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

## Contribuindo com a Documentação

A documentação é um produto do time — não de uma pessoa. Todo engenheiro DevOps é encorajado a contribuir com melhorias.

**Como contribuir:**

- Abra um Pull Request com a melhoria ou correção diretamente neste repositório
- Atualize a documentação sempre que houver mudanças relevantes na plataforma — documentação desatualizada é pior do que documentação inexistente
- Registre novos aprendizados: se você resolveu um problema que não estava documentado, documente. A próxima pessoa vai agradecer.
- Se encontrar algo errado ou desatualizado mas não tiver tempo de corrigir agora, abra uma issue descrevendo o problema

A documentação evolui junto com a plataforma. Manter esse repositório atualizado é uma responsabilidade coletiva do time.


