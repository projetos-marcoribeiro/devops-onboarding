# Docker

## 📑 Índice

- [Por que containers](#por-que-containers)
- [Fluxo de uma aplicação containerizada](#fluxo-de-uma-aplicação-containerizada)
- [Dockerfile](#dockerfile)
- [Comandos essenciais](#comandos-essenciais)
- [Boas práticas de Dockerfile](#boas-práticas-de-dockerfile)
- [Docker no contexto da Hotmart](#docker-no-contexto-da-hotmart)

---

O Docker é a plataforma de containerização utilizada na Hotmart para empacotar aplicações e suas dependências em unidades portáteis e isoladas chamadas containers. Todo workload que roda nos clusters EKS da Hotmart começa como uma imagem Docker.

🔗 [Documentação oficial do Docker](https://docs.docker.com/)

---

## Por que containers

Antes dos containers, o problema clássico era: "funciona na minha máquina". Containers resolvem isso empacotando a aplicação junto com todas as suas dependências (runtime, bibliotecas, configurações) em uma imagem que roda de forma idêntica em qualquer ambiente — desenvolvimento, staging ou produção.

Na Hotmart, isso significa que a mesma imagem que você testa localmente é a que vai para o EKS em produção. O que você vê é o que roda.

---

## Fluxo de uma aplicação containerizada

```
Código da aplicação
         │
         ▼
Dockerfile
         │  define como a imagem é construída
         ▼
docker build
         │  cria a imagem localmente
         ▼
docker push → ECR (Elastic Container Registry)
         │  imagem armazenada no registry da AWS
         ▼
GitHub Actions (CI/CD)
         │  automatiza o build e push a cada commit
         ▼
ArgoCD → EKS
         imagem deployada no cluster Kubernetes
```

---

## Dockerfile

O Dockerfile é o arquivo que define como a imagem Docker é construída. Cada instrução cria uma camada na imagem.

### Exemplo básico — Node.js

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package.json .
RUN npm install

COPY . .

CMD ["node", "index.js"]
```

### Exemplo com multi-stage build (recomendado)

O multi-stage build é a prática recomendada para produção. Ele separa o ambiente de build do ambiente de execução, resultando em imagens menores e mais seguras:

```dockerfile
# Stage 1: build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Stage 2: runtime
FROM node:18-alpine
WORKDIR /app

# Não rode como root em produção
USER node

COPY --from=builder /app/node_modules ./node_modules
COPY . .

EXPOSE 8080
CMD ["node", "index.js"]
```

**Por que multi-stage:**
- A imagem final não contém ferramentas de build (npm, compiladores, etc.)
- Menor superfície de ataque para vulnerabilidades
- Imagem significativamente menor em tamanho

---

## Comandos essenciais

### Build de imagem

```bash
# Build básico
docker build -t minha-aplicacao:latest .

# Build com tag específica
docker build -t minha-aplicacao:1.0.0 .

# Build para múltiplas arquiteturas (amd64 e arm64)
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t minha-aplicacao:latest \
  --push .
```

### Executar containers localmente

```bash
# Rodar um container
docker run minha-aplicacao:latest

# Rodar com porta mapeada
docker run -p 8080:8080 minha-aplicacao:latest

# Rodar em background
docker run -d -p 8080:8080 minha-aplicacao:latest

# Rodar com variáveis de ambiente
docker run -e DATABASE_URL=postgres://... minha-aplicacao:latest

# Rodar com shell interativo
docker run -it minha-aplicacao:latest /bin/sh
```

### Gerenciar containers e imagens

```bash
# Listar containers rodando
docker ps

# Listar todos os containers (incluindo parados)
docker ps -a

# Ver logs de um container
docker logs -f <container-id>

# Parar um container
docker stop <container-id>

# Remover um container
docker rm <container-id>

# Listar imagens locais
docker images

# Remover uma imagem
docker rmi minha-aplicacao:latest

# Limpar recursos não utilizados
docker system prune
```

### Push para o ECR

No fluxo da Hotmart, o push para o ECR é feito automaticamente pelo pipeline de CI/CD. Mas é útil saber como fazer manualmente:

```bash
# Autenticar no ECR
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin \
  <account-id>.dkr.ecr.us-east-1.amazonaws.com

# Taguear a imagem com o endereço do ECR
docker tag minha-aplicacao:latest \
  <account-id>.dkr.ecr.us-east-1.amazonaws.com/minha-aplicacao:latest

# Push para o ECR
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/minha-aplicacao:latest
```

---

## Boas práticas de Dockerfile

**Use imagens base mínimas:** prefira `alpine` ou `distroless` para reduzir o tamanho e a superfície de ataque da imagem.

**Não rode como root:** adicione `USER` no Dockerfile para rodar o processo com um usuário sem privilégios.

**Use `.dockerignore`:** assim como o `.gitignore`, o `.dockerignore` evita que arquivos desnecessários (node_modules, .git, arquivos de teste) sejam copiados para a imagem.

```
# .dockerignore
node_modules
.git
*.test.js
.env
README.md
```

**Ordene as instruções do Dockerfile** do menos para o mais frequentemente alterado. O Docker usa cache de camadas — se uma camada não mudou, ela é reutilizada. Copiar o `package.json` antes do código fonte garante que o `npm install` só roda quando as dependências mudam.

**Não coloque secrets no Dockerfile.** Nunca use `ENV` ou `ARG` para passar credenciais — elas ficam visíveis no histórico da imagem. Use Secrets Manager ou variáveis de ambiente injetadas em runtime.

---

## Docker no contexto da Hotmart

Na Hotmart, você raramente vai fazer push manual de imagens para o ECR. O pipeline de CI/CD (GitHub Actions) cuida disso automaticamente a cada commit na branch principal.

O Docker no dia a dia do DevOps é mais usado para:
- Testar imagens localmente antes de fazer push
- Debugar problemas de build de imagem
- Rodar ferramentas containerizadas localmente
- Entender o que está dentro de uma imagem que está causando problema em produção

Para rodar Docker localmente, use o Rancher Desktop conforme descrito em [`devops-tools/rancher-desktop.md`](./rancher-desktop.md).
