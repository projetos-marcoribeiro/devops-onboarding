# Financial

> ⚠️ **Página em construção.** As informações específicas desta PU (repositórios, dashboards, pipelines, serviços) devem ser preenchidas pelo engenheiro responsável. Consulte o tech lead da unidade.

## 📑 Índice

- [Domínio](#domínio)
- [Particularidades desta PU](#particularidades-desta-pu)
- [Repositórios](#repositórios)
- [Pipelines](#pipelines)
- [Serviços financeiros críticos](#serviços-financeiros-críticos)
- [Contatos](#contatos)

---

A Product Unit **Financial** é responsável pelos sistemas financeiros da Hotmart: o coração da operação de negócio da empresa. Esta PU cuida de tudo relacionado a pagamentos, faturamento e integrações com o ecossistema financeiro.

---

## Domínio

O Financial gerencia os fluxos de dinheiro dentro da plataforma: desde o momento em que um comprador realiza um pagamento até a liquidação para o produtor. Isso envolve integrações com adquirentes, gateways de pagamento, sistemas antifraude, faturamento e conformidade regulatória.

**Exemplos de responsabilidades:**

- Processamento de pagamentos (cartão, boleto, PIX, carteiras digitais)
- Integrações com adquirentes e gateways de pagamento
- Faturamento e emissão de notas fiscais
- Liquidação e repasse para produtores
- Sistemas antifraude
- Conformidade com regulamentações financeiras (PCI-DSS, LGPD, etc.)

---

## Particularidades desta PU

Os serviços financeiros têm requisitos especialmente rigorosos de segurança, disponibilidade e conformidade. Ao trabalhar com infraestrutura desta PU, tenha em mente:

- **Isolamento:** a conta AWS `hotpay` é dedicada a workloads financeiros, com políticas de segurança mais restritivas
- **Disponibilidade:** qualquer degradação nos serviços de pagamento tem impacto direto na receita da empresa: incidentes aqui são sempre de alta prioridade
- **Auditoria:** mudanças em infraestrutura financeira podem exigir aprovações adicionais e devem ser documentadas com cuidado
- **Conformidade:** alguns serviços têm requisitos de PCI-DSS que impactam como a infraestrutura pode ser configurada

Antes de fazer qualquer mudança em serviços desta PU, confirme com o tech lead se há requisitos específicos a seguir.

---

## Repositórios

> Os repositórios desta PU devem ser listados aqui conforme necessário.

| Repositório | Descrição |
|---|---|
| - | - |

---

## Pipelines

> Workflows de CI/CD específicos desta PU.

| Pipeline | Repositório | Trigger |
|---|---|---|
| - | - | - |

---

## Serviços financeiros críticos

> Serviços críticos desta PU com runbooks associados. Incidentes nesses serviços têm impacto direto na receita.

| Serviço | Descrição | Runbook |
|---|---|---|
| - | - | - |

---

## Contatos

> Engenheiros de referência e canais de comunicação desta PU.

| Papel | Contato |
|---|---|
| Tech Lead | - |
| Canal no Google Chat | - |

---

> Esta página deve ser mantida atualizada pelos engenheiros responsáveis pela PU Financial. Se você identificar informação desatualizada ou ausente, atualize o documento ou reporte ao tech lead da unidade.
