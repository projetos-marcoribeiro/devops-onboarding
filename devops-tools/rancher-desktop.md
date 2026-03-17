# Rancher Desktop

## 📑 Índice

- [O que o Rancher Desktop oferece](#o-que-o-rancher-desktop-oferece)
- [Por que usar o Rancher Desktop](#por-que-usar-o-rancher-desktop)
- [Instalação](#instalação)
- [Configuração recomendada](#configuração-recomendada)
- [Verificando a instalação](#verificando-a-instalação)
- [Uso básico](#uso-básico)
- [Kubernetes local com Rancher Desktop](#kubernetes-local-com-rancher-desktop)

---

O Rancher Desktop é uma aplicação open source que fornece um ambiente local completo para execução de containers e Kubernetes. Na Hotmart, ele é a ferramenta recomendada para rodar Docker localmente, substituindo o Docker Desktop por questões de licenciamento.

🔗 [Site oficial](https://rancherdesktop.io/)
🔗 [Documentação](https://docs.rancherdesktop.io/)

---

## O que o Rancher Desktop oferece

- Runtime de containers (`dockerd` ou `containerd`) para rodar containers localmente
- Kubernetes local (via k3s) para testar workloads antes de subir para o EKS
- Interface gráfica para gerenciar containers e imagens
- CLI `docker` e `kubectl` integrados e configurados automaticamente
- Suporte a múltiplas arquiteturas (amd64 e arm64)

---

## Por que usar o Rancher Desktop

### Substituição ao Docker Desktop

O Docker Desktop passou a exigir licença paga para uso corporativo em empresas acima de um determinado porte. O Rancher Desktop é uma alternativa open source que oferece as mesmas funcionalidades essenciais sem custo de licenciamento.

### Ambiente local consistente com produção

Como a Hotmart roda workloads em Kubernetes (EKS), ter um Kubernetes local facilita testar configurações, Helm charts e manifests antes de fazer deploy nos clusters reais.

### Casos de uso no dia a dia DevOps

- **Buildar imagens Docker** localmente antes de fazer push para o ECR
- **Testar containers** para verificar se a aplicação roda corretamente antes do deploy
- **Desenvolver e testar Helm charts** em um cluster local antes de aplicar em staging
- **Simular workloads Kubernetes** para entender comportamentos sem afetar ambientes reais
- **Executar ferramentas containerizadas** que o time usa no dia a dia

---

## Instalação

1. Acesse [https://rancherdesktop.io](https://rancherdesktop.io)
2. Faça o download da versão para macOS (`.dmg`)
3. Abra o arquivo `.dmg` e arraste o Rancher Desktop para a pasta `Applications`
4. Abra o Rancher Desktop e aguarde a inicialização

Na primeira execução, o Rancher Desktop vai baixar as dependências necessárias (k3s, imagens base). Isso pode levar alguns minutos dependendo da conexão.

---

## Configuração recomendada

### Runtime de containers

Nas preferências do Rancher Desktop, configure o runtime de containers para **dockerd (moby)**. Isso garante compatibilidade com o comando `docker` padrão e com scripts que usam a Docker CLI.

```
Rancher Desktop → Preferences → Container Engine → dockerd (moby)
```

### Recursos alocados

Configure a quantidade de CPU e memória alocada para o Rancher Desktop conforme a capacidade da sua máquina. Para uso típico de DevOps:

- CPU: 4 cores
- Memória: 8 GB

```
Rancher Desktop → Preferences → Virtual Machine → Hardware
```

---

## Verificando a instalação

Após instalar e configurar, verifique se tudo está funcionando:

```bash
# Verificar Docker
docker --version
docker run hello-world

# Verificar kubectl local
kubectl version --client

# Verificar cluster Kubernetes local
kubectl get nodes
# Deve mostrar um node com status Ready
```

---

## Uso básico

```bash
# Buildar uma imagem localmente
docker build -t minha-aplicacao:latest .

# Rodar um container localmente
docker run -p 8080:8080 minha-aplicacao:latest

# Listar containers rodando
docker ps

# Ver logs de um container
docker logs -f <container-id>

# Parar um container
docker stop <container-id>

# Listar imagens locais
docker images
```

---

## Kubernetes local com Rancher Desktop

O Rancher Desktop inclui um cluster Kubernetes local (k3s) que pode ser usado para testar workloads:

```bash
# Verificar que o contexto local está ativo
kubectl config current-context
# rancher-desktop

# Aplicar um manifest localmente
kubectl apply -f deployment.yaml

# Verificar pods no cluster local
kubectl get pods
```

> Lembre de trocar o contexto do kubectl de volta para o cluster EKS da Hotmart após os testes locais:
> ```bash
> hotctl cluster use <cluster-name>
> ```
