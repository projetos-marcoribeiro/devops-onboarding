# Hands-On Project

## 📑 Índice

- [Por que o Hands-On existe](#por-que-o-hands-on-existe)
- [O que o Hands-On envolve](#o-que-o-hands-on-envolve)
- [Onde encontrar o projeto](#onde-encontrar-o-projeto)
- [Próximos passos](#próximos-passos)

---

O Hands-On é a etapa prática do onboarding DevOps da Hotmart. Depois de estudar a plataforma no Training Camp, é hora de colocar a mão na massa e realizar um deploy real utilizando os mesmos padrões, ferramentas e fluxos que o time usa no dia a dia.

O objetivo não é criar uma aplicação complexa — é entender o ciclo completo de deploy da plataforma, do código ao cluster.

---

## Por que o Hands-On existe

Ler documentação e assistir apresentações é necessário, mas não suficiente. O Hands-On existe porque:

- Operar uma plataforma exige familiaridade prática, não só teórica
- Errar em um ambiente controlado é muito melhor do que errar em produção
- O ciclo completo de deploy envolve muitas peças que só fazem sentido quando você as conecta na prática
- Dúvidas reais só aparecem quando você tenta fazer algo funcionar

---

## O que o Hands-On envolve

```
Developer
    │
    │  escreve código, Dockerfile, configurações
    ▼
GitHub Repository
    │
    │  push dispara o pipeline automaticamente
    ▼
GitHub Actions Pipeline
    │  build da imagem Docker
    │  push para ECR
    │  execução de workflows reutilizáveis
    ▼
Base Module (Terraform)
    │  provisionamento de infraestrutura
    │  ECR, IAM Roles, ALB, Security Groups
    ▼
ArgoCD Deploy
    │  detecta mudança no repositório de configuração
    │  sincroniza o estado desejado no cluster
    ▼
EKS Cluster
       aplicação rodando em produção
```

### Criação de imagem Docker

O engenheiro escreve um `Dockerfile` para containerizar a aplicação backend. Isso envolve entender como construir uma imagem eficiente, escolher a imagem base correta e garantir que a aplicação roda corretamente dentro do container.

### Configuração de pipeline GitHub Actions

O pipeline de CI/CD é configurado usando os workflows reutilizáveis da organização. O engenheiro aprende como referenciar esses workflows, passar os parâmetros corretos e entender o que cada step faz.

### Uso do Base Module

A infraestrutura necessária para a aplicação é declarada em um YAML e provisionada via Base Module. O engenheiro entende na prática quais recursos são criados automaticamente e como configurá-los.

### Deploy via ArgoCD

O deploy é gerenciado pelo ArgoCD seguindo o modelo GitOps. O engenheiro aprende a acompanhar o status do deploy, interpretar o health da aplicação e entender o que acontece quando algo dá errado.

### Execução em EKS

A aplicação roda em um cluster Kubernetes gerenciado pelo EKS. O engenheiro pratica os comandos básicos do kubectl para verificar o status dos pods, ler logs e fazer troubleshooting.

---

## Onde encontrar o projeto

🔗 [https://github.com/Hotmart-Org/handson_devops](https://github.com/Hotmart-Org/handson_devops)

O repositório contém as instruções detalhadas, o código base da aplicação e os templates de configuração necessários para completar o projeto.

---

## Próximos passos

📄 [`hands-on/hands-on-project.md`](hands-on-project) — detalhes do projeto
📄 [`hands-on/deployment-workflow.md`](deployment-workflow) — fluxo completo de deploy
📄 [`hands-on/expected-results.md`](expected-results) — o que é esperado ao final
