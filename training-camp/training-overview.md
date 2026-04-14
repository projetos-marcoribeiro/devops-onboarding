# Training Camp

## 📑 Índice

- [Onde o Training Camp se encaixa na jornada](#onde-o-training-camp-se-encaixa-na-jornada)
- [Quando acontece](#quando-acontece)
- [O que é coberto](#o-que-é-coberto)
- [Como aproveitar bem o Training Camp](#como-aproveitar-bem-o-training-camp)

---

O Training Camp é a fase estruturada de aprendizado do onboarding DevOps da Hotmart. Antes de começar a atuar em atividades operacionais reais, o engenheiro passa por um período dedicado a entender os principais conceitos, ferramentas e padrões utilizados na infraestrutura da empresa.

O objetivo não é decorar documentação: é construir o entendimento necessário para operar com segurança e autonomia na plataforma.

---

## Onde o Training Camp se encaixa na jornada

```
Onboarding
    │
    ├── Setup e Acessos
    ├── Platform Overview
    │
    ▼
Training Camp
    │  estudo guiado, labs, discussões com mentor
    ▼
Conhecimento da Plataforma
    │  base module, observabilidade, secrets, troubleshooting
    ▼
Hands-on Project
    │  aplicação prática supervisionada
    ▼
Trabalho Operacional
       tickets, deploys, suporte, incidentes
```

---

## Quando acontece

O Training Camp normalmente ocorre nas primeiras semanas do onboarding, logo após o setup inicial de acessos e ferramentas e a leitura do Platform Overview.

Não existe uma duração fixa: o ritmo é ajustado conforme o background do engenheiro e as dúvidas que surgem ao longo do estudo. O mentor (fellow) acompanha o progresso e ajuda a calibrar o ritmo.

---

## O que é coberto

### Base Module

O Base Module é o módulo Terraform central da plataforma. Entender como ele funciona é fundamental porque praticamente toda infraestrutura de aplicação na Hotmart é provisionada através dele. O estudo cobre a estrutura do módulo, o YAML declarativo, os recursos criados automaticamente e as variantes disponíveis.

📄 [`platform-overview/base-module.md`](../platform-overview/base-module)

### Observabilidade

Como as aplicações são monitoradas na Hotmart: NewRelic para APM, Prometheus para métricas internas, Grafana para visualização, Sentry para error tracking, Kubecost para custos, Pingdom para uptime externo e JiraOps para alertas e gestão de incidentes (substituindo o PagerDuty). O Datadog é utilizado apenas pela PU Teachable. O engenheiro aprende a navegar nas ferramentas e a interpretar o que está vendo.

📄 [`platform-overview/observability-stack.md`](../platform-overview/observability-stack)

### Secrets Management

Como credenciais e dados sensíveis são gerenciados na plataforma: Vault, Secrets Manager e as integrações com o base-module. Entender o ciclo de vida de um secret (criação, rotação, acesso e revogação) é essencial para operar com segurança.

### Troubleshooting

Metodologias e ferramentas para investigar problemas em ambientes distribuídos. Isso inclui como ler logs, interpretar métricas, usar o kubectl para inspecionar workloads e seguir um raciocínio estruturado de causa raiz.

### Infraestrutura Kubernetes

Conceitos práticos de Kubernetes aplicados à plataforma Hotmart: pods, deployments, services, ingress, namespaces, Karpenter, EKS Pod Identity e como tudo isso se conecta.

📄 [`platform-overview/eks-clusters.md`](../platform-overview/eks-clusters)

### Workflows DevOps

Como os pipelines de CI/CD funcionam, os workflows reutilizáveis da organização e o fluxo completo de um deploy do commit ao cluster. O ArgoCD está sendo adotado como o novo modelo de deploy via GitOps, substituindo o fluxo anterior baseado em Helm upgrade/install/rollback.

📄 [`platform-overview/ci-cd-pipelines.md`](../platform-overview/ci-cd-pipelines)
📄 [`platform-overview/argocd-gitops.md`](../platform-overview/argocd-gitops)

---

## Como aproveitar bem o Training Camp

- Não tente absorver tudo de uma vez. Siga o learning path na ordem recomendada.
- Teste os comandos e conceitos na prática, não apenas leia.
- Anote dúvidas e discuta com o mentor. Dúvidas não resolvidas viram gaps que aparecem na hora errada.
- Explore os repositórios reais da organização enquanto estuda. Ver código real é mais valioso do que exemplos genéricos.
- Use a Skill Matrix para acompanhar seu próprio progresso e identificar onde focar mais.

📄 [`training-camp/learning-path.md`](learning-path)
📄 [`training-camp/skill-matrix.md`](skill-matrix)
📄 [`training-camp/recommended-materials.md`](recommended-materials)
