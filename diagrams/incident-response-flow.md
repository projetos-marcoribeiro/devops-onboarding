# Diagrama — Fluxo de Resposta a Incidentes

## 📑 Índice

- [Fluxo Principal](#fluxo-principal)
- [Ferramentas por etapa](#ferramentas-por-etapa)
- [Descrição de cada etapa](#descrição-de-cada-etapa)
- [Escalation Policy](#escalation-policy)
- [Referências](#referências)

---

Este diagrama representa o processo completo de resposta a incidentes na Hotmart, desde a detecção automática pelo monitoramento até o encerramento e postmortem.

---

## Fluxo Principal

```mermaid
flowchart TD
    A(["⚠️ Produção degrada"]) --> B

    subgraph MON ["Monitoramento"]
        B["Ferramentas detectam o problema"]
        B1["🔵 Pingdom\nUptime externo"]
        B2["🟢 NewRelic\nAPM · Taxa de erro · Latência"]
        B3["🟣 Datadog\nInfraestrutura · Containers"]
        B4["🟠 CloudWatch\nMétricas AWS"]
        B --> B1 & B2 & B3 & B4
    end

    B1 & B2 & B3 & B4 -->|"alerta roteado"| C

    C["📟 PagerDuty\nCria incidente\nNotifica on-call"]

    C -->|"push · SMS · ligação"| D

    D(["👤 On-call recebe alerta\nFaz acknowledge"])

    D --> E["🔍 Triagem\nConfirmar o problema\nAvaliar impacto e severidade\nP1 · P2 · P3 · P4"]

    E --> F["🧪 Investigação"]

    subgraph INV ["Ferramentas de Investigação"]
        F1["NewRelic\nTraces · Logs · APM"]
        F2["Datadog\nMétricas de infra"]
        F3["CloudWatch\nLogs AWS"]
        F4["kubectl\nPods · Logs · Eventos"]
        F5["ArgoCD\nDeploy recente?"]
    end

    F --> F1 & F2 & F3 & F4 & F5

    F1 & F2 & F3 & F4 & F5 --> G["🎯 Causa raiz identificada"]

    G --> H{"Tipo de ação\ncorretiva"}

    H -->|"deploy recente"| I1["⏪ Rollback\nargocd app rollback\nkubectl rollout undo"]
    H -->|"capacidade"| I2["📈 Scaling\nkubectl scale\nAumento de réplicas"]
    H -->|"configuração"| I3["🔧 Correção de config\nAtualiza values\nDeploy via ArgoCD"]
    H -->|"bug simples"| I4["⏩ Fix Forward\nCorrige código\nNovo deploy"]

    I1 & I2 & I3 & I4 --> J(["✅ Serviço restaurado\nMétricas normalizadas"])

    J --> K["📢 Comunicação de resolução\nCanal de incidentes\nStakeholders notificados"]

    K --> L{"Severidade\nP1 ou P2?"}

    L -->|"sim"| M["📝 Postmortem\nTimeline · Causa raiz\nAções de melhoria\nCultura blameless"]
    L -->|"não"| N["📋 Ticket encerrado\nHistórico documentado"]

    M --> O(["🔄 Melhoria contínua\nAções implementadas\nRecorrência evitada"])
    N --> O

    style A fill:#E74C3C,color:#fff,stroke:#c0392b
    style C fill:#06AC38,color:#fff,stroke:#058a2d
    style D fill:#4A90D9,color:#fff,stroke:#2c6fad
    style E fill:#F39C12,color:#fff,stroke:#d68910
    style F fill:#8E44AD,color:#fff,stroke:#6c3483
    style G fill:#2C3E50,color:#fff,stroke:#1a252f
    style H fill:#F39C12,color:#fff,stroke:#d68910
    style I1 fill:#E74C3C,color:#fff,stroke:#c0392b
    style I2 fill:#3498DB,color:#fff,stroke:#2980b9
    style I3 fill:#E67E22,color:#fff,stroke:#ca6f1e
    style I4 fill:#27AE60,color:#fff,stroke:#1e8449
    style J fill:#27AE60,color:#fff,stroke:#1e8449
    style K fill:#2C3E50,color:#fff,stroke:#1a252f
    style M fill:#8E44AD,color:#fff,stroke:#6c3483
    style O fill:#27AE60,color:#fff,stroke:#1e8449
```

---

## Ferramentas por etapa

| Etapa | Ferramentas utilizadas |
|---|---|
| Detecção | Pingdom, NewRelic, Datadog, CloudWatch, Statping |
| Notificação | PagerDuty (push, SMS, ligação) |
| Investigação — Aplicação | NewRelic (APM, traces, logs) |
| Investigação — Infraestrutura | Datadog, CloudWatch, kubectl |
| Investigação — Deploy | ArgoCD, GitHub Actions |
| Ação corretiva | ArgoCD, kubectl, GitHub Actions |
| Documentação | Jira, Google Chat |

---

## Descrição de cada etapa

| Etapa | Objetivo |
|---|---|
| Monitoramento detecta | Ferramentas identificam automaticamente condições anormais: queda de uptime, aumento de erro, latência elevada, recursos esgotados |
| PagerDuty cria incidente | Consolida os alertas e notifica o engenheiro de plantão via push, SMS ou ligação conforme a escalation policy |
| On-call faz acknowledge | Sinaliza que alguém está ciente do problema e para a escalação automática |
| Triagem | Confirma se o problema é real, avalia o impacto e define a severidade (P1-P4) |
| Investigação | Coleta evidências usando as ferramentas de observabilidade para identificar a causa raiz |
| Ação corretiva | Executa a solução mais adequada: rollback, scaling, correção de configuração ou fix forward |
| Serviço restaurado | Confirma que as métricas voltaram ao normal e o serviço está operando corretamente |
| Comunicação | Informa stakeholders e o time sobre a resolução com resumo do ocorrido |
| Postmortem | Para P1/P2: análise blameless de causa raiz com ações concretas de melhoria para evitar recorrência |

---

## Escalation Policy

```mermaid
flowchart LR
    A["Alerta dispara"] -->|"5 min sem resposta"| B
    B["Nível 1\nOn-call primário"] -->|"10 min sem resposta"| C
    C["Nível 2\nSegundo engenheiro"] -->|"15 min sem resposta"| D
    D["Nível 3\nLiderança técnica"] -->|"sem resposta"| E
    E["Nível 4\nEquipe responsável\npelo serviço"]

    style A fill:#E74C3C,color:#fff,stroke:#c0392b
    style B fill:#4A90D9,color:#fff,stroke:#2c6fad
    style C fill:#F39C12,color:#fff,stroke:#d68910
    style D fill:#8E44AD,color:#fff,stroke:#6c3483
    style E fill:#2C3E50,color:#fff,stroke:#1a252f
```

---

## Referências

📄 [`incident-management/incident-process.md`](#/incident-management/incident-process)
📄 [`incident-management/troubleshooting-flow.md`](#/incident-management/troubleshooting-flow)
📄 [`incident-management/postmortem-process.md`](#/incident-management/postmortem-process)
📄 [`oncall-readiness/escalation-policies.md`](#/oncall-readiness/escalation-policies)
