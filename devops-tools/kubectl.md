# kubectl

## 📑 Índice

- [Instalação](#instalação)
- [Conceitos fundamentais](#conceitos-fundamentais)
- [Comandos essenciais](#comandos-essenciais)
- [Dicas de uso no dia a dia](#dicas-de-uso-no-dia-a-dia)

---

O `kubectl` é a ferramenta de linha de comando oficial para interagir com clusters Kubernetes. Na Hotmart, praticamente todo workload de aplicação roda em Kubernetes via Amazon EKS, o que torna o kubectl uma das ferramentas mais usadas no dia a dia do time DevOps.

Com o kubectl você consegue inspecionar o estado dos workloads, acessar logs, executar comandos dentro de containers, aplicar configurações e fazer troubleshooting de problemas em produção.

🔗 [Documentação oficial do kubectl](https://kubernetes.io/docs/reference/kubectl/)

---

## Instalação

```bash
# macOS via Homebrew
brew install kubectl

# Verificar instalação
kubectl version --client
```

Após instalar, configure o acesso aos clusters EKS da Hotmart via `hotctl`:

```bash
hotctl cluster use <cluster-name>

# Verificar contexto ativo
kubectl config current-context

# Listar todos os contextos disponíveis
kubectl config get-contexts
```

---

## Conceitos fundamentais

Antes de usar o kubectl com eficiência, é importante entender os principais objetos do Kubernetes:

| Objeto | Descrição |
|---|---|
| Pod | Unidade mínima de execução — um ou mais containers rodando juntos |
| Deployment | Gerencia réplicas de pods e estratégias de atualização |
| Service | Expõe pods internamente ou externamente via DNS estável |
| Namespace | Isolamento lógico de recursos dentro do cluster |
| Ingress | Regras de roteamento de tráfego HTTP/HTTPS externo |
| ConfigMap | Configurações não sensíveis injetadas nos pods |
| Secret | Dados sensíveis (senhas, tokens) injetados nos pods |

---

## Comandos essenciais

### Pods

```bash
# Listar todos os pods do namespace padrão
kubectl get pods

# Listar pods de um namespace específico
kubectl get pods -n <namespace>

# Listar pods de todos os namespaces
kubectl get pods -A

# Listar pods com mais detalhes (node, IP, status)
kubectl get pods -n <namespace> -o wide

# Acompanhar mudanças em tempo real
kubectl get pods -n <namespace> -w

# Filtrar pods com problema
kubectl get pods -n <namespace> | grep -v Running
```

### Logs

```bash
# Logs de um pod
kubectl logs <pod-name> -n <namespace>

# Logs em tempo real (follow)
kubectl logs -f <pod-name> -n <namespace>

# Logs de um container específico (quando o pod tem múltiplos containers)
kubectl logs <pod-name> -c <container-name> -n <namespace>

# Logs do container anterior (útil quando o pod reiniciou)
kubectl logs <pod-name> --previous -n <namespace>

# Últimas N linhas de log
kubectl logs <pod-name> -n <namespace> --tail=100
```

### Diagnóstico

```bash
# Detalhes completos de um pod (inclui eventos, condições, volumes)
kubectl describe pod <pod-name> -n <namespace>

# Detalhes de um deployment
kubectl describe deployment <deployment-name> -n <namespace>

# Eventos recentes do namespace (ordenados por tempo)
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Uso de recursos dos pods
kubectl top pods -n <namespace>

# Uso de recursos dos nodes
kubectl top nodes
```

### Deployments

```bash
# Listar deployments
kubectl get deployments -n <namespace>

# Status de um rollout em andamento
kubectl rollout status deployment/<nome> -n <namespace>

# Histórico de versões de um deployment
kubectl rollout history deployment/<nome> -n <namespace>

# Rollback para a versão anterior
kubectl rollout undo deployment/<nome> -n <namespace>

# Rollback para uma versão específica
kubectl rollout undo deployment/<nome> --to-revision=3 -n <namespace>

# Escalar réplicas manualmente
kubectl scale deployment/<nome> --replicas=5 -n <namespace>
```

### Namespaces e serviços

```bash
# Listar namespaces
kubectl get namespaces

# Listar services de um namespace
kubectl get services -n <namespace>

# Listar ingress de um namespace
kubectl get ingress -n <namespace>

# Listar todos os recursos de um namespace
kubectl get all -n <namespace>
```

### Execução dentro de containers

```bash
# Abrir shell interativo em um pod
kubectl exec -it <pod-name> -n <namespace> -- /bin/sh

# Executar um comando específico
kubectl exec <pod-name> -n <namespace> -- env

# Copiar arquivo do pod para local
kubectl cp <namespace>/<pod-name>:/path/to/file ./local-file
```

---

## Dicas de uso no dia a dia

**Sempre confirme o contexto antes de agir em produção:**
```bash
kubectl config current-context
```

**Use `-n` sempre que possível** para evitar operar no namespace errado. Omitir o namespace usa o namespace `default`, que raramente é onde as aplicações da Hotmart estão.

**Nunca use `kubectl apply` diretamente em produção.** Toda mudança deve passar pelo fluxo GitOps via ArgoCD. O kubectl em produção é para leitura e diagnóstico, não para modificação.

**Crie aliases para os comandos mais usados:**
```bash
# Adicione ao seu ~/.zshrc
alias k='kubectl'
alias kgp='kubectl get pods'
alias kgpn='kubectl get pods -n'
alias kl='kubectl logs -f'
```
