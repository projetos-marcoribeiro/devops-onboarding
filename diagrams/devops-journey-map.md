# Diagrama: Jornada DevOps

Este diagrama representa a jornada de evolução de um engenheiro DevOps dentro da Hotmart, desde o primeiro dia até atingir maturidade plena na plataforma.

---

## Fluxo da Jornada

```mermaid
flowchart TD
    A(["Engenheiro DevOps"]) --> B

    B["Getting Started\nAcessos - Ferramentas - Setup"]

    B --> C["Platform Overview\nArquitetura - AWS - EKS - CI/CD - GitOps"]

    C --> D["Training Camp\nBase Module - Observabilidade - Secrets - Troubleshooting"]

    D --> E["Hands-On Project\nDeploy supervisionado - Frontend + Backend - Pipeline completo"]

    E --> F["Operations\nTickets - Deploys - Service Desk - Tarefas operacionais"]

    F --> G["Incident Management\nResposta a incidentes - Investigacao - Postmortem"]

    G --> H["On-call Readiness\nChecklist - Runbooks - Escalation policies - Plantao"]

    H --> I["Quarter Projects\nIniciativas trimestrais - Melhorias de plataforma - IaC"]

    I --> J["POCs e Evolucao\nNovas tecnologias - Avaliacoes - Contribuicao com a plataforma"]

    J --> K(["Engenheiro Maduro na Plataforma"])

    style A fill:#4A90D9,color:#fff,stroke:#2c6fad
    style K fill:#27AE60,color:#fff,stroke:#1e8449
    style B fill:#EBF5FB,stroke:#4A90D9
    style C fill:#EBF5FB,stroke:#4A90D9
    style D fill:#FEF9E7,stroke:#F39C12
    style E fill:#FEF9E7,stroke:#F39C12
    style F fill:#EAFAF1,stroke:#27AE60
    style G fill:#FDEDEC,stroke:#E74C3C
    style H fill:#FDEDEC,stroke:#E74C3C
    style I fill:#F4ECF7,stroke:#8E44AD
    style J fill:#F4ECF7,stroke:#8E44AD
```

---

## Descrição de cada etapa

| Etapa | Objetivo |
|---|---|
| Getting Started | Configurar acessos, instalar ferramentas e preparar o ambiente local para começar a trabalhar |
| Platform Overview | Entender a arquitetura da plataforma: contas AWS, clusters EKS, CI/CD, GitOps e observabilidade |
| Training Camp | Aprendizado guiado sobre Base Module, monitoramento, secrets e troubleshooting |
| Hands-On Project | Aplicar o conhecimento em um deploy real supervisionado com frontend e backend |
| Operations | Participar do trabalho operacional do dia a dia: tickets, deploys e suporte a times de produto |
| Incident Management | Responder a incidentes reais, investigar causa raiz e participar de postmortems |
| On-call Readiness | Completar o checklist de prontidão e entrar na rotação de plantão |
| Quarter Projects | Contribuir com iniciativas trimestrais de melhoria e evolução da plataforma |
| POCs e Evolução | Avaliar novas tecnologias e contribuir ativamente com a evolução técnica da plataforma |

---

## Referências

📄 [`devops-journey/devops-journey-map.md`](../devops-journey/devops-journey-map)
📄 [`devops-journey/30-60-90-days.md`](../devops-journey/30-60-90-days)
