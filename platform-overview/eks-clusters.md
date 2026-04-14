# Clusters EKS

## 📑 Índice

- [Por que EKS](#por-que-eks)
- [Karpenter: Provisionamento Automático de Nodes](#karpenter--provisionamento-automático-de-nodes)
- [EKS Pod Identity: Autenticação IAM para Pods](#eks-pod-identity--autenticação-iam-para-pods)
- [Estrutura de Clusters](#estrutura-de-clusters)
- [MultiCluster com ArgoCD](#multicluster-com-argocd)
- [Interagindo com os clusters](#interagindo-com-os-clusters)
- [Boas práticas](#boas-práticas)
- [Referências](#referências)

---

A Hotmart utiliza o Amazon EKS (Elastic Kubernetes Service) como plataforma de orquestração de containers. Praticamente todos os workloads de aplicação rodam em Kubernetes, o que torna o entendimento dos clusters EKS fundamental para qualquer engenheiro DevOps.

---

## Por que EKS

O EKS é o Kubernetes gerenciado da AWS. Ele cuida do control plane (API server, etcd, scheduler, controller manager), deixando para o time DevOps a responsabilidade de gerenciar os nodes (data plane) e os workloads que rodam neles.

Usar EKS em vez de Kubernetes self-managed reduz a carga operacional do time: atualizações do control plane, alta disponibilidade e integrações nativas com serviços AWS (IAM, ALB, EBS, EFS, etc.) são gerenciadas pela AWS.

---

## Karpenter: Provisionamento Automático de Nodes

O Karpenter é o provisionador de nodes utilizado na Hotmart. Ele substitui o Cluster Autoscaler tradicional com uma abordagem mais eficiente e flexível.

**Como funciona:**

```
Pod pendente (sem node disponível)
         │
         ▼
Karpenter analisa os requisitos do pod
(CPU, memória, arquitetura, tolerations, etc.)
         │
         ▼
Karpenter provisiona o node ideal na AWS
(tipo de instância, zona, Spot vs On-Demand)
         │
         ▼
Node fica disponível e pod é agendado
         │
         ▼
Quando o node fica ocioso por tempo suficiente,
Karpenter o remove automaticamente
```

**Vantagens do Karpenter:**
- Provisiona nodes em segundos, não minutos
- Escolhe automaticamente o tipo de instância mais adequado e econômico
- Suporta Spot Instances para redução de custo
- Consolida workloads e remove nodes ociosos automaticamente

**NodePools e NodeClasses:**

O Karpenter usa `NodePool` para definir as regras de provisionamento e `EC2NodeClass` para configurar os detalhes da instância EC2:

```yaml
apiVersion: karpenter.sh/v1
kind: NodePool
metadata:
  name: general
spec:
  template:
    spec:
      requirements:
        - key: kubernetes.io/arch
          operator: In
          values: ["amd64", "arm64"]
        - key: karpenter.sh/capacity-type
          operator: In
          values: ["spot", "on-demand"]
```

---

## EKS Pod Identity: Autenticação IAM para Pods

O EKS Pod Identity é o mecanismo utilizado para que pods dentro do cluster possam assumir IAM Roles na AWS sem precisar de credenciais estáticas.

**Como funciona:**

```
Pod precisa acessar um serviço AWS (S3, DynamoDB, etc.)
         │
         ▼
Pod tem uma ServiceAccount associada
         │
         ▼
ServiceAccount está vinculada a uma IAM Role
via EKS Pod Identity Association
         │
         ▼
AWS injeta credenciais temporárias no pod
automaticamente via token projetado
         │
         ▼
Pod acessa o serviço AWS com as permissões
da IAM Role, sem credenciais hardcoded
```

O base-module cria automaticamente as IAM Roles e as associações de Pod Identity necessárias para cada aplicação.

---

## Estrutura de Clusters

A Hotmart opera múltiplos clusters EKS organizados por critérios como ambiente, domínio e criticidade. Essa separação garante isolamento entre workloads de diferentes naturezas.

**Exemplos de organização:**

```
Clusters por ambiente:
├── cluster-production     (workloads de produção)
├── cluster-staging        (homologação e testes)
└── cluster-development    (desenvolvimento)

Clusters por domínio:
├── cluster-payments       (workloads de pagamento, alta criticidade)
├── cluster-ai             (workloads de IA, conta hot-ai)
└── cluster-platform       (ferramentas de plataforma, ArgoCD, runners)
```

---

## MultiCluster com ArgoCD

O ArgoCD na Hotmart opera em modo multicluster, o que significa que uma única instância do ArgoCD pode gerenciar deploys em múltiplos clusters simultaneamente.

**clusterSelector e contextSelector:**

Para controlar em quais clusters uma aplicação deve ser deployada, o ArgoCD usa seletores:

```yaml
# ApplicationSet com clusterSelector
spec:
  generators:
    - clusters:
        selector:
          matchLabels:
            environment: production
            region: us-east-1
```

O `contextSelector` permite selecionar clusters com base em contexto mais amplo, como domínio de negócio ou criticidade do workload.

---

## Interagindo com os clusters

**Configurando o kubeconfig via hotctl:**

```bash
# Listar clusters disponíveis
hotctl cluster list

# Configurar acesso a um cluster específico
hotctl cluster use <cluster-name>

# Verificar contexto atual
kubectl config current-context
```

**Comandos úteis do dia a dia:**

```bash
# Listar pods em um namespace
kubectl get pods -n <namespace>

# Ver logs de um pod
kubectl logs -f <pod-name> -n <namespace>

# Descrever um pod (útil para troubleshooting)
kubectl describe pod <pod-name> -n <namespace>

# Ver eventos do cluster
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Verificar uso de recursos dos nodes
kubectl top nodes

# Verificar uso de recursos dos pods
kubectl top pods -n <namespace>
```

---

## Boas práticas

- Sempre confirme o contexto do kubectl antes de executar comandos que modificam recursos em produção
- Use namespaces para isolar workloads de diferentes times e aplicações
- Nunca execute `kubectl delete` em produção sem confirmar com o time
- Para troubleshooting, prefira `kubectl describe` e `kubectl logs` antes de qualquer ação destrutiva
- Mudanças em produção devem sempre passar pelo fluxo GitOps (ArgoCD), não via `kubectl apply` direto

---

## Referências

📄 [`platform-overview/platform-architecture.md`](platform-architecture)
📄 [`platform-overview/argocd-gitops.md`](argocd-gitops)
📄 [`platform-overview/networking-ingress.md`](networking-ingress)
📄 [`devops-tools/kubectl.md`](../devops-tools/kubectl)
