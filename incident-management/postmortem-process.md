# Processo de Postmortem

## 📑 Índice

- [Quando fazer um postmortem](#quando-fazer-um-postmortem)
- [Cultura blameless](#cultura-blameless)
- [Objetivos do postmortem](#objetivos-do-postmortem)
- [O que é analisado](#o-que-é-analisado)
- [Estrutura do documento de postmortem](#estrutura-do-documento-de-postmortem)
- [Onde registrar o postmortem](#onde-registrar-o-postmortem)
- [Ações de melhoria](#ações-de-melhoria)
- [Postmortem como ferramenta de aprendizado](#postmortem-como-ferramenta-de-aprendizado)
- [Referências](#referências)

---

O postmortem é a análise conduzida após incidentes importantes para entender o que aconteceu, por que aconteceu e como evitar que aconteça novamente. É uma das práticas mais valiosas de uma cultura de engenharia madura.

---

## Quando fazer um postmortem

Nem todo incidente exige um postmortem formal. A regra geral na Hotmart:

| Severidade | Postmortem |
|---|---|
| P1 | Obrigatório |
| P2 | Obrigatório |
| P3 | Recomendado quando há aprendizado relevante |
| P4 | Opcional, a critério do time |

Além da severidade, considere fazer postmortem quando:
- O incidente durou mais de 30 minutos
- O mesmo tipo de problema ocorreu mais de uma vez
- A causa raiz revelou um gap sistêmico na plataforma
- O processo de resposta teve falhas que precisam ser corrigidas

---

## Cultura blameless

O postmortem da Hotmart segue a cultura **blameless** (sem culpa), inspirada nas práticas de SRE do Google.

O objetivo do postmortem não é encontrar quem errou: é entender como o sistema (processos, ferramentas, infraestrutura, comunicação) permitiu que o problema acontecesse e como melhorá-lo.

**Na prática, isso significa:**
- Nenhuma pessoa é culpada ou punida por um incidente
- O foco é em fatores sistêmicos, não em erros individuais
- Qualquer engenheiro pode ter cometido o mesmo erro nas mesmas circunstâncias
- A pergunta não é "quem fez isso?" mas "por que o sistema permitiu que isso acontecesse?"

Isso não significa que erros não têm consequências: significa que as consequências são melhorias no sistema, não punição de pessoas. Uma cultura blameless encoraja transparência e aprendizado honesto.

---

## Objetivos do postmortem

1. **Entender a causa raiz**: não apenas o sintoma imediato, mas a cadeia de causas que levou ao incidente
2. **Identificar melhorias**: ações concretas que reduzem a probabilidade ou o impacto de incidentes similares
3. **Evitar recorrência**: transformar o aprendizado em mudanças reais na plataforma, nos processos ou nas ferramentas
4. **Compartilhar conhecimento**: documentar o incidente para que todo o time aprenda, não apenas quem estava de plantão

---

## O que é analisado

### Timeline do incidente

Reconstrução cronológica de tudo que aconteceu: quando o problema começou, quando foi detectado, quando o on-call foi notificado, cada ação tomada e quando o serviço foi restaurado.

A timeline revela gaps importantes: quanto tempo levou para detectar? Quanto tempo para responder? Quanto tempo para resolver? Onde o processo travou?

### Mudanças recentes na infraestrutura

Deploys, mudanças de configuração, atualizações de dependências, mudanças de infraestrutura nas horas ou dias anteriores ao incidente. A maioria dos incidentes em produção está relacionada a uma mudança recente.

### Logs e métricas

Evidências coletadas durante a investigação: logs de erro, métricas que saíram do normal, traces de performance. Esses dados confirmam a causa raiz e ajudam a entender o impacto real.

### Ações tomadas durante o incidente

O que foi feito para resolver o problema, em que ordem e com qual resultado. Isso inclui tanto as ações que funcionaram quanto as que não funcionaram: ambas têm valor de aprendizado.

---

## Estrutura do documento de postmortem

```markdown
## Postmortem: [Nome do Incidente]: [Data]

### Resumo
Descrição breve do incidente, impacto e duração.

### Impacto
- Duração: X minutos
- Serviços afetados: [lista]
- Usuários afetados: [estimativa]
- Impacto financeiro: [se aplicável]

### Timeline
| Horário | Evento |
|---------|--------|
| 14:32   | Alerta disparado no PagerDuty |
| 14:35   | On-call fez acknowledge |
| 14:40   | Investigação iniciada |
| 14:55   | Causa raiz identificada |
| 15:02   | Rollback executado |
| 15:08   | Serviço restaurado |

### Causa Raiz
Descrição técnica da causa raiz identificada.

### Fatores Contribuintes
Condições que tornaram o incidente possível ou pior.

### O que foi bem
Aspectos do processo de resposta que funcionaram bem.

### O que pode melhorar
Aspectos do processo que poderiam ter sido melhores.

### Ações de melhoria
| Ação | Responsável | Prazo |
|------|-------------|-------|
| Adicionar alerta para X | [nome] | [data] |
| Criar runbook para Y | [nome] | [data] |
| Corrigir configuração Z | [nome] | [data] |
```

---

## Onde registrar o postmortem

O postmortem é registrado em um ticket Jira vinculado ao incidente original, ou em um documento interno no Confluence/Notion, dependendo do processo atual do time.

Consulte o tech lead para saber onde os postmortems recentes estão documentados: ler postmortems anteriores é uma das melhores formas de aprender sobre a plataforma e sobre como o time responde a incidentes.

---

## Ações de melhoria

A parte mais importante do postmortem são as ações de melhoria. Um postmortem sem ações concretas é apenas documentação: não gera mudança.

Cada ação deve ter:
- **O que:** descrição clara do que precisa ser feito
- **Por que:** qual problema essa ação resolve
- **Responsável:** quem vai executar
- **Prazo:** quando deve estar concluído

As ações são criadas como tickets no Jira e acompanhadas nas cerimônias do time. O time deve revisar periodicamente se as ações de postmortems anteriores foram concluídas.

---

## Postmortem como ferramenta de aprendizado

Para engenheiros novos no time, ler postmortems anteriores é altamente recomendado. Eles revelam:

- Tipos de problemas que já aconteceram na plataforma
- Como o time investiga e resolve incidentes na prática
- Melhorias que já foram implementadas e por quê
- Gaps que ainda existem e estão sendo trabalhados

Pergunte ao mentor onde encontrar os postmortems do time e reserve um tempo para ler os mais recentes durante o onboarding.

---

## Referências

📄 [`incident-management/incident-process.md`](incident-process)
📄 [`incident-management/troubleshooting-flow.md`](troubleshooting-flow)
📄 [`oncall-readiness/incident-investigation-guide.md`](../oncall-readiness/incident-investigation-guide)
