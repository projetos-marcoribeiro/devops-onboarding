# AWS CLI

## 📑 Índice

- [Instalação](#instalação)
- [Autenticação via Okta (SSO)](#autenticação-via-okta-sso)
- [Comandos essenciais](#comandos-essenciais)
- [Dicas de produtividade](#dicas-de-produtividade)

---

O AWS CLI é a interface de linha de comando oficial da Amazon Web Services. Ele permite interagir com praticamente qualquer serviço AWS diretamente pelo terminal, sem precisar acessar o console web. Para o time DevOps da Hotmart, é uma ferramenta indispensável no dia a dia.

🔗 [Documentação oficial do AWS CLI](https://docs.aws.amazon.com/cli/)

---

## Instalação

```bash
# macOS via Homebrew
brew install awscli

# Verificar instalação
aws --version
```

---

## Autenticação via Okta (SSO)

Na Hotmart, o acesso à AWS é feito via SSO através do Okta. Isso significa que você não usa access key + secret key estáticos: você autentica via SSO e recebe credenciais temporárias.

**Configuração inicial do SSO:**

```bash
aws configure sso
```

O comando vai pedir:
- SSO start URL: URL do portal SSO da Hotmart
- SSO Region: região AWS do SSO (geralmente `us-east-1`)
- Conta e role que deseja usar

Após configurar, você terá perfis nomeados para cada conta AWS. Para autenticar:

```bash
# Fazer login via SSO (abre o browser para autenticação)
aws sso login --profile <profile-name>

# Listar perfis configurados
aws configure list-profiles
```

**Usando um perfil específico:**

```bash
# Definir o perfil para o comando atual
aws s3 ls --profile hotmart-devops

# Definir o perfil padrão para a sessão
export AWS_PROFILE=hotmart-devops
```

---

## Comandos essenciais

### Verificar identidade e contexto

Sempre verifique em qual conta e com qual role você está antes de executar comandos que modificam recursos:

```bash
# Verificar identidade atual (conta, user/role, ARN)
aws sts get-caller-identity

# Saída esperada:
# {
#     "UserId": "AROAXXXXXXXXXXXXXXXXX:session-name",
#     "Account": "123456789012",
#     "Arn": "arn:aws:sts::123456789012:assumed-role/DevOpsRole/session-name"
# }
```

### S3

```bash
# Listar todos os buckets
aws s3 ls

# Listar conteúdo de um bucket
aws s3 ls s3://nome-do-bucket/

# Copiar arquivo para S3
aws s3 cp arquivo.txt s3://nome-do-bucket/

# Sincronizar diretório local com S3
aws s3 sync ./dist s3://nome-do-bucket/frontend/

# Remover arquivo do S3
aws s3 rm s3://nome-do-bucket/arquivo.txt
```

### EC2

```bash
# Listar instâncias EC2
aws ec2 describe-instances

# Listar instâncias com filtro por estado
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query "Reservations[*].Instances[*].[InstanceId,InstanceType,State.Name]" \
  --output table
```

### EKS

```bash
# Listar clusters EKS
aws eks list-clusters

# Detalhes de um cluster
aws eks describe-cluster --name <cluster-name>

# Atualizar kubeconfig para acessar um cluster
aws eks update-kubeconfig \
  --name <cluster-name> \
  --region us-east-1 \
  --profile hotmart-devops
```

### IAM

```bash
# Listar roles IAM
aws iam list-roles --query "Roles[*].[RoleName,Arn]" --output table

# Ver políticas anexadas a uma role
aws iam list-attached-role-policies --role-name <role-name>

# Detalhes de uma política
aws iam get-policy --policy-arn <policy-arn>
```

### Secrets Manager

```bash
# Listar secrets
aws secretsmanager list-secrets

# Obter valor de um secret
aws secretsmanager get-secret-value --secret-id <secret-name>

# Criar um secret
aws secretsmanager create-secret \
  --name meu-secret \
  --secret-string '{"username":"admin","password":"senha123"}'
```

### CloudWatch Logs

```bash
# Listar log groups
aws logs describe-log-groups

# Ver logs recentes de um log group
aws logs tail /aws/eks/<cluster-name>/cluster --follow

# Filtrar logs por padrão
aws logs filter-log-events \
  --log-group-name /aws/lambda/minha-funcao \
  --filter-pattern "ERROR"
```

### SQS

```bash
# Listar filas
aws sqs list-queues

# Ver atributos de uma fila (inclui número de mensagens)
aws sqs get-queue-attributes \
  --queue-url <queue-url> \
  --attribute-names All
```

---

## Dicas de produtividade

**Use `--output` para formatar a saída:**
```bash
# Saída em tabela (legível)
aws ec2 describe-instances --output table

# Saída em JSON (padrão, bom para scripts)
aws ec2 describe-instances --output json

# Saída em texto simples (bom para pipes)
aws ec2 describe-instances --output text
```

**Use `--query` para filtrar a saída com JMESPath:**
```bash
# Listar apenas os nomes dos buckets S3
aws s3api list-buckets --query "Buckets[*].Name" --output text
```

**Nunca use credenciais estáticas (access key + secret key) em scripts ou código.** Use sempre SSO, roles IAM ou EKS Pod Identity para autenticação. Credenciais estáticas são um risco de segurança sério.
