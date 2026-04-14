# Smart Contract

## 📑 Índice

- [O que é o Smart Contract](#o-que-é-o-smart-contract)
- [O que o Smart Contract cobre](#o-que-o-smart-contract-cobre)
- [Por que todos devem conhecer o Smart Contract](#por-que-todos-devem-conhecer-o-smart-contract)
- [Como o Smart Contract evolui](#como-o-smart-contract-evolui)

---

O Smart Contract é o documento que define as boas práticas, diretrizes e acordos que guiam o trabalho diário do time DevOps da Hotmart. É uma referência viva que todos os membros do time devem conhecer e seguir.

🔗 [Smart Contract: Documento Oficial](https://docs.google.com/presentation/d/1SdSBnz-yxGYtGLgD18UdjWT4Pr2WCFMPlwbsW4m8p8I/edit)

---

## O que é o Smart Contract

O nome "Smart Contract" é uma analogia aos contratos inteligentes da blockchain: um conjunto de regras acordadas coletivamente que se aplicam automaticamente, sem precisar de supervisão constante. No contexto do time DevOps, é o conjunto de acordos sobre como o time trabalha, se comunica e toma decisões.

Não é um documento burocrático: é um guia prático que reflete a cultura e os padrões do time. Ele evolui conforme o time aprende e melhora seus processos.

---

## O que o Smart Contract cobre

### Padrões de deploy

Define como deploys devem ser feitos na plataforma: uso obrigatório do fluxo GitOps, proibição de `kubectl apply` direto em produção, processo de aprovação para deploys em horários críticos, uso de feature flags para mudanças de alto risco, etc.

Exemplos de diretrizes:
- Todo deploy em produção passa pelo ArgoCD
- Deploys em produção durante horários de pico exigem aprovação do tech lead
- Rollback deve ser executado em menos de 5 minutos se o deploy causar degradação

### Boas práticas de infraestrutura

Padrões técnicos que o time segue para garantir consistência, segurança e manutenibilidade da infraestrutura.

Exemplos de diretrizes:
- Toda infraestrutura é provisionada via Base Module ou Terraform: sem recursos criados manualmente no console AWS
- Secrets nunca são hardcoded em código ou configuração: sempre via Vault ou Secrets Manager
- Todo recurso AWS deve ter tags de identificação (time, produto, ambiente, custo)
- Requests e limits de CPU/memória são obrigatórios em todos os pods

### Processos de troubleshooting

Como o time aborda a investigação de problemas: metodologia de causa raiz, ferramentas utilizadas em cada etapa, quando escalar, como comunicar durante um incidente.

Exemplos de diretrizes:
- Antes de qualquer ação corretiva, documente o sintoma observado
- Use dados (métricas, logs, traces) para confirmar hipóteses: não aja por suposição
- Comunique o status do incidente no canal apropriado a cada 15 minutos enquanto estiver ativo
- Toda ação tomada durante um incidente deve ser registrada no ticket

### Responsabilidades do time DevOps

Define claramente o que é responsabilidade do time DevOps e o que é responsabilidade dos times de produto. Isso evita ambiguidade e conflitos sobre quem deve resolver o quê.

Exemplos de responsabilidades do DevOps:
- Manutenção e evolução da plataforma de infraestrutura
- Operação dos clusters EKS e ferramentas de plataforma
- Resposta a incidentes de infraestrutura
- Suporte a times de produto em questões de infraestrutura

Exemplos de responsabilidades dos times de produto:
- Código da aplicação e seus bugs
- Configuração de variáveis de ambiente da aplicação
- Definição de recursos necessários (CPU, memória, réplicas)

### Comunicação entre equipes

Como o time DevOps se comunica internamente e com outros times: canais de comunicação, SLAs de resposta, como escalar problemas, como solicitar suporte.

Exemplos de diretrizes:
- Solicitações ao time DevOps passam pelo Service Desk: não por mensagem direta
- Incidentes críticos são comunicados no canal de incidentes, não em DM
- Decisões técnicas relevantes são documentadas em ADRs (Architecture Decision Records)
- Reuniões são usadas para discussões que exigem colaboração: não para passar informação que poderia ser assíncrona

---

## Por que todos devem conhecer o Smart Contract

O Smart Contract só funciona se todos o conhecem e seguem. Um time onde metade das pessoas segue os acordos e a outra metade não é um time com processos inconsistentes: e processos inconsistentes geram incidentes, retrabalho e frustração.

Para novos engenheiros, conhecer o Smart Contract desde o início é especialmente importante porque:

- Evita que você aprenda práticas erradas que depois precisam ser corrigidas
- Dá contexto para entender por que o time faz as coisas de determinada forma
- Facilita a integração com o time, pois você já fala a mesma língua operacional

---

## Como o Smart Contract evolui

O Smart Contract não é imutável. Quando o time identifica que uma diretriz não está funcionando, que existe um gap não coberto ou que uma prática melhor foi descoberta, o documento é atualizado.

Qualquer membro do time pode propor mudanças. O processo é simples: propor a mudança no canal do time, discutir, chegar a um consenso e atualizar o documento.

Se durante o onboarding você identificar algo que parece inconsistente com o que está no Smart Contract, ou uma situação que o documento não cobre, traga para o mentor. Pode ser uma oportunidade de melhoria.
