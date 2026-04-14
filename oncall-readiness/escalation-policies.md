# Políticas de Escalação

## 📑 Índice

- [O que é uma escalation policy](#o-que-é-uma-escalation-policy)
- [Sequência de escalação](#sequência-de-escalação)
- [Canais de notificação](#canais-de-notificação)
- [Como o PagerDuty automatiza o processo](#como-o-pagerduty-automatiza-o-processo)
- [Responsabilidades em cada nível](#responsabilidades-em-cada-nível)
- [Quando escalar proativamente](#quando-escalar-proativamente)
- [Configurando suas notificações no PagerDuty](#configurando-suas-notificações-no-pagerduty)
- [Visualizando a escalation policy](#visualizando-a-escalation-policy)

---

A escalation policy define quem é notificado, em que ordem e em quanto tempo quando um incidente não recebe resposta. É o mecanismo que garante que nenhum incidente crítico fique sem atenção, independentemente do horário ou da disponibilidade do on-call primário.

---

## O que é uma escalation policy

No PagerDuty, uma escalation policy é uma sequência de níveis de notificação. Quando um alerta dispara, o PagerDuty começa pelo nível 1 e avança automaticamente para os próximos níveis se não houver resposta dentro do tempo configurado.

Isso garante que incidentes críticos sempre chegam a alguém: mesmo que o on-call primário esteja dormindo profundamente, sem bateria no celular ou em uma área sem sinal.

---

## Sequência de escalação

```
Alerta dispara no PagerDuty
         │
         ▼
Nível 1: On-call primário
         │  engenheiro de plantão no período atual
         │  notificado via push, SMS e ligação
         │
         │  sem acknowledge em 5 minutos
         ▼
Nível 2: Segundo engenheiro do time
         │  outro membro do time DevOps
         │  notificado via push e SMS
         │
         │  sem acknowledge em 10 minutos
         ▼
Nível 3: Liderança técnica
         │  tech lead ou engenheiro sênior de referência
         │  notificado via push, SMS e ligação
         │
         │  sem acknowledge em 15 minutos
         ▼
Nível 4: Equipe responsável pelo serviço
         │  time de produto ou área responsável pelo serviço afetado
         │  acionado em casos de impacto crítico prolongado
```

---

## Canais de notificação

O PagerDuty utiliza múltiplos canais para garantir que a notificação chegue, em ordem crescente de urgência:

### Push notification (app mobile)

Primeira tentativa. O app do PagerDuty envia uma notificação push para o celular do on-call. É silenciosa o suficiente para não acordar em alertas de baixa prioridade, mas configurável para soar alto em P1/P2.

**Configuração recomendada:** ative o modo de "override" de som para alertas críticos no app do PagerDuty, para que a notificação toque mesmo com o celular no silencioso.

### SMS

Se não houver resposta ao push em alguns minutos, o PagerDuty envia um SMS. Útil quando o app não está instalado ou quando há problema de conectividade.

### Ligação telefônica

O canal mais invasivo e mais confiável. O PagerDuty liga para o número cadastrado do on-call. Difícil ignorar: e é exatamente esse o objetivo para incidentes críticos.

### Escalação automática

Se nenhum dos canais acima resultar em acknowledge dentro do tempo configurado, o PagerDuty avança automaticamente para o próximo nível da escalation policy, sem necessidade de intervenção manual.

---

## Como o PagerDuty automatiza o processo

```
Alerta criado no PagerDuty
         │
         ▼
Nível 1 notificado (push + SMS + ligação)
         │
         ├── Acknowledge recebido → escalação para
         │                          investigação e resolução
         │
         └── Sem resposta em X min → Nível 2 notificado automaticamente
                   │
                   ├── Acknowledge recebido → investigação continua
                   │
                   └── Sem resposta em X min → Nível 3 notificado
                             │
                             └── ... e assim por diante
```

O tempo entre cada nível é configurado pelo time e pode variar conforme a severidade do incidente. Incidentes P1 têm tempos de escalação mais curtos do que P3.

---

## Responsabilidades em cada nível

### On-call primário (Nível 1)

- Fazer acknowledge imediatamente ao receber a notificação
- Iniciar a investigação seguindo o guia de troubleshooting
- Comunicar o status no canal de incidentes
- Escalar proativamente se não conseguir resolver em tempo razoável

### Segundo engenheiro (Nível 2)

- Entrar na investigação se o nível 1 não respondeu ou pediu ajuda
- Trazer um segundo ponto de vista para a investigação
- Assumir a liderança da resposta se o nível 1 estiver indisponível

### Liderança técnica (Nível 3)

- Coordenar a resposta em incidentes de alta complexidade
- Tomar decisões de alto impacto (rollback de múltiplos serviços, comunicação com stakeholders)
- Acionar recursos adicionais se necessário

### Equipe responsável pelo serviço (Nível 4)

- Acionada quando o problema está no código da aplicação e o time DevOps precisa de suporte do time de produto
- Pode ser acionada proativamente pelo on-call antes de chegar ao nível 4 automático, se a causa raiz for claramente de aplicação

---

## Quando escalar proativamente

A escalação automática é uma rede de segurança: mas você não precisa esperar ela acontecer. Escale proativamente quando:

- Você investigou por 15-20 minutos sem identificar a causa raiz
- O impacto é P1 e você precisa de mais pares de olhos
- A causa raiz está em uma área fora do seu domínio de conhecimento
- Você precisa executar uma ação de alto risco e quer validação
- O incidente está se prolongando e o impacto está aumentando

**Escalar não é admitir derrota.** É reconhecer que dois engenheiros investigando juntos são mais eficientes do que um investigando sozinho, especialmente sob pressão.

---

## Configurando suas notificações no PagerDuty

Para garantir que você vai receber as notificações de on-call:

1. Instale o app PagerDuty no celular
2. Configure seu número de telefone no perfil
3. Ative as notificações de alta prioridade no app
4. Configure o override de som para alertas críticos (toca mesmo no silencioso)
5. Teste as notificações antes de entrar na rotação

Verifique também se o seu número está correto no PagerDuty: um número desatualizado significa que a ligação de escalação não vai chegar.

---

## Visualizando a escalation policy

Para ver a escalation policy configurada para o time DevOps:

```
PagerDuty → Configurations → Escalation Policies → DevOps
```

Familiarize-se com a política antes de entrar na rotação. Saber quem está no nível 2 e 3 é importante para saber quem acionar quando precisar de ajuda.
