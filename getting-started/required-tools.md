# Ferramentas Obrigatórias

## 📑 Índice

- [kubectl](#kubectl)
- [AWS CLI](#aws-cli)
- [hotctl](#hotctl)
- [Docker: via Rancher Desktop](#docker--via-rancher-desktop)
- [Resumo](#resumo)
- [Próximos passos](#próximos-passos)

---

Este documento lista as ferramentas essenciais para trabalhar no ambiente DevOps da Hotmart. Instale todas antes de começar o Training Camp.

---

## kubectl

O `kubectl` é a ferramenta de linha de comando oficial para interagir com clusters Kubernetes. Na Hotmart, praticamente todo workload roda em Kubernetes (EKS na AWS), então o kubectl é uma das ferramentas mais usadas no dia a dia.

Com ele você consegue:
- Verificar o status de pods, deployments e services
- Acessar logs de aplicações
- Executar comandos dentro de containers
- Aplicar e remover manifests YAML
- Fazer troubleshooting de workloads em produção

**Instalação:**
- macOS: `brew install kubectl`
- Documentação oficial: [https://kubernetes.io/docs/tasks/tools/](https://kubernetes.io/docs/tasks/tools/)

**Verificar instalação:**
```bash
kubectl version --client
```

---

## AWS CLI

A AWS CLI é a interface de linha de comando para interagir com os serviços da Amazon Web Services. Na Hotmart, a infraestrutura roda inteiramente na AWS, então essa ferramenta é indispensável.

Com ela você consegue:
- Autenticar e assumir roles em diferentes contas AWS
- Interagir com S3, EC2, EKS, ECR, SSM e outros serviços
- Executar scripts de automação que interagem com a AWS
- Configurar o kubeconfig para acessar clusters EKS

**Instalação:**
- macOS: `brew install awscli`
- Documentação oficial: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

**Verificar instalação:**
```bash
aws --version
```

**Configuração inicial:**
```bash
aws configure
```

> Na Hotmart, a autenticação na AWS é feita via SSO. Consulte o guia de acesso AWS para configurar corretamente o perfil SSO no seu ambiente local.

---

## hotctl

O `hotctl` é a CLI interna da Hotmart para interagir com a plataforma DevOps. É distribuído como um binário e abstrai operações comuns que, sem ele, exigiriam múltiplos comandos de diferentes ferramentas.

Com ele você consegue:
- Autenticar e configurar acesso aos clusters EKS da Hotmart
- Executar operações específicas da plataforma interna
- Interagir com serviços e recursos gerenciados pelo time de Platform Engineering

**Instalação:**
Consulte o repositório oficial para as instruções de download do binário:

📄 [DevOps Docs: GitHub](https://github.com/Hotmart-Org/devops-docs)

**Verificar instalação:**
```bash
hotctl version
```

---

## Docker: via Rancher Desktop

O Docker é a plataforma de containerização mais usada no mercado. Na Hotmart, containers são a base de todos os workloads. Você vai precisar do Docker para buildar imagens localmente, testar aplicações containerizadas e interagir com o ECR (Elastic Container Registry da AWS).

Na Hotmart, o uso do **Rancher Desktop** é recomendado como substituto ao Docker Desktop, por questões de licenciamento. O Rancher Desktop inclui o runtime de containers e o kubectl, sendo uma solução completa para desenvolvimento local.

**O que é o Rancher Desktop:**
Rancher Desktop é uma aplicação open source que fornece um ambiente Kubernetes local e runtime de containers (usando `containerd` ou `dockerd`) no seu computador. É a forma recomendada de rodar containers localmente na Hotmart.

**Instalação:**
- Download: [https://rancherdesktop.io](https://rancherdesktop.io)
- Documentação: [https://docs.rancherdesktop.io](https://docs.rancherdesktop.io)

Após instalar, configure o runtime de containers para `dockerd (moby)` nas preferências do Rancher Desktop para garantir compatibilidade com comandos `docker` padrão.

**Verificar instalação:**
```bash
docker --version
docker run hello-world
```

---

## Resumo

| Ferramenta | Uso principal | Instalação |
|---|---|---|
| kubectl | Interagir com clusters Kubernetes | `brew install kubectl` |
| AWS CLI | Interagir com serviços AWS | `brew install awscli` |
| hotctl | CLI interna da plataforma Hotmart | Repositório interno |
| Rancher Desktop | Runtime de containers (Docker) | [rancherdesktop.io](https://rancherdesktop.io) |

---

## Próximos passos

Após instalar todas as ferramentas, configure o acesso aos clusters EKS seguindo o guia disponível na documentação de plataforma:

📄 [`platform-overview/eks-clusters.md`](../platform-overview/eks-clusters)

Para documentação detalhada de cada ferramenta (comandos, dicas, exemplos), consulte a seção DevOps Tools:

📄 [`devops-tools/kubectl.md`](../devops-tools/kubectl)
📄 [`devops-tools/aws-cli.md`](../devops-tools/aws-cli)
📄 [`devops-tools/hotctl.md`](../devops-tools/hotctl)
📄 [`devops-tools/rancher-desktop.md`](../devops-tools/rancher-desktop)
📄 [`devops-tools/docker.md`](../devops-tools/docker)
