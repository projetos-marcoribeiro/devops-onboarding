# Tarefas Operacionais

## 📑 Índice

- [Tipos de tarefas operacionais](#tipos-de-tarefas-operacionais)
- [Como novos engenheiros começam](#como-novos-engenheiros-começam)
- [Atividades recomendadas durante o onboarding](#atividades-recomendadas-durante-o-onboarding)
- [Ferramentas usadas no trabalho operacional](#ferramentas-usadas-no-trabalho-operacional)
- [Referências](#referências)

---

O trabalho operacional do time DevOps é variado e contínuo. Além dos projetos trimestrais e do atendimento ao Service Desk, existe uma camada de tarefas recorrentes que mantêm a plataforma saudável, segura e atualizada.

Este documento descreve os tipos de tarefas que aparecem no dia a dia e como novos engenheiros podem começar a contribuir.

---

## Tipos de tarefas operacionais

### Atualização de versões de EKS

Os clusters EKS precisam ser atualizados periodicamente para manter suporte da AWS, receber correções de segurança e ter acesso a novos recursos do Kubernetes.

O processo envolve:
1. Verificar a versão atual dos clusters e o calendário de fim de suporte da AWS
2. Testar a atualização em clusters de desenvolvimento/staging primeiro
3. Validar que os workloads continuam funcionando após a atualização
4. Atualizar os node groups e os add-ons do EKS
5. Documentar e comunicar a atualização ao time

```bash
# Verificar versão atual do cluster
aws eks describe-cluster --name <cluster-name> --query "cluster.version"

# Listar versões disponíveis para atualização
aws eks describe-addon-versions --kubernetes-version 1.30
```

### Atualização de Helm Charts

Os Helm Charts utilizados na plataforma precisam ser atualizados para incorporar melhorias, correções de segurança e novos padrões. Isso inclui tanto os charts de infraestrutura mantidos pelo time DevOps quanto os charts de terceiros (cert-manager, external-dns, karpenter, etc.).

O processo envolve:
1. Verificar novas versões disponíveis
2. Ler o changelog para identificar breaking changes
3. Testar em ambiente de desenvolvimento
4. Abrir PR com a atualização
5. Fazer deploy gradual (dev → staging → produção)

### Otimização de custos AWS

A otimização de custos é uma responsabilidade contínua do time DevOps. Isso inclui:

- Identificar recursos ociosos ou superprovisionados (instâncias EC2, RDS, etc.)
- Ajustar requests e limits de pods para refletir o uso real
- Migrar workloads elegíveis para Spot Instances via Karpenter
- Revisar e remover recursos não utilizados (snapshots, AMIs, load balancers órfãos)
- Analisar relatórios de custo por conta e por serviço

```bash
# Ver pods com alto consumo de recursos
kubectl top pods -A --sort-by=memory

# Identificar nodes ociosos
kubectl top nodes
```

### Correção de bugs de infraestrutura

Problemas menores de infraestrutura aparecem regularmente: configurações incorretas, recursos mal dimensionados, integrações quebradas, certificados próximos do vencimento, etc.

Exemplos comuns:
- Pod em `CrashLoopBackOff` por configuração incorreta de variável de ambiente
- Certificado TLS próximo do vencimento
- Alerta disparando com threshold incorreto
- Pipeline falhando por dependência desatualizada

### Suporte a times de desenvolvimento

Times de produto frequentemente precisam de ajuda com infraestrutura: configurar um novo pipeline, entender por que um deploy falhou, ajustar recursos de um workload, criar um novo ambiente, etc.

Esse suporte chega principalmente via Service Desk, mas também via canais de comunicação do time.

---

## Como novos engenheiros começam

Engenheiros recém-chegados normalmente começam com tarefas menores para ganhar familiaridade com a plataforma antes de assumir responsabilidades maiores. Isso é intencional: operar com segurança exige contexto que só vem com a prática gradual.

**Tarefas recomendadas para começar:**

- Atender tickets simples do Service Desk com orientação do mentor
- Fazer atualizações de versão de Helm Charts em ambientes de desenvolvimento
- Corrigir alertas com threshold incorreto
- Ajustar requests/limits de pods com base em dados de uso real
- Documentar runbooks para tarefas que ainda não estão documentadas

---

## Atividades recomendadas durante o onboarding

Além de executar tarefas, existem atividades de aprendizado que aceleram muito a familiarização com a plataforma:

### Investigar tickets antigos

Leia tickets resolvidos no Service Desk e no backlog DevOps. Entender como problemas passados foram investigados e resolvidos é uma das formas mais eficientes de aprender sobre a plataforma.

Foque em tickets de incidentes e bugs de infraestrutura: eles geralmente têm o raciocínio de troubleshooting documentado nos comentários.

### Revisar Pull Requests de infraestrutura

Acompanhe os PRs abertos e recentemente mergeados nos repositórios de infraestrutura ([devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac), [terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module)). Ler o diff e os comentários de revisão é uma forma excelente de entender os padrões do time e como decisões técnicas são tomadas.

### Acompanhar troubleshooting de incidentes

Quando um incidente acontecer, peça para acompanhar a investigação mesmo que você não esteja de plantão. Observar como engenheiros experientes investigam e resolvem problemas em produção é insubstituível.

Após o incidente, leia o postmortem e tente entender cada etapa da investigação.

---

## Ferramentas usadas no trabalho operacional

| Ferramenta | Uso |
|---|---|
| kubectl | Inspecionar e operar workloads no Kubernetes |
| AWS CLI | Interagir com serviços AWS |
| hotctl | CLI interna para operações na plataforma |
| Grafana | Dashboards e métricas operacionais |
| NewRelic | APM e investigação de performance |
| Datadog | Monitoramento de infraestrutura (Teachable) |
| PagerDuty | Alertas e gestão de on-call |
| ArgoCD | Acompanhar e gerenciar deploys |
| Jira | Rastrear e gerenciar tarefas |

---

## Referências

📄 [`operations/jira-workflows.md`](jira-workflows)
📄 [`operations/service-desk-tickets.md`](service-desk-tickets)
📄 [`incident-management/troubleshooting-flow.md`](../incident-management/troubleshooting-flow)
