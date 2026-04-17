# Cultura do Time DevOps

## 📑 Índice

- [Como decisões técnicas são tomadas](#como-decisões-técnicas-são-tomadas)
- [Revisão de Pull Requests](#revisão-de-pull-requests)
- [Dinâmica de trabalho](#dinâmica-de-trabalho)
- [Canais de comunicação por PU](#canais-de-comunicação-por-pu)
- [Plantão e on-call](#plantão-e-on-call)
- [Expectativas para novos engenheiros](#expectativas-para-novos-engenheiros)

---

Este documento descreve como o time DevOps da Hotmart funciona na prática: como decisões são tomadas, como o trabalho é organizado e o que se espera de cada engenheiro.

---

## Como decisões técnicas são tomadas

### Architecture Decision Records (ADRs)

Decisões técnicas relevantes são documentadas como ADRs no repositório [devops-docs](https://github.com/Hotmart-Org/devops-docs/tree/master/docs/architecture/decisions). Cada ADR registra o contexto, as opções avaliadas, a decisão tomada e as consequências esperadas.

Exemplos de ADRs existentes:
- Escolha de ferramenta de GitOps (ArgoCD)
- Estrutura de AppProjects no ArgoCD
- Modo de runner do ARC (Actions Runner Controller)
- Estratégia de NodePools do Karpenter
- Estrutura de Helm charts para aplicações

Quando uma decisão técnica impacta a plataforma como um todo, ela passa por discussão no grupo geral dos DevOps antes de ser implementada.

### Design Documents

Para mudanças maiores na arquitetura, o time produz design documents que detalham o problema, a solução proposta e o plano de implementação. Esses documentos ficam em [devops-docs/architecture/design](https://github.com/Hotmart-Org/devops-docs/tree/master/docs/architecture/design).

---

## Revisão de Pull Requests

PRs são o principal mecanismo de mudança na plataforma. Toda alteração em infraestrutura, configuração ou código passa por revisão.

### Checklist de revisão de IAC

O time segue um [checklist de revisão para PRs de IAC](https://github.com/Hotmart-Org/devops-docs/blob/master/docs/process/pastel/iacs-pr-review.md) que inclui:

- Avaliar o plano de execução (verificar se há recursos sendo destruídos)
- Verificar se o recurso está na stack correta
- Validar políticas de IAM (nunca aprovar com `*` em main, resource ou action)
- Verificar WAF em CDNs (obrigatório exceto em buildstaging)
- Validar configurações de S3, secrets, RDS, Redis e subnet groups
- Verificar se arrays foram modificados corretamente (novos itens sempre no final, nunca no início ou meio)

### Boas práticas

- PRs devem ter descrição clara do que está sendo alterado e por quê
- Vincule o PR ao ticket do Jira correspondente
- Para mudanças sensíveis (produção, segurança), solicite revisão de mais de uma pessoa
- Acompanhe PRs abertos nos repos de infraestrutura: ler diffs e comentários de revisão é uma forma excelente de aprender os padrões do time

---

## Dinâmica de trabalho

### Cerimônias

O time participa das cerimônias ágeis padrão:
- Dailies
- Plannings
- Retrospectivas

Cada PU tem seu próprio board no Jira e suas cerimônias. O DevOps participa das cerimônias da PU à qual está associado.

### Gestão de tarefas no Jira

O trabalho é organizado em uma hierarquia:

```
Initiative (PORTM)
  └── Epic
       └── Story
            └── Sub-task
```

- Initiatives são criadas no projeto de Portfolio Management
- Epics agrupam um tema (ex: "Migração de EKS da PU Leads Solution")
- Stories são entregas de valor menores dentro do epic
- Sub-tasks são os passos para completar uma story

Boards por PU estão disponíveis no Jira. Pergunte ao seu fellow qual é o board da sua PU.

### Demandas operacionais

As demandas do dia a dia chegam por dois caminhos:

- **Gestor do time**: tarefas como atualização de EKS, atualização de Helm charts, otimização de custos AWS
- **Service Desk + Google Chat**: suporte a times de desenvolvimento. Cada PU tem um grupo no Google Chat onde desenvolvedores pedem ajuda. Problemas que exigem mais tempo são convertidos em tickets no Jira

---

## Canais de comunicação por PU

Cada Product Unit tem um grupo no Google Chat onde os desenvolvedores da PU interagem com o DevOps responsável. Esses grupos são um canal importante de suporte e comunicação.

Além dos grupos por PU, existe o grupo geral dos DevOps para discussões que impactam todo o time: decisões técnicas, mudanças na plataforma, avisos gerais.

Incidentes e falhas também podem ser reportados via chat pelos desenvolvedores, mesmo antes de aparecerem nas ferramentas de monitoramento.

---

## Plantão e on-call

O plantão de on-call ocorre nos finais de semana e é atribuído a engenheiros DevOps com mais experiência na plataforma.

### Como funciona

- Nos dias úteis (08:00-18:00): alertas são enviados para o email do time
- Nos dias úteis (18:00-08:00): alertas são escalados para o gestor
- Nos finais de semana (08:00-18:00): o plantonista recebe os alertas
- Nos finais de semana (18:00-08:00): alertas são escalados para o gestor

A escala de plantão é gerenciada via JiraOps (Jira Service Management) e definida como código no repositório [pagerduty-iac](https://github.com/Hotmart-Org/pagerduty-iac) (legado, em migração).

### Feriados

Feriados não são cobertos automaticamente pela escalation policy (limitação da ferramenta). O plantão em feriados é lançado manualmente via Override no JiraOps.

### Troca de plantão

Para trocar o plantão, use a funcionalidade de Override no JiraOps: `People > On-Call Schedules > Schedule an override`.

### Durante o onboarding

Novos engenheiros não participam da rotação de plantão durante o onboarding. O foco é acompanhar incidentes que acontecem no dia a dia como observador, entender o processo e se preparar gradualmente.

---

## Expectativas para novos engenheiros

- Pergunte. O time tem cultura de compartilhamento de conhecimento. Dúvidas não resolvidas viram gaps que aparecem na hora errada.
- Documente. Se resolveu um problema que não estava documentado, documente. Se encontrou algo errado, abra uma Issue.
- Acompanhe PRs. Ler PRs de infraestrutura é uma das melhores formas de aprender como o time trabalha.
- Participe das cerimônias. Mesmo que no início você não tenha muito a contribuir, estar presente ajuda a absorver contexto.
- Não tenha medo de errar em staging. O ambiente de staging existe para isso.
- Registre seu progresso na Issue do GitHub. Seu fellow e gestor acompanham por lá.
