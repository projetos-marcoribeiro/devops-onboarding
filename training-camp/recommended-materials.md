# Materiais Recomendados

## 📑 Índice

- [Infraestrutura](#infraestrutura)
- [Observabilidade](#observabilidade)
- [Segurança](#segurança)
- [Ferramentas](#ferramentas)
- [Banco de Dados](#banco-de-dados)
- [Troubleshooting](#troubleshooting)
- [Comunicação](#comunicação)
- [Como acessar os materiais](#como-acessar-os-materiais)

---

Estes materiais foram produzidos internamente pelo time DevOps da Hotmart e servem como referência para entender como a infraestrutura da empresa funciona. São apresentações, guias e documentações criadas por engenheiros do próprio time, com foco nos padrões e ferramentas reais utilizados na plataforma.

Consuma os materiais na ordem sugerida pelo [Learning Path](learning-path), não aleatoriamente. O contexto acumulado em cada etapa torna os materiais seguintes mais fáceis de absorver.

---

## Infraestrutura

Materiais fundamentais para entender como a infraestrutura da Hotmart é provisionada e gerenciada.

### Base Module
Apresentação introdutória sobre o terraform-base-module: o que é, como funciona, quais recursos cria e como os times de produto o utilizam para provisionar infraestrutura de forma declarativa.

**Quando estudar:** início do Training Camp, antes de qualquer outra coisa.

### IAC + Base Module
Aprofundamento em Infrastructure as Code com foco na integração entre Terraform e o Base Module. Cobre boas práticas de IaC, estrutura de módulos e como o Base Module evoluiu para atender os padrões da plataforma.

**Quando estudar:** logo após o material de Base Module.

### Hotmart Charts
Documentação sobre os Helm Charts utilizados na plataforma. Explica a estrutura dos charts, como os values são organizados por ambiente e como o ArgoCD os utiliza para fazer deploy das aplicações.

**Quando estudar:** após entender o Base Module e o fluxo de deploy com ArgoCD.

---

## Observabilidade

Materiais sobre como as aplicações e a infraestrutura são monitoradas na Hotmart.

### Monitoring Applications
Guia completo sobre a stack de observabilidade: NewRelic, Datadog, Prometheus, Grafana e PagerDuty. Explica como cada ferramenta é usada, como o monitoramento é configurado automaticamente pelo Base Module e como navegar nas ferramentas durante um incidente.

**Quando estudar:** etapa 2 do Learning Path.

### KEDA
Apresentação sobre o Kubernetes Event-Driven Autoscaling (KEDA). Explica como o KEDA é utilizado na plataforma para escalar workloads com base em eventos externos (filas SQS, métricas customizadas, etc.), complementando o autoscaling padrão do Kubernetes.

**Quando estudar:** após o material de Monitoring Applications.

---

## Segurança

Materiais sobre práticas e ferramentas de segurança utilizadas na plataforma.

### Vault
Guia sobre o HashiCorp Vault: como ele é utilizado para gerenciar secrets na Hotmart, como as aplicações acessam credenciais em runtime e como o ciclo de vida de um secret é gerenciado (criação, rotação, revogação).

**Quando estudar:** etapa 3 do Learning Path.

### SIEM
Apresentação sobre o sistema de gerenciamento de eventos de segurança (SIEM) utilizado na Hotmart. Explica como logs de segurança são coletados, correlacionados e analisados para detecção de ameaças e auditoria.

**Quando estudar:** após o material de Vault.

### Cloudflare
Documentação sobre o uso do Cloudflare na plataforma: proteção DDoS, WAF, regras de segurança e como o Cloudflare se integra com a infraestrutura AWS da Hotmart.

**Quando estudar:** junto com os materiais de segurança ou após o estudo de networking.

---

## Ferramentas

Materiais sobre ferramentas internas e de developer experience.

### Criando uma aplicação via Backstage
Guia passo a passo para criar uma nova aplicação utilizando o portal Backstage da Hotmart. Explica como o Backstage automatiza o scaffolding de repositórios, configurações de CI/CD e provisionamento inicial de infraestrutura via Base Module.

**Quando estudar:** após entender o Base Module e o fluxo de deploy.

### DAS
Apresentação sobre o DAS (ferramenta interna de automação). Explica como o DAS é utilizado pelo time DevOps para automatizar tarefas operacionais recorrentes na plataforma.

**Quando estudar:** após os materiais de infraestrutura e CI/CD.

---

## Banco de Dados

### Criando um banco de dados do zero
Guia completo para provisionar e configurar um banco de dados na plataforma Hotmart. Cobre a escolha do tipo de banco, provisionamento via Base Module, configuração de acesso seguro via Vault e boas práticas de operação.

**Quando estudar:** quando precisar trabalhar com aplicações que utilizam banco de dados, ou como parte do Hands-on Project.

---

## Troubleshooting

### Troubleshooting Guide
Guia de investigação de problemas em ambientes distribuídos. Apresenta uma metodologia estruturada para ir do sintoma à causa raiz, com exemplos de cenários comuns na plataforma Hotmart e os comandos e ferramentas utilizados em cada etapa da investigação.

**Quando estudar:** etapa 4 do Learning Path. Também é uma referência útil durante incidentes reais.

---

## Comunicação

### WhatsApp Integrations
Documentação sobre as integrações de comunicação via WhatsApp utilizadas na plataforma. Explica como notificações, alertas e automações são enviados via WhatsApp e como configurar novas integrações.

**Quando estudar:** após os materiais principais, como tópico complementar.

---

## Como acessar os materiais

Os materiais internos estão disponíveis no portal de documentação TechDeck e nos repositórios da organização no GitHub. Caso não encontre algum material ou o link esteja desatualizado, consulte seu mentor ou pergunte no canal do time no Google Chat.

Se você identificar que algum material está desatualizado ou incompleto, abra um PR com a correção ou reporte ao time. Manter a documentação atualizada é responsabilidade coletiva.
