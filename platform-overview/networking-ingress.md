# Networking e Ingress

> ⚠️ **Nota:** O Cloudflare está sendo removido da plataforma. Algumas contas ainda possuem configurações legadas em processo de migração. As informações sobre Cloudflare neste documento são mantidas como referência durante a transição.

## 📑 Índice

- [Fluxo de tráfego](#fluxo-de-tráfego)
- [Cloudflare](#cloudflare)
- [AWS ALB: Application Load Balancer](#aws-alb--application-load-balancer)
- [Certificados TLS: AWS ACM](#certificados-tls--aws-acm)
- [Route53 e External DNS](#route53-e-external-dns)
- [Security Groups](#security-groups)
- [Ingress no Kubernetes](#ingress-no-kubernetes)
- [Networking interno (Service Mesh)](#networking-interno-service-mesh)
- [Referências](#referências)

---

Este documento explica como o tráfego externo chega até as aplicações rodando nos clusters EKS da Hotmart, e como a infraestrutura de rede é gerenciada.

---

## Fluxo de tráfego

```
Usuário (browser / cliente)
         │
         │  requisição HTTPS
         ▼
Cloudflare
         │  proteção DDoS, WAF, CDN, TLS termination
         │
         ▼
AWS ALB (Application Load Balancer)
         │  roteamento por host/path, health checks
         │  certificado TLS via ACM
         ▼
Ingress Controller (dentro do cluster EKS)
         │  regras de roteamento Kubernetes
         │  baseado em host e path do Ingress resource
         ▼
Service (ClusterIP)
         │
         ▼
Pods da aplicação
```

---

## Cloudflare

O Cloudflare é a primeira camada de proteção e distribuição de tráfego. Todo tráfego externo passa pelo Cloudflare antes de chegar à infraestrutura AWS.

**Funções do Cloudflare na Hotmart:**
- Proteção contra DDoS
- Web Application Firewall (WAF)
- CDN para assets estáticos
- Terminação TLS para domínios públicos
- Rate limiting e regras de segurança customizadas

O Cloudflare aponta para os ALBs da AWS via registros DNS. A integração é gerenciada pelo time DevOps e as regras de roteamento são configuradas conforme necessário para cada aplicação.

---

## AWS ALB: Application Load Balancer

O ALB é o load balancer da AWS que recebe o tráfego vindo do Cloudflare e distribui para os pods dentro do cluster EKS.

Na Hotmart, os ALBs são criados automaticamente pelo **AWS Load Balancer Controller** dentro do cluster, que monitora recursos `Ingress` do Kubernetes e provisiona o ALB correspondente na AWS.

**Características do ALB na plataforma:**
- Roteamento baseado em host (hostname) e path
- Health checks automáticos nos pods
- Suporte a múltiplos target groups por regra
- Integração nativa com ACM para certificados TLS
- Security Groups gerados automaticamente pelo base-module

---

## Certificados TLS: AWS ACM

Os certificados TLS utilizados nos ALBs são gerenciados pelo AWS Certificate Manager (ACM). O base-module provisiona automaticamente os certificados necessários para cada aplicação com base no domínio declarado no YAML de configuração.

O processo de validação dos certificados é feito via DNS (Route53), sem necessidade de intervenção manual.

---

## Route53 e External DNS

O Route53 é o serviço de DNS da AWS utilizado para gerenciar os registros DNS internos e externos da Hotmart.

O **External DNS** é um componente que roda dentro do cluster EKS e monitora recursos `Ingress` e `Service` do Kubernetes. Quando um novo Ingress é criado com uma anotação de hostname, o External DNS cria automaticamente o registro DNS correspondente no Route53.

```
Novo Ingress criado no cluster
         │
         ▼
External DNS detecta o hostname configurado
         │
         ▼
External DNS cria/atualiza registro no Route53
         │
         ▼
DNS resolve para o ALB correto
```

Isso significa que, ao fazer deploy de uma nova aplicação via base-module, o DNS é configurado automaticamente: sem necessidade de criar registros manualmente.

---

## Security Groups

Os Security Groups que controlam o tráfego de rede para os ALBs e nodes do cluster são gerados automaticamente pelo base-module com base nas configurações declaradas no YAML da aplicação.

As regras padrão permitem:
- Tráfego HTTPS (443) vindo do Cloudflare para o ALB
- Tráfego do ALB para os nodes do cluster nas portas dos pods
- Comunicação interna entre pods dentro do cluster

Regras adicionais podem ser declaradas no YAML do base-module quando a aplicação precisa de acesso a recursos externos específicos.

---

## Ingress no Kubernetes

O recurso `Ingress` do Kubernetes define as regras de roteamento dentro do cluster. Um exemplo típico:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minha-aplicacao
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:...
spec:
  rules:
    - host: minha-aplicacao.hotmart.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: minha-aplicacao
                port:
                  number: 80
```

Na prática, você não escreve esse YAML manualmente. O base-module gera os recursos de Ingress automaticamente com base na configuração declarativa da aplicação.

---

## Networking interno (Service Mesh)

Para comunicação entre serviços dentro do cluster, os pods se comunicam via Services do Kubernetes usando DNS interno (`service-name.namespace.svc.cluster.local`). Não é necessário passar pelo ALB para comunicação interna.

Para casos que exigem controle mais granular de tráfego interno (circuit breaking, retry, observabilidade de tráfego east-west), consulte o tech lead sobre as opções disponíveis na plataforma.

---

## Referências

📄 [`platform-overview/platform-architecture.md`](platform-architecture)
📄 [`platform-overview/eks-clusters.md`](eks-clusters)
📄 [`platform-overview/aws-accounts.md`](aws-accounts)
