# Participação em POCs

## 📑 Índice

- [Por que POCs existem](#por-que-pocs-existem)
- [Como uma POC funciona na prática](#como-uma-poc-funciona-na-prática)
- [Exemplos de POCs no contexto DevOps](#exemplos-de-pocs-no-contexto-devops)
- [Por que participar de POCs é importante para o engenheiro](#por-que-participar-de-pocs-é-importante-para-o-engenheiro)
- [Quando o engenheiro começa a participar de POCs](#quando-o-engenheiro-começa-a-participar-de-pocs)
- [Documentação de POCs](#documentação-de-pocs)
- [Referências](#referências)

---

POC é a sigla para Proof of Concept, ou Prova de Conceito. No contexto do time DevOps da Hotmart, POCs são experimentos técnicos estruturados usados para avaliar novas tecnologias, validar abordagens e testar ferramentas antes de qualquer decisão de adoção oficial.

Participar de POCs é uma das formas mais diretas de contribuir com a evolução da plataforma.

---

## Por que POCs existem

A plataforma DevOps da Hotmart está em constante evolução. Novas ferramentas surgem, padrões mudam, e o que funcionava bem há dois anos pode não ser mais a melhor opção hoje. POCs são o mecanismo que o time usa para tomar decisões técnicas com base em evidências, não em suposições ou hype.

Uma POC bem conduzida responde perguntas concretas:

- Essa ferramenta resolve o problema que temos?
- Qual é o custo operacional de adotá-la?
- Ela se integra bem com o que já temos?
- Quais são os riscos e limitações?
- Vale a pena investir na adoção?

---

## Como uma POC funciona na prática

Uma POC geralmente segue esse fluxo:

```
Identificação       Definição          Execução          Avaliação         Decisão
do problema    →    do escopo     →    do experimento →  dos resultados →  (adotar / descartar
ou oportunidade     e critérios        em ambiente                          / investigar mais)
                    de sucesso         isolado
```

1. **Identificação**: alguém no time identifica um problema, uma limitação ou uma oportunidade de melhoria
2. **Definição**: o escopo da POC é definido com clareza — o que será testado, como será avaliado e qual é o critério de sucesso
3. **Execução**: o experimento é conduzido em ambiente isolado (geralmente dev ou sandbox), sem impacto em produção
4. **Avaliação**: os resultados são documentados e compartilhados com o time
5. **Decisão**: o time decide se avança com a adoção, descarta a ideia ou aprofunda a investigação

---

## Exemplos de POCs no contexto DevOps

### Observabilidade

- Avaliar uma nova ferramenta de APM (Application Performance Monitoring)
- Testar uma solução de distributed tracing alternativa ao que está em uso
- Explorar novas formas de correlacionar logs, métricas e traces
- Avaliar ferramentas de análise de custo de infraestrutura em tempo real

### Deploy e CI/CD

- Testar uma nova estratégia de deploy (blue/green, canary, feature flags)
- Avaliar uma ferramenta de pipeline alternativa
- Explorar formas de reduzir o tempo de build em pipelines existentes
- Testar GitOps com uma abordagem diferente da atual

### Infraestrutura e Kubernetes

- Avaliar um novo operador Kubernetes para gerenciar um tipo de workload
- Testar políticas de segurança com OPA ou Kyverno
- Explorar soluções de service mesh
- Avaliar ferramentas de autoscaling mais sofisticadas (KEDA, por exemplo)

### IA para operações (AIOps)

- Testar ferramentas de detecção de anomalias em métricas
- Avaliar assistentes de IA para troubleshooting de incidentes
- Explorar automação inteligente de runbooks
- Testar geração automática de postmortems com base em dados de incidentes

### Otimização de infraestrutura

- Avaliar estratégias de Spot Instances para redução de custo
- Testar ferramentas de rightsizing automático de recursos
- Explorar soluções de FinOps para visibilidade de custo por time ou produto

---

## Por que participar de POCs é importante para o engenheiro

Além de contribuir diretamente com a evolução da plataforma, participar de POCs traz benefícios concretos para o desenvolvimento do engenheiro:

**Aprendizado acelerado**: POCs expõem o engenheiro a tecnologias novas em um contexto prático e com propósito claro. É uma das formas mais eficientes de aprender.

**Visibilidade técnica**: conduzir ou co-liderar uma POC dá visibilidade ao trabalho do engenheiro dentro do time e da organização.

**Influência na plataforma**: as decisões que saem de uma POC moldam a plataforma pelos próximos meses ou anos. Participar significa ter voz nessas decisões.

**Desenvolvimento de pensamento crítico**: avaliar uma tecnologia de forma estruturada — identificando prós, contras, riscos e limitações — é uma habilidade valiosa que vai além de qualquer ferramenta específica.

---

## Quando o engenheiro começa a participar de POCs

Não existe um momento fixo. Engenheiros mais novos no time geralmente começam participando de POCs como suporte — ajudando na execução, documentando resultados, testando cenários específicos.

Com o tempo, à medida que o engenheiro ganha maturidade na plataforma, passa a propor e liderar suas próprias POCs.

O importante é não esperar estar "pronto" para participar. Curiosidade técnica e disposição para aprender são os únicos pré-requisitos reais.

---

## Documentação de POCs

Toda POC deve ser documentada. Isso inclui:

- O problema ou oportunidade que motivou a POC
- O escopo e os critérios de sucesso definidos
- O que foi testado e como
- Os resultados obtidos
- A decisão tomada e a justificativa

Essa documentação é valiosa não só para a decisão imediata, mas como referência futura. Muitas vezes, uma tecnologia descartada hoje pode ser reavaliada no futuro com um contexto diferente.

---

## Referências

📄 [`devops-journey/quarterly-expectations.md`](quarterly-expectations)
📄 [`devops-journey/devops-journey-map.md`](devops-journey-map)
