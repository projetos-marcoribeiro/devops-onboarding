# Contas AWS

## 📑 Índice

- [Por que múltiplas contas](#por-que-múltiplas-contas)
- [Contas da organização](#contas-da-organização)
- [Como as contas se relacionam](#como-as-contas-se-relacionam)
- [Acessando as contas](#acessando-as-contas)
- [Boas práticas ao trabalhar com múltiplas contas](#boas-práticas-ao-trabalhar-com-múltiplas-contas)
- [Referências](#referências)

---

A Hotmart utiliza uma estratégia multi-account na AWS, onde cada domínio de negócio ou função técnica possui sua própria conta AWS isolada. Essa abordagem é uma das melhores práticas recomendadas pelo AWS Well-Architected Framework e é fundamental para operar com segurança e governança em escala.

---

## Por que múltiplas contas

Usar uma única conta AWS para tudo seria mais simples no início, mas cria problemas sérios conforme a empresa cresce:

**Isolamento de recursos:** um problema em uma conta (como um recurso mal configurado ou um ataque) não afeta outras contas. O blast radius de qualquer incidente fica contido.

**Segurança:** políticas de IAM, SCPs (Service Control Policies) e guardrails de segurança podem ser aplicados por conta, garantindo que cada domínio tenha apenas as permissões que precisa.

**Governança:** é possível aplicar políticas diferentes para contas de produção, staging e desenvolvimento, sem que uma configuração afete a outra.

**Controle de custos:** cada conta tem seu próprio billing, o que permite visibilidade clara de quanto cada domínio ou produto está gastando na AWS. Isso é essencial para FinOps e otimização de custos.

---

## Contas da organização

| Conta | Domínio | Descrição |
|---|---|---|
| `hot-ai` | Inteligência Artificial | Workloads de IA, Amazon Bedrock, AgentCore e Dynamic Agents |
| `buildstaging` | CI/CD | Runners de GitHub Actions, pipelines de build e staging |
| `analytics` | Dados | Data platform, pipelines de dados e analytics |
| `auth` | Autenticação | Serviços de autenticação e autorização |
| `hotpay` | Pagamentos | Infraestrutura de processamento de pagamentos |
| `send` | Notificações | Serviços de envio de e-mail, SMS e push notifications |
| `sites` | Frontend | Hospedagem de sites e aplicações frontend |
| `secops` | Segurança | Ferramentas de segurança, SIEM, auditoria e compliance |
| `club` | Assinaturas | Infraestrutura do produto de assinaturas |
| `devops` | Infraestrutura | Infraestrutura compartilhada, ferramentas DevOps e plataforma |

> Esta lista representa exemplos das contas existentes. A organização completa pode ter mais contas conforme novos domínios são criados.

---

## Como as contas se relacionam

```
AWS Organizations (conta master)
         │
         ├── devops          (infraestrutura compartilhada)
         ├── buildstaging    (CI/CD e runners)
         ├── secops          (segurança centralizada)
         │
         ├── hot-ai          (IA)
         ├── analytics       (dados)
         ├── auth            (autenticação)
         ├── hotpay          (pagamentos)
         ├── send            (notificações)
         ├── sites           (frontend)
         └── club            (assinaturas)
```

A conta `devops` funciona como hub de infraestrutura compartilhada — ferramentas como ArgoCD, runners de CI/CD e recursos de rede que são usados por múltiplos domínios podem residir aqui.

A conta `secops` centraliza ferramentas de segurança e tem visibilidade sobre todas as outras contas para fins de auditoria e compliance.

---

## Acessando as contas

O acesso às contas AWS é feito via SSO através do Okta. Após ter o acesso aprovado no Service Desk, a conta aparecerá no painel do Okta e você poderá acessar o console AWS ou configurar o perfil SSO na AWS CLI.

**Configurando acesso via CLI:**

```bash
# Configurar SSO
aws configure sso

# Listar perfis disponíveis
aws configure list-profiles

# Usar um perfil específico
aws s3 ls --profile hotmart-devops
```

**Assumindo roles entre contas:**

Em alguns casos, você precisará assumir uma role em outra conta a partir da conta principal. Isso é feito via `sts:AssumeRole`:

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::ACCOUNT_ID:role/DevOpsRole \
  --role-session-name my-session \
  --profile hotmart-devops
```

---

## Boas práticas ao trabalhar com múltiplas contas

- Sempre verifique em qual conta você está antes de executar comandos que modificam recursos
- Use o perfil correto na AWS CLI para cada operação
- Nunca use credenciais de longa duração (access key + secret key) — sempre use SSO ou roles temporárias
- Ao criar recursos via Terraform, confirme que o provider está apontando para a conta correta
- Em caso de dúvida sobre qual conta usar para uma tarefa, consulte o tech lead ou a documentação do domínio

---

## Referências

📄 [`platform-overview/platform-architecture.md`](#/platform-overview/platform-architecture)
📄 [`platform-overview/eks-clusters.md`](#/platform-overview/eks-clusters)
📄 [`getting-started/okta-access.md`](#/getting-started/okta-access)
