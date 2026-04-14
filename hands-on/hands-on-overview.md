# Hands-On: Visão Geral

---

O Hands-On é a etapa prática do onboarding. Depois do Training Camp, é hora de fazer um deploy real utilizando os mesmos padrões, ferramentas e fluxos que o time usa no dia a dia.

🔗 Repositório: [https://github.com/Hotmart-Org/handson_devops](https://github.com/Hotmart-Org/handson_devops)

```text
Info
- Goal: deploy de uma aplicação em staging e produção
- Tempo estimado: 1-2 semanas
- Checkpoint: após semana 1 + após semana 2
```

---

## O que você vai fazer

Subir um frontend (S3 + CloudFront) e um backend (EKS) usando:

| Tecnologia | Papel |
|---|---|
| EKS | Cluster Kubernetes |
| Helm | Configuração do deploy |
| Base Module | Provisionamento de infra AWS |
| Docker | Containerização do backend |
| S3 + CloudFront | Hospedagem do frontend |
| GitHub Actions | Pipeline CI/CD |

---

## Por que o Hands-On existe

- Operar uma plataforma exige familiaridade prática, não só teórica
- Errar em ambiente controlado é melhor do que errar em produção
- Dúvidas reais só aparecem quando você tenta fazer algo funcionar
- O ciclo completo de deploy só faz sentido quando você conecta as peças na prática

---

## Documentos desta seção

📄 [`hands-on/hands-on-project.md`](hands-on-project): instruções passo a passo do projeto (5 partes)
📄 [`hands-on/deployment-workflow.md`](deployment-workflow): fluxo completo de deploy na plataforma
📄 [`hands-on/expected-results.md`](expected-results): checklist de entrega e o que demonstrar ao mentor
