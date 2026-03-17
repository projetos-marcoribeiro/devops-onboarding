# Plataforma de IA

## 📑 Índice

- [Conta AWS dedicada — hot-ai](#conta-aws-dedicada--hot-ai)
- [Amazon Bedrock](#amazon-bedrock)
- [AgentCore](#agentcore)
- [Dynamic Agents](#dynamic-agents)
- [Deploy via base-module-agentcore](#deploy-via-base-module-agentcore)
- [Arquitetura ARM64](#arquitetura-arm64)
- [Integração com a plataforma DevOps](#integração-com-a-plataforma-devops)
- [Acesso à conta hot-ai](#acesso-à-conta-hot-ai)
- [Referências](#referências)

---

A Hotmart possui uma camada dedicada de infraestrutura para workloads de Inteligência Artificial. Essa camada é isolada em uma conta AWS própria e utiliza serviços especializados da AWS para construção, deploy e operação de agentes e modelos de IA.

---

## Conta AWS dedicada — hot-ai

Todos os workloads de IA rodam na conta AWS `hot-ai`, separada das demais contas da organização. Essa separação existe por razões de:

- **Isolamento de custo:** workloads de IA podem ter custos significativos e variáveis. Ter uma conta dedicada permite visibilidade e controle precisos.
- **Segurança:** modelos e dados de IA podem ser sensíveis. O isolamento em conta própria reduz a superfície de ataque.
- **Governança:** políticas específicas para uso de IA (acesso a modelos, limites de uso, auditoria) podem ser aplicadas na conta sem afetar outras áreas.
- **Otimização de infraestrutura:** workloads de IA têm requisitos específicos de hardware (ARM64, instâncias com GPU, etc.) que são mais fáceis de gerenciar em uma conta dedicada.

---

## Amazon Bedrock

O Amazon Bedrock é o serviço AWS utilizado para acesso a modelos de linguagem e outros modelos de IA. Ele fornece uma API unificada para interagir com modelos de diferentes provedores (Anthropic Claude, Amazon Titan, Meta Llama, etc.) sem precisar gerenciar a infraestrutura dos modelos.

**Como é usado na Hotmart:**
- Acesso a modelos de linguagem para features de produto
- Base para construção de agentes de IA
- Processamento de linguagem natural em pipelines de dados
- Geração de conteúdo e automações inteligentes

---

## AgentCore

O AgentCore é o framework da AWS para construção e operação de agentes de IA em produção. Ele fornece a infraestrutura necessária para que agentes de IA rodem de forma confiável, escalável e observável.

**Componentes do AgentCore:**
- Runtime gerenciado para execução de agentes
- Gerenciamento de memória e contexto dos agentes
- Integração nativa com Bedrock para acesso a modelos
- Observabilidade e logging específicos para agentes de IA
- Controles de segurança e guardrails

---

## Dynamic Agents

Dynamic Agents são agentes de IA que podem ser criados, configurados e destruídos dinamicamente conforme a demanda. Na Hotmart, esse modelo permite:

- Escalar o número de agentes conforme o volume de requisições
- Isolar contextos de diferentes usuários ou sessões
- Atualizar a configuração dos agentes sem downtime
- Controlar custos pagando apenas pelo tempo de execução efetivo

---

## Deploy via base-module-agentcore

Workloads de IA são deployados usando a variante `base-module-agentcore` do terraform-base-module. Essa variante é especializada para os requisitos específicos de agentes de IA:

```yaml
# Exemplo de configuração para um agente de IA
app:
  name: meu-agente-ia
  type: agentcore
  team: ai-platform

bedrock:
  model: anthropic.claude-3-5-sonnet-20241022-v2:0
  region: us-east-1

agentcore:
  memory: enabled
  guardrails: enabled
  max_tokens: 4096

compute:
  architecture: arm64   # workloads de IA rodam em ARM64
  instance_type: c7g    # instâncias Graviton para melhor custo/performance

monitoring:
  enabled: true
  custom_metrics:
    - latency_p99
    - token_usage
    - error_rate
```

---

## Arquitetura ARM64

Os workloads de IA na Hotmart rodam em instâncias ARM64 (AWS Graviton). Essa escolha é motivada por:

- **Custo:** instâncias Graviton oferecem melhor relação custo/performance para workloads de inferência
- **Performance:** processadores ARM64 modernos têm boa performance para operações de matriz usadas em IA
- **Eficiência energética:** menor consumo de energia por operação

Ao fazer deploy de workloads de IA, certifique-se de que as imagens Docker são buildadas para `linux/arm64`:

```yaml
# No workflow de CI/CD
- name: Build multi-arch image
  run: |
    docker buildx build \
      --platform linux/arm64 \
      --tag $ECR_REGISTRY/$IMAGE_NAME:$TAG \
      --push .
```

---

## Integração com a plataforma DevOps

A plataforma de IA não é um silo separado — ela se integra com os mesmos componentes da plataforma DevOps:

```
Código do agente de IA
         │
         ▼
GitHub Actions (CI/CD)
         │  build da imagem ARM64, push para ECR
         ▼
base-module-agentcore (Terraform)
         │  provisiona infraestrutura AgentCore, IAM Roles, Bedrock config
         ▼
ArgoCD (GitOps)
         │  deploy no cluster EKS da conta hot-ai
         ▼
EKS Cluster (conta hot-ai, nodes ARM64)
         │
         ▼
AgentCore Runtime + Amazon Bedrock
         │
         ▼
Observability Stack
       NewRelic / Datadog / Grafana / PagerDuty
```

O fluxo é o mesmo da plataforma padrão. A diferença está nos recursos provisionados (AgentCore em vez de ALB+pods genéricos) e na arquitetura de compute (ARM64).

---

## Acesso à conta hot-ai

O acesso à conta `hot-ai` segue o mesmo processo das demais contas AWS: solicitação via Service Desk, aprovação e acesso via SSO no Okta. Por se tratar de uma conta com workloads sensíveis, o acesso pode exigir aprovação adicional.

Para trabalhar com workloads de IA, consulte o time de AI Platform para entender os padrões específicos e as boas práticas adotadas.

---

## Referências

📄 [`platform-overview/platform-architecture.md`](./platform-architecture.md)
📄 [`platform-overview/aws-accounts.md`](./aws-accounts.md)
📄 [`platform-overview/base-module.md`](./base-module.md)
