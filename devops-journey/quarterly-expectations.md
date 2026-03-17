# Expectativas Trimestrais

## 📑 Índice

- [Como funciona o ciclo trimestral](#como-funciona-o-ciclo-trimestral)
- [Tipos de trabalho trimestral](#tipos-de-trabalho-trimestral)
- [Quando o engenheiro começa a contribuir nos Quarters](#quando-o-engenheiro-começa-a-contribuir-nos-quarters)
- [Exemplos de tarefas trimestrais por nível de complexidade](#exemplos-de-tarefas-trimestrais-por-nível-de-complexidade)
- [Acompanhamento e visibilidade](#acompanhamento-e-visibilidade)
- [Uma perspectiva importante](#uma-perspectiva-importante)
- [Referências](#referências)

---

O trabalho DevOps na Hotmart é organizado em ciclos trimestrais, conhecidos internamente como Quarters (Q1, Q2, Q3, Q4). Cada trimestre tem um planejamento próprio com projetos estruturais, melhorias de infraestrutura e iniciativas técnicas que guiam o trabalho do time além das tarefas operacionais do dia a dia.

Entender como esse ciclo funciona é fundamental para que o engenheiro saiba como seu trabalho se encaixa no contexto maior da plataforma.

---

## Como funciona o ciclo trimestral

Cada trimestre começa com um processo de planejamento onde o time define as iniciativas prioritárias para os próximos três meses. Esse planejamento leva em conta:

- Débitos técnicos identificados no trimestre anterior
- Demandas vindas dos times de produto
- Iniciativas estratégicas da área de Platform Engineering
- Melhorias de confiabilidade, segurança e eficiência operacional
- Oportunidades de redução de custo na infraestrutura AWS

O resultado do planejamento é um conjunto de projetos e tarefas que são distribuídos entre os engenheiros do time, respeitando capacidade e nível de senioridade.

---

## Tipos de trabalho trimestral

### Projetos estruturais da plataforma

São iniciativas maiores que evoluem a plataforma de forma significativa. Exemplos:

- Migração de workloads para uma nova versão do Kubernetes
- Reestruturação de clusters EKS para melhorar isolamento e segurança
- Implementação de um novo modelo de networking ou ingress
- Evolução do base module para suportar novos padrões de deploy

### Melhorias de infraestrutura

Trabalho contínuo de melhoria incremental na infraestrutura existente. Exemplos:

- Otimização de recursos nos clusters (right-sizing de nodes e pods)
- Revisão e atualização de políticas de IAM
- Melhoria na gestão de secrets e configurações sensíveis
- Atualização de versões de ferramentas e dependências críticas

### Iniciativas técnicas

Projetos com foco em automação, eficiência e qualidade de engenharia. Exemplos:

- Melhorias em pipelines de CI/CD para reduzir tempo de build e deploy
- Automações de tarefas operacionais repetitivas
- Melhorias na stack de observabilidade (alertas, dashboards, SLOs)
- Implementação de políticas de segurança via OPA/Kyverno
- Otimização de custos AWS com análise de uso e rightsizing

---

## Quando o engenheiro começa a contribuir nos Quarters

Durante os primeiros 90 dias, o foco é onboarding e aprendizado. A partir do terceiro mês, espera-se que o engenheiro comece a contribuir com tarefas do trimestre em andamento, mesmo que sejam tarefas menores ou de suporte a projetos maiores.

A partir do segundo trimestre completo na empresa, espera-se participação plena no planejamento e execução das iniciativas trimestrais.

---

## Exemplos de tarefas trimestrais por nível de complexidade

### Tarefas de entrada (bom para quem está começando a contribuir)

- Atualizar documentação de runbooks e processos
- Criar ou melhorar dashboards de observabilidade no Grafana
- Revisar e ajustar alertas existentes no Alertmanager
- Automatizar uma tarefa manual recorrente com script ou pipeline
- Contribuir com melhorias em um pipeline de CI/CD existente

### Tarefas de complexidade média

- Implementar uma nova feature no base module
- Criar um novo pipeline de deploy para um time de produto
- Configurar políticas de autoscaling para workloads específicos
- Implementar melhorias de segurança em clusters EKS
- Otimizar custos de uma conta AWS específica

### Tarefas de alta complexidade

- Liderar a migração de um cluster EKS para nova versão
- Projetar e implementar uma nova estratégia de networking
- Definir e implementar um novo padrão de observabilidade para a plataforma
- Liderar uma iniciativa de redução de custos cross-account

---

## Acompanhamento e visibilidade

Todo o trabalho trimestral é rastreado no Jira. Cada iniciativa tem épicos e tarefas associadas, com responsáveis definidos e critérios de conclusão claros.

O progresso é revisado nas cerimônias do time e em checkpoints trimestrais. Transparência sobre o andamento das tarefas é esperada de todos os engenheiros.

---

## Uma perspectiva importante

O trabalho operacional (tickets, deploys, suporte) nunca para. Os projetos trimestrais existem em paralelo com as responsabilidades do dia a dia. Saber equilibrar os dois é uma habilidade importante que se desenvolve com o tempo.

O time tem processos para proteger o tempo de foco necessário para projetos maiores. Mas é responsabilidade de cada engenheiro comunicar quando a carga operacional está impedindo o progresso nas iniciativas trimestrais.

---

## Referências

📄 [`devops-journey/devops-journey-map.md`](./devops-journey-map.md)
📄 [`devops-journey/poc-participation.md`](./poc-participation.md)
📄 [`operations/jira-workflows.md`](../operations/jira-workflows.md)
