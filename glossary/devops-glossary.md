# Glossário DevOps: Hotmart

## 📑 Índice

- [Infraestrutura](#infraestrutura)
- [Ferramentas e Plataformas](#ferramentas-e-plataformas)
- [Workflows e Processos](#workflows-e-processos)
- [Monitoramento](#monitoramento)
- [Bancos de Dados e Troubleshooting](#bancos-de-dados-e-troubleshooting)

---

Este glossário serve como referência rápida para novos engenheiros DevOps entenderem os termos, siglas e ferramentas mais comuns utilizados na plataforma Hotmart. Os termos estão organizados por categoria para facilitar a consulta.

Se você encontrar um termo que não está aqui, considere adicioná-lo: manter o glossário atualizado é uma contribuição valiosa para quem vem depois.

---

## Infraestrutura

### PU (Product Unit)

Unidade de produto responsável por uma área específica da plataforma Hotmart. Cada PU tem ownership sobre seus serviços, pipelines, repositórios e infraestrutura. Times de engenharia são organizados em PUs para garantir autonomia e clareza de responsabilidade.

**Contexto na Hotmart:** o time DevOps atua de forma transversal, suportando todas as PUs. Exemplos de PUs: Content Flow, Teachable, Financial, Platform Engineering.

---

### EKS (Elastic Kubernetes Service)

Serviço gerenciado de Kubernetes da AWS. O EKS cuida do control plane (API server, etcd, scheduler), deixando para o time DevOps a responsabilidade de gerenciar os nodes e os workloads.

**Contexto na Hotmart:** praticamente todos os workloads de aplicação rodam em clusters EKS. A Hotmart opera múltiplos clusters organizados por domínio, ambiente e criticidade.

---

### IaC (Infrastructure as Code)

Prática de gerenciar e provisionar infraestrutura através de código, em vez de processos manuais. A infraestrutura é descrita em arquivos de configuração versionados no Git, o que garante reprodutibilidade, rastreabilidade e facilidade de revisão.

**Contexto na Hotmart:** toda infraestrutura é provisionada via Terraform (principalmente através do Base Module). Não existe provisionamento manual de recursos no console AWS.

---

### Base Module

Módulo Terraform padrão utilizado para provisionar a infraestrutura necessária para uma aplicação na Hotmart. Ele recebe um arquivo YAML declarativo e cria automaticamente os recursos AWS necessários: ECR, IAM Roles, ALB, Security Groups, KMS, Secrets Manager e configurações de monitoramento.

**Contexto na Hotmart:** é o componente central de provisionamento da plataforma. Times de produto não escrevem Terraform diretamente: apenas declaram o que precisam no YAML do Base Module. Variantes: `base-module`, `base-module-lambda`, `base-module-agentcore`, `base-module-nomad`.

---

### Hotmart Charts

Templates Helm customizados utilizados para o deploy de aplicações nos clusters EKS da Hotmart. Os charts definem como as aplicações são configuradas e deployadas no Kubernetes, com padrões e boas práticas da plataforma já incorporados.

**Contexto na Hotmart:** os charts ficam no repositório [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac) e são gerenciados pelo time DevOps. Os values específicos de cada aplicação e ambiente são configurados pelos times de produto. O ArgoCD está sendo implementado como o novo modelo de deploy, substituindo o fluxo anterior baseado em Helm upgrade/install/rollback.

---

### KEDA (Kubernetes Event-Driven Autoscaling)

Componente open source que permite escalar workloads Kubernetes com base em eventos externos, como profundidade de filas SQS, métricas customizadas ou outros triggers além do uso de CPU e memória.

**Contexto na Hotmart:** utilizado para escalar automaticamente aplicações que processam filas ou respondem a eventos, garantindo que o número de pods acompanhe a demanda real sem superprovisionamento.

---

### hotctl

CLI interna desenvolvida pelo time DevOps da Hotmart para interagir com a infraestrutura da plataforma. Abstrai operações comuns que, sem ela, exigiriam múltiplos comandos de diferentes ferramentas.

**Contexto na Hotmart:** principal ferramenta para configurar acesso aos clusters EKS. Instalação obrigatória durante o onboarding. Repositório: [github.com/Hotmart-Org/hotctl](https://github.com/Hotmart-Org/hotctl).

---

### IRSA (IAM Roles for Service Accounts)

Mecanismo de autenticação que permite que pods Kubernetes assumam IAM Roles na AWS sem precisar de credenciais estáticas. Funciona associando uma ServiceAccount do Kubernetes a uma IAM Role via anotação e trust policy.

**Contexto na Hotmart:** mecanismo atual utilizado para autenticação IAM de pods, implementado via EKS Pod Identity. O Base Module cria automaticamente as roles e associações necessárias para cada aplicação.

---

### KIAM ⚠️ Descontinuado

Mecanismo anterior de autenticação IAM para pods Kubernetes, que funcionava interceptando chamadas de metadados EC2 para assumir roles.

**Contexto na Hotmart:** descontinuado. Substituído por **EKS Pod Identity**, que é o mecanismo atual para dar acesso a recursos AWS a pods no Kubernetes. Se você encontrar referências ao KIAM em código ou configuração, trata-se de configuração legada. Novos serviços devem usar exclusivamente EKS Pod Identity. Repositórios legados como `analytics-iac`, `auth-iac` e `buildstaging-iac` ainda contêm arquivos `kiam-roles.tf` que estão em processo de migração.

---

## Ferramentas e Plataformas

### Backstage

Portal de desenvolvedores open source (criado pelo Spotify) utilizado para catalogar serviços, projetos e recursos de engenharia. Centraliza a descoberta de serviços, documentação e ferramentas de developer experience.

**Contexto na Hotmart:** utilizado para criar novas aplicações via scaffolding automatizado, catalogar serviços existentes e facilitar a descoberta de recursos de infraestrutura. Reduz a fricção para times de produto ao iniciar novos projetos.

---

### Vault

Sistema de gerenciamento de secrets desenvolvido pela HashiCorp. Armazena, controla o acesso e audita o uso de credenciais, tokens, certificados e outros dados sensíveis.

**Contexto na Hotmart:** utilizado para gerenciar secrets de aplicações. Integrado com o Base Module para injeção automática de secrets nos pods via Secrets Manager. Acessível via Okta.

---

### Okta

Plataforma de gerenciamento de identidades e Single Sign-On (SSO) utilizada pela Hotmart. Centraliza a autenticação de todos os colaboradores para os sistemas e ferramentas da empresa.

**Contexto na Hotmart:** ponto de entrada para praticamente todas as ferramentas internas: AWS, Jira, Vault, SonarQube, Grafana, etc. Acessível em [hotmart.okta.com](https://hotmart.okta.com). Todos os acessos aprovados via Service Desk aparecem no painel do Okta.

---

### Spot

Plataforma de otimização de custos de infraestrutura cloud. Gerencia automaticamente o uso de Spot Instances e outras estratégias de redução de custo na AWS, garantindo disponibilidade mesmo com instâncias preemptíveis.

**Contexto na Hotmart:** utilizado para reduzir custos de infraestrutura ao aproveitar Spot Instances da AWS de forma inteligente, com fallback automático para On-Demand quando necessário.

---

### DAS

Ferramenta interna relacionada a serviços de dados da plataforma Hotmart. Utilizada pelo time DevOps para automações e operações específicas do ecossistema de dados.

**Contexto na Hotmart:** consulte a documentação interna e o material de treinamento do Training Camp para detalhes sobre o uso atual do DAS na plataforma.

---

### SIEM (Security Information and Event Management)

Sistema que coleta, correlaciona e analisa eventos de segurança de múltiplas fontes para detectar ameaças, anomalias e violações de política em tempo real.

**Contexto na Hotmart:** utilizado pela equipe de SecOps para monitoramento de segurança centralizado. Logs de infraestrutura, acessos e eventos de sistema são enviados ao SIEM para análise e auditoria.

---

## Workflows e Processos

### Service Desk

Portal oficial de suporte da Hotmart onde colaboradores abrem solicitações para o time DevOps: pedidos de acesso, suporte técnico, problemas de infraestrutura e solicitações de configuração.

**Contexto na Hotmart:** toda solicitação ao time DevOps deve passar pelo Service Desk para garantir rastreabilidade. URL: [hotmart.atlassian.net/servicedesk](https://hotmart.atlassian.net/servicedesk).

---

### GDV (DevOps Tech & Data – Vulnerability & Weakness)

Tickets relacionados a vulnerabilidades e fraquezas de segurança identificadas pela equipe de segurança (SecOps) na infraestrutura e nos sistemas da Hotmart. O time DevOps é responsável por tratar os GDVs relacionados à infraestrutura.

**Contexto na Hotmart:** GDVs aparecem no backlog DevOps e têm SLA de resolução definido conforme a criticidade da vulnerabilidade. Fazem parte do trabalho operacional regular do time.

---

### Operations Task

Tarefas operacionais executadas pelo time DevOps no dia a dia: atualizações de versão, correções de infraestrutura, suporte a times de produto, otimizações de custo e manutenção geral da plataforma.

**Contexto na Hotmart:** rastreadas no backlog DevOps no Jira. Distinguem-se dos projetos trimestrais por serem tarefas recorrentes e de menor escopo, necessárias para manter a plataforma saudável.

---

### Smart Contract

Documento interno que define as boas práticas, diretrizes e acordos que guiam o trabalho diário do time DevOps. Cobre padrões de deploy, boas práticas de infraestrutura, processos de troubleshooting, responsabilidades do time e comunicação entre equipes.

**Contexto na Hotmart:** todos os membros do time DevOps devem conhecer e seguir o Smart Contract. Documento oficial: [Smart Contract](https://docs.google.com/presentation/d/1SdSBnz-yxGYtGLgD18UdjWT4Pr2WCFMPlwbsW4m8p8I/edit).

---

### JIRA Validator

Sistema que integra commits e Pull Requests do GitHub com tickets do Jira. Valida que cada PR está associado a um ticket válido, garantindo rastreabilidade entre o código produzido e o trabalho planejado.

**Contexto na Hotmart:** ao abrir um PR, o título ou a branch deve referenciar o número do ticket Jira correspondente. PRs sem referência válida podem ser bloqueados pelo JIRA Validator.

---

## Monitoramento

### New Relic

Plataforma de observabilidade de aplicações (APM). Fornece visibilidade sobre performance de código, traces distribuídos, taxa de erros, latência e comportamento interno das aplicações em produção.

**Contexto na Hotmart:** principal ferramenta de APM. Utilizado para investigar lentidão, erros e anomalias em aplicações. O agente é injetado automaticamente via Base Module.

---

### Datadog ⚠️ Escopo limitado

Plataforma de monitoramento de infraestrutura. Coleta métricas de nodes Kubernetes, containers, uso de recursos e rede.

**Contexto na Hotmart:** utilizado exclusivamente pela Product Unit **Teachable**. O Datadog está sendo removido da plataforma principal (Pyhot / Monitoring / Base-Module) e não é mais a ferramenta padrão de monitoramento de infraestrutura para a Hotmart como um todo. Para métricas de infraestrutura e cluster, a plataforma utiliza **Prometheus + Grafana**. Para monitoramento de custos em Kubernetes, utiliza-se o **Kubecost**.

---

### Kubecost

Ferramenta de monitoramento e otimização de custos em clusters Kubernetes. Fornece visibilidade sobre o custo real de namespaces, workloads e recursos, permitindo identificar desperdícios e otimizar a alocação.

**Contexto na Hotmart:** utilizado para monitoramento de custos nos clusters EKS. Permite ao time DevOps acompanhar quanto cada namespace, deployment ou PU está consumindo em termos de recursos e custo de infraestrutura.

---

### Sentry

Plataforma de monitoramento de erros e exceções em aplicações. Captura erros em tempo real com contexto completo: stack trace, breadcrumbs, informações do usuário e do ambiente.

**Contexto na Hotmart:** utilizado para rastreamento de erros em aplicações da plataforma. Complementa o NewRelic na camada de error tracking, fornecendo detalhes granulares sobre exceções e crashes.

---

### Prometheus

Sistema open source de coleta e armazenamento de métricas. Coleta métricas expostas por aplicações e componentes da plataforma via endpoints HTTP no formato de scraping.

**Contexto na Hotmart:** base para alertas internos e dashboards operacionais no Grafana. Métricas do Kubernetes, Karpenter, ArgoCD e aplicações são coletadas pelo Prometheus.

---

### Grafana

Plataforma open source de visualização de dados. Cria dashboards a partir de múltiplas fontes de dados como Prometheus e CloudWatch.

**Contexto na Hotmart:** ferramenta central de visualização operacional. Dashboards do time DevOps e de aplicações específicas ficam no Grafana. Inclui métricas de Karpenter, ARC e clusters EKS via Prometheus. Acessível via Okta.

---

### Pingdom

Serviço de monitoramento de uptime externo. Verifica a disponibilidade de endpoints públicos a partir de múltiplas regiões geográficas, simulando o comportamento de um usuário real.

**Contexto na Hotmart:** detecta indisponibilidade do ponto de vista do usuário final. Alertas do Pingdom são roteados para o JiraOps (via Grafana) e indicam que um serviço está inacessível externamente.

---

### Statping

Ferramenta de monitoramento de uptime para endpoints internos. Complementa o Pingdom com visibilidade sobre serviços que não são expostos publicamente.

**Contexto na Hotmart:** utilizado para monitorar a disponibilidade de serviços internos da plataforma que não têm exposição externa.

---

### PagerDuty ⚠️ Em substituição por JiraOps

Plataforma de gerenciamento de alertas e on-call que foi utilizada na Hotmart. O PagerDuty está sendo substituído pelo JiraOps (Jira Service Management Operations), que integra a gestão de incidentes diretamente com o Jira e o Grafana.

**Contexto na Hotmart:** a migração do PagerDuty para o JiraOps está em andamento. Os módulos base (terraform-base-module, monitoring, pyhot) já foram atualizados para remover a integração com o PagerDuty. Os alertas agora são roteados via Grafana para o JiraOps, que abre incidentes automaticamente no Jira Service Management. O repositório [pagerduty-iac](https://github.com/Hotmart-Org/pagerduty-iac) ainda existe como legado durante a transição.

### JiraOps (Jira Service Management Operations)

Solução de gestão de incidentes integrada ao Jira Service Management que está substituindo o PagerDuty na Hotmart. O JiraOps recebe alertas do Grafana e abre incidentes automaticamente, centralizando a gestão de alertas, escalação e on-call dentro do ecossistema Atlassian.

**Contexto na Hotmart:** os alertas de monitoramento (Grafana, NewRelic, Prometheus) são roteados para o JiraOps via contact points configurados no Grafana. Por padrão, alertas enviados para o JiraOps recebem prioridade P3. O canal `devops-jiraops` no Grafana é o contact point principal para abertura de incidentes, e o canal `devops-notification` envia alertas para o Google Chat do time.

---

### AlertManager

Componente do ecossistema Prometheus responsável por gerenciar alertas: deduplicação, agrupamento, silenciamento e roteamento para os destinos corretos (JiraOps, Google Chat, e-mail, etc.).

**Contexto na Hotmart:** processa os alertas gerados pelas regras do Prometheus e os encaminha para o JiraOps (via Grafana) ou outros canais conforme a configuração de roteamento.

---

### Cloudflare ⚠️ Em remoção

Plataforma de segurança, CDN e controle de tráfego que foi utilizada como a primeira camada de proteção para o tráfego externo da Hotmart.

**Contexto na Hotmart:** o Cloudflare está sendo removido da plataforma. Algumas contas ainda possuem configurações legadas em processo de migração. Se você encontrar referências ao Cloudflare em código ou configuração, trata-se de configuração legada.

---

## Bancos de Dados e Troubleshooting

### RDS (Relational Database Service)

Serviço de banco de dados relacional gerenciado da AWS. Cuida de tarefas operacionais como backups automáticos, patching, alta disponibilidade e failover, reduzindo a carga operacional de gerenciar bancos de dados.

**Contexto na Hotmart:** banco de dados relacional padrão para aplicações que precisam de persistência estruturada. Suporta MySQL, PostgreSQL, MariaDB, Oracle e SQL Server.

---

### Aurora

Banco de dados relacional gerenciado pela AWS, compatível com MySQL e PostgreSQL, com performance e disponibilidade superiores às versões padrão. Oferece escalabilidade automática de storage e réplicas de leitura de baixa latência.

**Contexto na Hotmart:** utilizado para workloads que exigem alta performance e disponibilidade. O Aurora Serverless é uma opção para workloads com demanda variável, pois escala automaticamente conforme o uso.

---

### Vanilla MySQL

Versão padrão do MySQL sem as customizações e otimizações do Aurora. Geralmente utilizado em cenários onde a compatibilidade estrita com MySQL padrão é necessária ou onde o overhead do Aurora não se justifica.

**Contexto na Hotmart:** presente em alguns serviços legados ou em casos específicos onde o Aurora não é adequado. Ao trabalhar com bancos de dados desta categoria, verifique se há planos de migração para Aurora.

---

### Troubleshooting

Processo estruturado de investigação e resolução de problemas em sistemas distribuídos. Envolve coleta de evidências (logs, métricas, traces), formulação e teste de hipóteses, identificação de causa raiz e execução de ação corretiva.

**Contexto na Hotmart:** habilidade central para qualquer engenheiro DevOps. O time segue um fluxo estruturado de troubleshooting durante incidentes, documentado em [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow). Bom troubleshooting combina conhecimento técnico com método: não com tentativa e erro aleatório.
