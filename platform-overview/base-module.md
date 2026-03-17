# Base Module

## 📑 Índice

- [Por que o base-module existe](#por-que-o-base-module-existe)
- [Como funciona](#como-funciona)
- [Exemplo de YAML declarativo](#exemplo-de-yaml-declarativo)
- [Recursos criados automaticamente](#recursos-criados-automaticamente)
- [Variantes do base-module](#variantes-do-base-module)
- [Como contribuir com o base-module](#como-contribuir-com-o-base-module)
- [Referências](#referências)

---

O `terraform-base-module` é o componente central de provisionamento de infraestrutura da Hotmart. Ele é um módulo Terraform que recebe um arquivo YAML declarativo e cria automaticamente todos os recursos AWS necessários para uma aplicação rodar na plataforma.

🔗 [https://github.com/Hotmart-Org/terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module)

---

## Por que o base-module existe

Sem o base-module, cada time de produto precisaria escrever e manter seu próprio código Terraform para criar ECR, IAM Roles, ALB, Security Groups, Secrets e configurações de monitoramento. Isso geraria:

- Duplicação massiva de código de infraestrutura
- Inconsistências de configuração entre times
- Dificuldade de aplicar mudanças de segurança ou padrões em toda a organização
- Carga operacional desnecessária para times de produto

O base-module resolve isso com uma abstração declarativa: o time de produto descreve **o que** precisa, e o base-module cuida de **como** provisionar.

---

## Como funciona

```
Time de produto cria/atualiza
um arquivo YAML declarativo
         │
         ▼
Pipeline de CI/CD executa o Terraform
com o base-module
         │
         ▼
Base-module interpreta o YAML e
provisiona os recursos necessários na AWS:
         │
         ├── ECR (repositório de imagem Docker)
         ├── IAM Role (permissões da aplicação)
         ├── KMS Key (criptografia de secrets)
         ├── Secrets Manager (secrets da aplicação)
         ├── ALB (load balancer)
         ├── Security Groups (regras de rede)
         ├── Route53 (registro DNS)
         ├── ACM Certificate (TLS)
         └── Configurações de monitoramento
```

---

## Exemplo de YAML declarativo

```yaml
app:
  name: minha-aplicacao
  team: payments
  environment: production

container:
  port: 8080
  replicas: 3
  resources:
    requests:
      cpu: "250m"
      memory: "512Mi"
    limits:
      cpu: "500m"
      memory: "1Gi"

ingress:
  enabled: true
  host: minha-aplicacao.hotmart.com
  tls: true

secrets:
  - name: DATABASE_URL
  - name: API_KEY

monitoring:
  enabled: true
  alerts: true

iam:
  policies:
    - s3:GetObject
    - s3:PutObject
  resources:
    - arn:aws:s3:::meu-bucket/*
```

Com esse YAML, o base-module cria automaticamente toda a infraestrutura necessária para a aplicação, sem que o time precise escrever uma linha de Terraform.

---

## Recursos criados automaticamente

| Recurso | Descrição |
|---|---|
| ECR | Repositório Docker para armazenar as imagens da aplicação |
| IAM Role | Role com as permissões declaradas, associada via EKS Pod Identity |
| KMS Key | Chave de criptografia para secrets e dados sensíveis |
| Secrets Manager | Armazenamento seguro de secrets da aplicação |
| ALB | Load balancer para expor a aplicação externamente |
| Security Groups | Regras de rede para o ALB e os pods |
| Route53 Record | Registro DNS apontando para o ALB |
| ACM Certificate | Certificado TLS para o domínio da aplicação |
| Monitoring Config | Integração com NewRelic, Datadog e alertas no PagerDuty |

---

## Variantes do base-module

O base-module tem variantes especializadas para diferentes tipos de workload:

### base-module

A variante padrão para aplicações web e APIs rodando em containers no EKS. É a mais utilizada na plataforma.

### base-module-lambda

Variante para funções AWS Lambda. Provisiona a função, IAM Role, triggers (API Gateway, SQS, EventBridge, etc.) e configurações de monitoramento específicas para serverless.

### base-module-agentcore

Variante especializada para agentes de IA que rodam no Amazon Bedrock AgentCore. Provisiona os recursos necessários para agentes de IA, incluindo integrações com Bedrock e configurações específicas para workloads de IA.

### base-module-nomad

Variante para workloads que rodam no HashiCorp Nomad, utilizada em casos específicos onde o Kubernetes não é a plataforma de execução.

---

## Como contribuir com o base-module

O base-module é mantido pelo time de Platform Engineering. Para propor mudanças:

1. Abra uma issue no repositório descrevendo a necessidade
2. Discuta a abordagem com o time antes de implementar
3. Abra um PR com a implementação e testes
4. Aguarde revisão do time de Platform Engineering

Mudanças no base-module afetam todas as aplicações que o utilizam, então o processo de revisão é criterioso.

---

## Referências

📄 [`platform-overview/platform-architecture.md`](./platform-architecture.md)
📄 [`platform-overview/ci-cd-pipelines.md`](./ci-cd-pipelines.md)
📄 [`platform-overview/argocd-gitops.md`](./argocd-gitops.md)
📄 [`training-camp/learning-path.md`](../training-camp/learning-path.md)
