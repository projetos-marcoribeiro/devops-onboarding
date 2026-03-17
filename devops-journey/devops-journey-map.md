# Jornada DevOps na Hotmart

## 📑 Índice

- [Visão Geral da Jornada](#visão-geral-da-jornada)
- [Etapas da Jornada](#etapas-da-jornada)
- [Considerações Finais](#considerações-finais)
- [Referências](#referências)

---

Este documento descreve a jornada de evolução de um engenheiro DevOps dentro da Hotmart, desde o primeiro dia até atingir maturidade plena na plataforma.

A jornada não é linear no sentido de que cada etapa tem um tempo fixo, mas sim progressiva: cada fase constrói a base para a próxima. O objetivo é que o engenheiro desenvolva autonomia gradual, entendendo a plataforma, operando com segurança e eventualmente contribuindo com sua evolução.

---

## Visão Geral da Jornada

```
Engenheiro DevOps
       │
       ▼
┌─────────────────┐
│   1. Setup e    │  → Acessos, ferramentas, ambiente local
│     Acessos     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  2. Entendimento│  → Arquitetura, AWS, EKS, CI/CD, GitOps
│  da Plataforma  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  3. Training    │  → Estudo guiado, skill matrix, labs
│     Camp        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  4. Hands-on    │  → Projeto prático supervisionado
│    Project      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  5. Trabalho    │  → Tickets, deploys, tarefas operacionais
│  Operacional    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  6. Resposta a  │  → Participação em incidentes reais
│   Incidentes    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  7. Preparação  │  → Checklist, runbooks, escalation policies
│   para On-call  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  8. Projetos    │  → Contribuição em iniciativas do trimestre
│  Trimestrais    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  9. POCs e      │  → Avaliação de novas tecnologias e evolução
│  Evolução       │    da plataforma
└─────────────────┘
```

---

## Etapas da Jornada

### 1. Setup e Acessos

O ponto de partida. Antes de qualquer coisa, o engenheiro precisa ter acesso às ferramentas e sistemas da empresa.

Isso inclui configurar o Okta, solicitar acessos via Service Desk, instalar as ferramentas obrigatórias (kubectl, AWS CLI, hotctl, Rancher Desktop, etc.) e garantir que o ambiente local está funcional.

**Objetivo:** estar operacional para começar a explorar a plataforma.

---

### 2. Entendimento da Plataforma

Com o ambiente configurado, o foco passa a ser entender como a plataforma Hotmart funciona: a arquitetura AWS, os clusters EKS, os pipelines de CI/CD, o modelo GitOps com ArgoCD, a stack de observabilidade e os módulos base.

Essa etapa é essencialmente de leitura, exploração e conversas com o time.

**Objetivo:** ter uma visão clara de como os sistemas se conectam e como o trabalho DevOps se encaixa nesse contexto.

---

### 3. Training Camp

Período estruturado de aprendizado guiado. O engenheiro segue um learning path definido, preenche a skill matrix e realiza exercícios práticos em ambientes de desenvolvimento.

O Training Camp cobre os principais pilares: Kubernetes, AWS, CI/CD, observabilidade, segurança e boas práticas de infraestrutura como código.

**Objetivo:** nivelar o conhecimento técnico necessário para operar com segurança na plataforma.

---

### 4. Hands-on Project

Projeto prático supervisionado onde o engenheiro aplica o que aprendeu em um cenário real ou simulado. O projeto envolve deploy de uma aplicação, configuração de pipeline, observabilidade e troubleshooting.

É a ponte entre o aprendizado teórico e o trabalho real.

**Objetivo:** validar na prática o entendimento da plataforma e ganhar confiança para operar de forma autônoma.

---

### 5. Trabalho Operacional

O engenheiro começa a participar do trabalho do dia a dia: atendimento de tickets no Service Desk, execução de deploys, tarefas de manutenção, suporte a times de produto e execução de runbooks.

Nessa fase, o engenheiro ainda conta com suporte do time, mas começa a ganhar autonomia progressiva.

**Objetivo:** desenvolver fluência operacional na plataforma.

---

### 6. Resposta a Incidentes

O engenheiro começa a participar de incidentes reais, inicialmente como observador e depois como participante ativo. Aprende a usar as ferramentas de monitoramento, entender alertas, investigar causas raiz e colaborar na resolução.

**Objetivo:** desenvolver a capacidade de responder a situações de degradação ou falha com calma e método.

---

### 7. Preparação para On-call

Antes de entrar na rotação de on-call, o engenheiro passa por um processo de preparação: revisão do checklist de prontidão, estudo dos runbooks, entendimento das políticas de escalação e simulações de incidentes.

**Objetivo:** garantir que o engenheiro está pronto para responder a incidentes de forma autônoma fora do horário comercial.

---

### 8. Projetos Trimestrais

Com maturidade operacional estabelecida, o engenheiro passa a contribuir em projetos estruturais planejados para o trimestre: melhorias de infraestrutura, automações, otimizações de custo, evoluções na plataforma Kubernetes, melhorias em CI/CD, entre outros.

**Objetivo:** contribuir ativamente com a evolução técnica da plataforma.

---

### 9. POCs e Evolução da Plataforma

O estágio mais avançado da jornada. O engenheiro participa de Proofs of Concept para avaliar novas tecnologias, propor melhorias e ajudar a definir o futuro da plataforma.

**Objetivo:** ser um agente ativo na evolução técnica do time e da empresa.

---

## Considerações Finais

A jornada DevOps na Hotmart é desenhada para ser progressiva e segura. Ninguém é jogado no fundo do poço. Cada etapa tem suporte, documentação e mentoria disponíveis.

O ritmo de progressão varia por pessoa, mas o caminho é o mesmo para todos. O importante é avançar com entendimento real, não apenas cumprir etapas.

---

## Referências

📄 [`devops-journey/30-60-90-days.md`](30-60-90-days)
📄 [`devops-journey/quarterly-expectations.md`](quarterly-expectations)
📄 [`devops-journey/poc-participation.md`](poc-participation)
📄 [`getting-started/introduction.md`](../getting-started/introduction)
