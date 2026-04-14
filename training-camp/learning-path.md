# Learning Path

## 📑 Índice

- [Como usar este documento](#como-usar-este-documento)
- [Etapa 1: Base Module](#etapa-1--base-module)
- [Etapa 2: Monitoring](#etapa-2--monitoring)
- [Etapa 3: Secrets](#etapa-3--secrets)
- [Etapa 4: Troubleshooting](#etapa-4--troubleshooting)
- [Etapa 5: Tópicos complementares](#etapa-5--tópicos-complementares)
- [Dicas para o estudo](#dicas-para-o-estudo)

---

Esta é a trilha de estudo recomendada para novos engenheiros DevOps durante o Training Camp. A ordem importa: cada etapa constrói sobre a anterior. Não pule etapas: mesmo que você já conheça o assunto, vale revisar no contexto da plataforma Hotmart.

---

## Como usar este documento

Para cada etapa:
1. Leia o material indicado
2. Explore os repositórios referenciados
3. Teste os comandos e conceitos na prática
4. Anote dúvidas e discuta com seu mentor (fellow)
5. Marque o progresso na Skill Matrix

---

## Etapa 1: Base Module

**Por que começar aqui:** o Base Module é o ponto de entrada para entender como infraestrutura é provisionada na Hotmart. Quase tudo que você vai operar foi criado por ele. Entender o Base Module dá contexto para todas as outras etapas.

**O que estudar:**
- O que é o Base Module e por que ele existe
- Como o YAML declarativo funciona
- Quais recursos AWS são criados automaticamente
- As variantes: `base-module`, `base-module-lambda`, `base-module-agentcore`, `base-module-nomad`
- Como o Base Module se integra com o pipeline de CI/CD e o ArgoCD

**Atividades práticas:**
- Leia o repositório [terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module) e entenda a estrutura de arquivos
- Analise um YAML de configuração de uma aplicação real no repositório [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac)
- Discuta com seu mentor: "o que acontece quando eu mudo um valor nesse YAML?"

**Materiais internos:**

<details>
<summary>▶️ IAC + Base Module: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1XGCX2qK7eep7TXeMzzUs2iGRc_cDhoUL/view)

</details>

📊 [Slides: Base Module](https://docs.google.com/presentation/d/18XEz6UfkMBxUj09o7kuEjkhukR6BVtN7YEnEd49KtBs/edit#slide=id.g134822534b4_0_42)

📄 [`platform-overview/base-module.md`](../platform-overview/base-module)

---

## Etapa 2: Monitoring

**Por que vem aqui:** depois de entender como a infraestrutura é criada, o próximo passo é entender como ela é observada. Monitoramento é a base para qualquer atividade operacional: sem ele, você opera no escuro.

**O que estudar:**
- O papel de cada ferramenta: NewRelic, Datadog, Prometheus, Grafana, Pingdom, PagerDuty, CloudWatch
- Como o Base Module integra monitoramento automaticamente
- Como navegar no Grafana e interpretar dashboards
- Como ler traces no NewRelic e identificar gargalos
- Como os alertas chegam no PagerDuty e como responder a eles

**Atividades práticas:**
- Acesse o Grafana e explore os dashboards existentes de uma aplicação real
- Abra o NewRelic e navegue pelos traces de uma aplicação em produção
- Verifique como um alerta está configurado no PagerDuty
- Pergunte ao mentor: "qual dashboard você usa primeiro quando recebe um alerta?"

**Materiais internos:**

<details>
<summary>▶️ Monitoring Applications: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1dAMDRPcs9q8BXOvqXWrGj1sfQCCdw-xN/view)

</details>

<details>
<summary>▶️ KEDA: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1RmMQr5i-5be87p21nZypY-ye2feV0ysP/view)

</details>

📊 [Slides: Monitoring](https://docs.google.com/presentation/d/10mv8d1JLiPlwqUc9GEqFLSgRsa6nBa_u2_ljLUkqccE/edit#slide=id.g134822534b4_0_42)

📄 [`platform-overview/observability-stack.md`](../platform-overview/observability-stack)

---

## Etapa 3: Secrets

**Por que vem aqui:** com infraestrutura e monitoramento entendidos, é hora de entender como dados sensíveis são gerenciados. Secrets mal gerenciados são uma das principais causas de incidentes de segurança.

**O que estudar:**
- O que é o HashiCorp Vault e como ele funciona
- Como o Base Module integra com o Vault e o Secrets Manager
- O ciclo de vida de um secret: criação, rotação, acesso e revogação
- Como as aplicações acessam secrets em runtime (sem hardcode)
- Boas práticas de segurança para secrets em pipelines de CI/CD

**Atividades práticas:**
- Acesse o Vault via Okta e explore a estrutura de paths
- Analise como um secret é referenciado em um YAML de configuração de aplicação
- Verifique como o EKS Pod Identity é usado para dar acesso ao Secrets Manager
- Discuta com o mentor: "como eu rotaciono um secret sem causar downtime?"

**Materiais internos:**

<details>
<summary>▶️ Vault: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1Npv25c62Co1nFGIHpzHxoQGqkMN7dX3y/view)

</details>

<details>
<summary>▶️ SIEM: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1bc5vh6douFHgbZsCiqV8e0DvfI2v1ucU/view)

</details>

<details>
<summary>▶️ Cloudflare: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1X6Bx4fiUn6bXFhju8T2mQ_91kF1Ny8cw/view)

</details>

📊 [Slides: Secrets](https://docs.google.com/presentation/d/1X_lXlyi3Fm8Qsj1aCATIUxQPu2RBrE0SQfFxxrR2HU0/edit#slide=id.ge9493f02a1_0_2)

---

## Etapa 4: Troubleshooting

**Por que vem aqui:** com o conhecimento das três etapas anteriores, você já tem o contexto necessário para investigar problemas de forma estruturada. Troubleshooting é uma habilidade que combina conhecimento técnico com método.

**O que estudar:**
- Metodologia de investigação: do sintoma à causa raiz
- Como usar `kubectl` para inspecionar workloads com problema
- Como correlacionar logs, métricas e traces para identificar a origem de um problema
- Padrões comuns de falha em ambientes Kubernetes
- Como documentar e comunicar um problema durante um incidente

**Atividades práticas:**
- Siga o Troubleshooting Guide interno do time
- Simule um cenário de pod em CrashLoopBackOff e investigue a causa
- Pratique os comandos de diagnóstico:

```bash
# Ver eventos recentes de um namespace
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Inspecionar um pod com problema
kubectl describe pod <pod-name> -n <namespace>

# Ver logs de um container específico
kubectl logs <pod-name> -c <container-name> -n <namespace>

# Ver logs de um pod que reiniciou (logs do container anterior)
kubectl logs <pod-name> --previous -n <namespace>

# Verificar uso de recursos
kubectl top pods -n <namespace>
```

- Discuta com o mentor um incidente real que aconteceu recentemente e como foi investigado

**Materiais internos:**

<details>
<summary>▶️ Troubleshooting: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1-9II___dGTomKbXN-mDn2tk4GSuvAHeB/view)

</details>

📊 [Slides: TroubleShooting & RDS](https://docs.google.com/presentation/d/1z7VtSBIesRg3tyAE5b_38waiDlcMXI5obGdrfU71akA/edit#slide=id.g20271aefe8d_0_191)

📄 [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow)

---

## Etapa 5: Tópicos complementares

Após as quatro etapas principais, explore os tópicos complementares conforme sua disponibilidade e os gaps identificados na Skill Matrix:

- **Backstage:** como criar uma nova aplicação via portal de developer experience
- **DAS:** ferramenta interna de automação
- **Banco de dados:** como provisionar e gerenciar bancos de dados na plataforma
- **Integrações de comunicação:** WhatsApp e outros canais integrados à plataforma

**Materiais internos:**

<details>
<summary>▶️ Criando uma app do zero (Backstage): Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1nC2e-jL7QlIkZfKfYhgrXXrfGFo_ymeM/view)

</details>

<details>
<summary>▶️ DAS: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1vIqonkris-k_Q4bS_6oSW3wKCos5ZoWN/view)

</details>

<details>
<summary>▶️ Criando um banco de dados do zero: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/14akfFasiIyBji2rIbHmO41SIr3rqErVc/view)

</details>

<details>
<summary>▶️ WhatsApp Integrations: Gravação</summary>

[🎬 Abrir vídeo](https://drive.google.com/file/d/1V2blKlkwmmQttZQ6IcJBXHYPOx7lEmo6/view)

</details>

---

## Dicas para o estudo

- **Não estude passivamente.** Abra o terminal, acesse as ferramentas e teste o que está aprendendo.
- **Use o mentor.** Seu fellow está disponível para tirar dúvidas. Não acumule dúvidas sem resolver.
- **Explore repositórios reais.** O código da organização é o melhor material de estudo disponível.
- **Atualize a Skill Matrix** ao longo do estudo para acompanhar seu progresso e identificar gaps.
- **Não precisa ser perfeito.** O objetivo do Training Camp é construir uma base sólida, não dominar tudo. A profundidade vem com a prática operacional.
