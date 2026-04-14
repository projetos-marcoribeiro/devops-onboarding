# Bem-vindo ao Time DevOps da Hotmart

## 📑 Índice

- [O que esperar dos primeiros 3 meses](#o-que-esperar-dos-primeiros-3-meses)
- [Etapas do Onboarding](#etapas-do-onboarding)
- [Por onde começar agora](#por-onde-começar-agora)
- [Referências](#referências)

---

Este repositório é o ponto de partida para todo engenheiro DevOps que está começando na Hotmart. Aqui você vai encontrar tudo que precisa para entender a plataforma, configurar seu ambiente, aprender as ferramentas e evoluir até operar com autonomia.

O onboarding tem duração aproximada de 3 meses e é estruturado de forma progressiva: cada etapa constrói a base para a próxima. O objetivo final é que você consiga operar a plataforma com autonomia, responder a incidentes e contribuir com a evolução técnica do time.

---

## O que esperar dos primeiros 3 meses

```
Dia 1               Semana 1            Semana 2-3          Mês 2-3
  │                   │                   │                   │
  ▼                   ▼                   ▼                   ▼
Kickoff &           Training Camp       Hands On            Operacional
Setup de            (~4 dias)           (1-2 semanas)       + Day by Day
Acessos             Vídeos · Slides     Deploy real         Tickets · Incidentes
                    Skill Matrix        Frontend+Backend    On-call Ready
```

---

## Etapas do Onboarding

### Kickoff: Apresentação do Time

No primeiro dia, você terá uma apresentação geral do time DevOps cobrindo:
- Estrutura e hierarquia do time
- Escopo de trabalho do DevOps na Hotmart
- Apresentação da sua Product Unit (PU)

📊 [Exemplo: DevOps Experience - PU Lead Solutions](https://docs.google.com/presentation/d/1OY9C5nSgSHQRzHNgOyXRl8rwey-7Xrdnua_rzQr5GXc/edit?usp=sharing)

### DevOps Journey

Leia o documento de jornada DevOps. Ele explica a progressão esperada desde o primeiro dia até a maturidade na plataforma, incluindo o plano de 30-60-90 dias e as expectativas trimestrais.

📄 [`devops-journey/devops-journey-map.md`](../devops-journey/devops-journey-map)

### Setup Inicial

Configure seus acessos e ferramentas. Sem isso, nada funciona. Siga os guias de acesso ao Okta, Service Desk e abra o pack completo de acessos no primeiro dia.

📄 [`getting-started/okta-access.md`](okta-access)
📄 [`getting-started/required-tools.md`](required-tools)
📄 [`getting-started/access-requests-examples.md`](access-requests-examples): pack completo de acessos

### Platform Overview

Entenda como a plataforma Hotmart está estruturada: contas AWS, clusters EKS, pipelines de CI/CD, GitOps com ArgoCD, observabilidade e o base module.

📄 [`platform-overview/platform-architecture.md`](../platform-overview/platform-architecture)

### Training Camp

Período de aprendizado guiado com learning path definido, skill matrix e exercícios práticos. Cobre Kubernetes, AWS, CI/CD, observabilidade e segurança.

📄 [`training-camp/training-overview.md`](../training-camp/training-overview)

### Hands-on Project

Projeto prático supervisionado onde você aplica o que aprendeu em um cenário real: deploy de aplicação, configuração de pipeline e troubleshooting.

📄 [`hands-on/hands-on-overview.md`](../hands-on/hands-on-overview)

### Trabalho Operacional

Participação no trabalho do dia a dia: tickets no Service Desk, deploys, manutenção e suporte a times de produto.

📄 [`operations/operational-tasks.md`](../operations/operational-tasks)

### Incident Response

Participação em incidentes reais, inicialmente como observador e depois como participante ativo. Uso de ferramentas de monitoramento, investigação de causa raiz e colaboração na resolução.

📄 [`incident-management/incident-process.md`](../incident-management/incident-process)

### On-call Readiness

Preparação para entrar na rotação de on-call: checklist de prontidão, runbooks, políticas de escalação e simulações.

📄 [`oncall-readiness/oncall-readiness-checklist.md`](../oncall-readiness/oncall-readiness-checklist)

---

## Por onde começar agora

1. Leia este documento até o fim
2. Siga o [Dia 1: Passo a Passo](dia-1-passo-a-passo): ele consolida tudo que precisa ser feito no primeiro dia na ordem certa (acessos, ferramentas, Issue de progresso, configuração do ambiente)
3. Comece a leitura do [Platform Overview](../platform-overview/platform-architecture)
4. Inicie o [Training Camp](../training-camp/training-overview)

Dúvidas? Fale com seu fellow.

---

## Referências

📄 [`devops-journey/devops-journey-map.md`](../devops-journey/devops-journey-map)
📄 [`devops-journey/30-60-90-days.md`](../devops-journey/30-60-90-days)
📄 [`devops-journey/onboarding-checklist.md`](../devops-journey/onboarding-checklist)
📄 [`getting-started/okta-access.md`](okta-access)
📄 [`getting-started/required-tools.md`](required-tools)
📄 [`getting-started/access-requests-examples.md`](access-requests-examples)
📄 [`getting-started/mapa-de-repositorios.md`](mapa-de-repositorios)
📄 [`platform-overview/platform-architecture.md`](../platform-overview/platform-architecture)
