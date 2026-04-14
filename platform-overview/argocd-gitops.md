# ArgoCD e GitOps

## 📑 Índice

- [O que é GitOps](#o-que-é-gitops)
- [Fluxo de deploy via ArgoCD](#fluxo-de-deploy-via-argocd)
- [ApplicationSet](#applicationset)
- [Deploy MultiCluster](#deploy-multicluster)
- [Repositório central de values](#repositório-central-de-values)
- [Sync Policy](#sync-policy)
- [Acessando o ArgoCD](#acessando-o-argocd)
- [Quando fazer sync manual](#quando-fazer-sync-manual)
- [Referências](#referências)

---

> ⚠️ **Nota:** O ArgoCD está sendo implementado como a nova ferramenta de deploy contínuo da Hotmart, substituindo o fluxo anterior baseado em Helm upgrade/install/rollback. A adoção está em andamento e nem todos os serviços foram migrados para esse modelo.

O ArgoCD é a nova ferramenta de deploy contínuo sendo adotada na Hotmart. Ele implementa o modelo GitOps, onde o Git é a fonte da verdade para o estado desejado das aplicações nos clusters Kubernetes.

---

## O que é GitOps

GitOps é um modelo operacional onde:

- Todo o estado desejado da infraestrutura e das aplicações é declarado em repositórios Git
- Um agente (no caso, o ArgoCD) monitora esses repositórios e garante que o estado real do cluster corresponde ao estado declarado no Git
- Qualquer mudança no cluster passa por um commit no Git, garantindo rastreabilidade completa
- Rollback é simplesmente reverter um commit

Isso significa que **ninguém faz `kubectl apply` diretamente em produção**. Toda mudança passa pelo Git.

---

## Fluxo de deploy via ArgoCD

```
1. Developer faz commit no repositório
   de configuração (values, manifests, etc.)
         │
         ▼
2. ArgoCD detecta a mudança
   (polling a cada ~3 minutos ou via webhook)
         │
         ▼
3. ArgoCD compara o estado desejado (Git)
   com o estado atual do cluster
         │
         ▼
4. ArgoCD sincroniza o cluster
   aplicando as mudanças necessárias
         │
         ▼
5. ArgoCD verifica o health da aplicação
   (pods running, health checks passando)
         │
         ├── Sucesso: deploy concluído ✓
         │
         └── Falha: ArgoCD pode fazer rollback
                    automático para a versão anterior
```

---

## ApplicationSet

O `ApplicationSet` é um recurso do ArgoCD que permite criar múltiplas `Application` a partir de um único template. É especialmente útil para gerenciar deploys em múltiplos clusters ou ambientes.

**Exemplo de ApplicationSet para múltiplos ambientes:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: minha-aplicacao
spec:
  generators:
    - list:
        elements:
          - cluster: production
            url: https://k8s-prod.hotmart.internal
            environment: production
          - cluster: staging
            url: https://k8s-staging.hotmart.internal
            environment: staging
  template:
    metadata:
      name: "minha-aplicacao-{{environment}}"
    spec:
      project: default
      source:
        repoURL: https://github.com/Hotmart-Org/devops-helm-iac
        targetRevision: HEAD
        path: "apps/minha-aplicacao/{{environment}}"
      destination:
        server: "{{url}}"
        namespace: minha-aplicacao
      syncPolicy:
        automated:
          prune: false
          selfHeal: false
```

> ⚠️ **Nota:** Na implementação da Hotmart, as opções `selfHeal` e `prune` **não são utilizadas**. O `project` real depende da configuração da PU e segue a estrutura de AppProjects definida pelo time. O exemplo acima é simplificado para fins didáticos.

---

## Deploy MultiCluster

O ArgoCD na Hotmart opera em modo multicluster: uma única instância gerencia deploys em múltiplos clusters EKS. Isso centraliza a visibilidade e o controle de todos os deploys da organização.

**Clusters registrados no ArgoCD:**

Cada cluster EKS é registrado no ArgoCD com um nome e URL únicos. O ArgoCD usa as credenciais de ServiceAccount para se autenticar em cada cluster.

```bash
# Listar clusters registrados no ArgoCD
argocd cluster list

# Ver status de uma aplicação
argocd app get minha-aplicacao

# Sincronizar manualmente uma aplicação
argocd app sync minha-aplicacao

# Ver histórico de deploys
argocd app history minha-aplicacao
```

---

## Repositório central de values

Os valores de configuração das aplicações (Helm values) são centralizados em um repositório dedicado, separado do código da aplicação. Isso segue o padrão GitOps de separar o código da configuração.

🔗 [https://github.com/Hotmart-Org/devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac)

**Estrutura típica:**

```
devops-helm-iac/
└── apps/
    └── minha-aplicacao/
        ├── production/
        │   └── values.yaml
        ├── staging/
        │   └── values.yaml
        └── development/
            └── values.yaml
```

Quando o pipeline de CI/CD faz o build de uma nova imagem, ele atualiza o `values.yaml` do ambiente correspondente com o novo tag da imagem. O ArgoCD detecta essa mudança e faz o deploy automaticamente.

---

## Sync Policy

O ArgoCD pode ser configurado com diferentes políticas de sincronização. Na implementação da Hotmart, **não são utilizadas as opções `selfHeal` e `prune`**:

```yaml
syncPolicy:
  automated:
    prune: false      # não remove automaticamente recursos ausentes no Git
    selfHeal: false   # não reverte automaticamente mudanças manuais no cluster
```

Essa decisão permite maior controle sobre o que é aplicado nos clusters durante o período de adoção da ferramenta. Mudanças manuais no cluster não são revertidas automaticamente pelo ArgoCD na configuração atual.

---

## Acessando o ArgoCD

O ArgoCD tem uma interface web onde você pode visualizar o estado de todas as aplicações, histórico de deploys e logs de sincronização. O acesso é feito via SSO através do Okta.

Consulte o time DevOps para obter a URL da instância ArgoCD e solicitar o acesso necessário.

---

## Quando fazer sync manual

Em situações normais, o ArgoCD sincroniza automaticamente. Sync manual é necessário quando:

- A sincronização automática está desabilitada para um ambiente específico (comum em produção para maior controle)
- Você precisa forçar um deploy imediato sem esperar o próximo ciclo de polling
- Está fazendo troubleshooting de um deploy que falhou

```bash
# Sync manual via CLI
argocd app sync minha-aplicacao --force

# Sync com prune (remove recursos obsoletos)
argocd app sync minha-aplicacao --prune
```

---

## Referências

📄 [`platform-overview/platform-architecture.md`](platform-architecture)
📄 [`platform-overview/ci-cd-pipelines.md`](ci-cd-pipelines)
📄 [`platform-overview/eks-clusters.md`](eks-clusters)
