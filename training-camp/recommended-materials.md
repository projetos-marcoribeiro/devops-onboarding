# Materiais Recomendados

## 📑 Índice

- [Pasta compartilhada](#pasta-compartilhada)
- [Infraestrutura](#infraestrutura)
- [Observabilidade](#observabilidade)
- [Segurança](#segurança)
- [Ferramentas e Automação](#ferramentas-e-automação)
- [Banco de Dados](#banco-de-dados)
- [Troubleshooting](#troubleshooting)
- [Comunicação](#comunicação)
- [Apresentações de Referência](#apresentações-de-referência)

---

Estes materiais foram produzidos internamente pelo time DevOps da Hotmart. São gravações de sessões de treinamento e apresentações criadas por engenheiros do próprio time, com foco nos padrões e ferramentas reais da plataforma.

Consuma os materiais na ordem sugerida pelo [Learning Path](learning-path), não aleatoriamente. O contexto acumulado em cada etapa torna os materiais seguintes mais fáceis de absorver.

> ⚠️ **Sobre os links de vídeo:** os materiais estão hospedados no Google Drive corporativo. Se algum link não funcionar, verifique se você está logado com sua conta `@hotmart.com`. Se ainda assim não abrir, procure o material na pasta compartilhada abaixo ou peça ao fellow para solicitar acesso. Se um vídeo foi removido ou movido, atualize este documento com o novo link: é uma contribuição rápida e valiosa.

---

## Pasta compartilhada

Todos os materiais de treinamento estão centralizados nesta pasta do Google Drive. Se algum link abaixo estiver desatualizado, procure aqui:

📂 [Treinamento DevOps: Google Drive](https://drive.google.com/drive/folders/1EbEAxfw6X6FBl-ARgiEbTWEOaiItUn_6)

---

## Infraestrutura

Materiais fundamentais para entender como a infraestrutura da Hotmart é provisionada e gerenciada.

### IAC + Base Module

Gravação sobre Infrastructure as Code e o terraform-base-module: o que é, como funciona, quais recursos cria e como os times de produto o utilizam para provisionar infraestrutura de forma declarativa.

**Quando estudar:** início do Training Camp, antes de qualquer outra coisa.

<details>
<summary>▶️ Assistir gravação: IAC + Base Module</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1XGCX2qK7eep7TXeMzzUs2iGRc_cDhoUL/view)

</details>

📊 [Slides: Base Module](https://docs.google.com/presentation/d/18XEz6UfkMBxUj09o7kuEjkhukR6BVtN7YEnEd49KtBs/edit#slide=id.g134822534b4_0_42)

📄 Documentação: [`platform-overview/base-module.md`](../platform-overview/base-module)

---

### Hotmart Charts

Gravação sobre os Helm Charts utilizados na plataforma. Explica a estrutura dos charts, como os values são organizados por ambiente e como o ArgoCD os utiliza para fazer deploy das aplicações.

**Quando estudar:** após entender o Base Module e o fluxo de deploy com ArgoCD.

<details>
<summary>▶️ Assistir gravação: Hotmart Charts</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1dk5x_cXQl2h98fDGhou83CRb_NtjaMWp/view)

</details>

---

## Observabilidade

Materiais sobre como as aplicações e a infraestrutura são monitoradas na Hotmart.

### Monitoring Applications

Guia completo sobre a stack de observabilidade: NewRelic, Prometheus, Grafana, Sentry, Kubecost e PagerDuty. O Datadog é abordado no contexto da PU Teachable. Explica como cada ferramenta é usada, como o monitoramento é configurado automaticamente pelo Base Module e como navegar nas ferramentas durante um incidente.

**Quando estudar:** etapa 2 do Learning Path.

<details>
<summary>▶️ Assistir gravação: Monitoring Applications</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1dAMDRPcs9q8BXOvqXWrGj1sfQCCdw-xN/view)

</details>

📊 [Slides: Monitoring](https://docs.google.com/presentation/d/10mv8d1JLiPlwqUc9GEqFLSgRsa6nBa_u2_ljLUkqccE/edit#slide=id.g134822534b4_0_42)

📄 Documentação: [`platform-overview/observability-stack.md`](../platform-overview/observability-stack)

---

### KEDA

Apresentação sobre o Kubernetes Event-Driven Autoscaling (KEDA). Explica como o KEDA é utilizado na plataforma para escalar workloads com base em eventos externos (filas SQS, métricas customizadas, etc.), complementando o autoscaling padrão do Kubernetes.

**Quando estudar:** após o material de Monitoring Applications.

<details>
<summary>▶️ Assistir gravação: KEDA</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1RmMQr5i-5be87p21nZypY-ye2feV0ysP/view)

</details>

---

## Segurança

Materiais sobre práticas e ferramentas de segurança utilizadas na plataforma.

### Vault

Guia sobre o HashiCorp Vault: como ele é utilizado para gerenciar secrets na Hotmart, como as aplicações acessam credenciais em runtime e como o ciclo de vida de um secret é gerenciado.

**Quando estudar:** etapa 3 do Learning Path.

<details>
<summary>▶️ Assistir gravação: Vault</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1Npv25c62Co1nFGIHpzHxoQGqkMN7dX3y/view)

</details>

📊 [Slides: Secrets](https://docs.google.com/presentation/d/1X_lXlyi3Fm8Qsj1aCATIUxQPu2RBrE0SQfFxxrR2HU0/edit#slide=id.ge9493f02a1_0_2)

---

### SIEM

Apresentação sobre o sistema de gerenciamento de eventos de segurança (SIEM) utilizado na Hotmart. Explica como logs de segurança são coletados, correlacionados e analisados para detecção de ameaças e auditoria.

**Quando estudar:** após o material de Vault.

<details>
<summary>▶️ Assistir gravação: SIEM</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1bc5vh6douFHgbZsCiqV8e0DvfI2v1ucU/view)

</details>

---

### Cloudflare

Documentação sobre o uso do Cloudflare na plataforma: proteção DDoS, WAF, regras de segurança e como o Cloudflare se integra com a infraestrutura AWS da Hotmart.

**Quando estudar:** junto com os materiais de segurança ou após o estudo de networking.

<details>
<summary>▶️ Assistir gravação: Cloudflare</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1X6Bx4fiUn6bXFhju8T2mQ_91kF1Ny8cw/view)

</details>

---

## Ferramentas e Automação

### Criando uma aplicação via Backstage

Guia passo a passo para criar uma nova aplicação utilizando o portal Backstage da Hotmart. Explica como o Backstage automatiza o scaffolding de repositórios, configurações de CI/CD e provisionamento inicial de infraestrutura via Base Module.

**Quando estudar:** após entender o Base Module e o fluxo de deploy.

<details>
<summary>▶️ Assistir gravação: Criando uma app do zero (Backstage)</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1nC2e-jL7QlIkZfKfYhgrXXrfGFo_ymeM/view)

</details>

---

### DAS

Apresentação sobre o DAS (ferramenta interna de automação). Explica como o DAS é utilizado pelo time DevOps para automatizar tarefas operacionais recorrentes na plataforma.

**Quando estudar:** após os materiais de infraestrutura e CI/CD.

<details>
<summary>▶️ Assistir gravação: DAS</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1vIqonkris-k_Q4bS_6oSW3wKCos5ZoWN/view)

</details>

---

## Banco de Dados

### Criando um banco de dados do zero

Guia completo para provisionar e configurar um banco de dados na plataforma Hotmart. Cobre a escolha do tipo de banco, provisionamento via Base Module, configuração de acesso seguro via Vault e boas práticas de operação.

**Quando estudar:** quando precisar trabalhar com aplicações que utilizam banco de dados, ou como parte do Hands-on Project.

<details>
<summary>▶️ Assistir gravação: Criando um banco de dados do zero</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/14akfFasiIyBji2rIbHmO41SIr3rqErVc/view)

</details>

---

## Troubleshooting

### Troubleshooting Guide

Guia de investigação de problemas em ambientes distribuídos. Apresenta uma metodologia estruturada para ir do sintoma à causa raiz, com exemplos de cenários comuns na plataforma Hotmart.

**Quando estudar:** etapa 4 do Learning Path. Também é uma referência útil durante incidentes reais.

<details>
<summary>▶️ Assistir gravação: Troubleshooting</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1-9II___dGTomKbXN-mDn2tk4GSuvAHeB/view)

</details>

📊 [Slides: TroubleShooting & RDS](https://docs.google.com/presentation/d/1z7VtSBIesRg3tyAE5b_38waiDlcMXI5obGdrfU71akA/edit#slide=id.g20271aefe8d_0_191)

📄 Documentação: [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow)

---

## Comunicação

### WhatsApp Integrations

Documentação sobre as integrações de comunicação via WhatsApp utilizadas na plataforma.

<details>
<summary>▶️ Assistir gravação: WhatsApp Integrations</summary>

[🎬 Clique para assistir no Google Drive](https://drive.google.com/file/d/1V2blKlkwmmQttZQ6IcJBXHYPOx7lEmo6/view)

</details>

---

## Apresentações de Referência

Resumo rápido de todos os materiais para consulta:

| Material | Gravação | Slides |
|---|---|---|
| IAC + Base Module | [🎬 Vídeo](https://drive.google.com/file/d/1XGCX2qK7eep7TXeMzzUs2iGRc_cDhoUL/view) | [📊 Slides](https://docs.google.com/presentation/d/18XEz6UfkMBxUj09o7kuEjkhukR6BVtN7YEnEd49KtBs/edit) |
| Hotmart Charts | [🎬 Vídeo](https://drive.google.com/file/d/1dk5x_cXQl2h98fDGhou83CRb_NtjaMWp/view) | - |
| Monitoring Applications | [🎬 Vídeo](https://drive.google.com/file/d/1dAMDRPcs9q8BXOvqXWrGj1sfQCCdw-xN/view) | [📊 Slides](https://docs.google.com/presentation/d/10mv8d1JLiPlwqUc9GEqFLSgRsa6nBa_u2_ljLUkqccE/edit) |
| KEDA | [🎬 Vídeo](https://drive.google.com/file/d/1RmMQr5i-5be87p21nZypY-ye2feV0ysP/view) | - |
| Vault | [🎬 Vídeo](https://drive.google.com/file/d/1Npv25c62Co1nFGIHpzHxoQGqkMN7dX3y/view) | [📊 Slides](https://docs.google.com/presentation/d/1X_lXlyi3Fm8Qsj1aCATIUxQPu2RBrE0SQfFxxrR2HU0/edit) |
| SIEM | [🎬 Vídeo](https://drive.google.com/file/d/1bc5vh6douFHgbZsCiqV8e0DvfI2v1ucU/view) | - |
| Cloudflare | [🎬 Vídeo](https://drive.google.com/file/d/1X6Bx4fiUn6bXFhju8T2mQ_91kF1Ny8cw/view) | - |
| Backstage | [🎬 Vídeo](https://drive.google.com/file/d/1nC2e-jL7QlIkZfKfYhgrXXrfGFo_ymeM/view) | - |
| DAS | [🎬 Vídeo](https://drive.google.com/file/d/1vIqonkris-k_Q4bS_6oSW3wKCos5ZoWN/view) | - |
| Banco de dados do zero | [🎬 Vídeo](https://drive.google.com/file/d/14akfFasiIyBji2rIbHmO41SIr3rqErVc/view) | - |
| Troubleshooting | [🎬 Vídeo](https://drive.google.com/file/d/1-9II___dGTomKbXN-mDn2tk4GSuvAHeB/view) | [📊 Slides](https://docs.google.com/presentation/d/1z7VtSBIesRg3tyAE5b_38waiDlcMXI5obGdrfU71akA/edit) |
| WhatsApp | [🎬 Vídeo](https://drive.google.com/file/d/1V2blKlkwmmQttZQ6IcJBXHYPOx7lEmo6/view) | - |

📂 [Pasta completa: Treinamento DevOps](https://drive.google.com/drive/folders/1EbEAxfw6X6FBl-ARgiEbTWEOaiItUn_6)
