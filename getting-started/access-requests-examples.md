# Solicitações de Acesso — Pack Completo

## 📑 Índice

- [Como funciona](#como-funciona)
- [Pack de acessos obrigatórios](#pack-de-acessos-obrigatórios)
- [Acessos adicionais por organização](#acessos-adicionais-por-organização)
- [Modelo de justificativa](#modelo-de-justificativa)
- [Checklist de acessos](#checklist-de-acessos)
- [Dicas para não travar](#dicas-para-não-travar)

---

Este documento contém o pack completo de acessos que um engenheiro DevOps precisa solicitar no primeiro dia. Todos os tickets são abertos no [Service Desk — Portal 4](https://hotmart.atlassian.net/servicedesk/customer/portal/4) usando o formulário **"Concessão de acesso a sistema"**.

Abra todos de uma vez. Não espere um ser aprovado para abrir o próximo — isso vai te bloquear por dias.

---

## Como funciona

1. Acesse o [Service Desk](https://hotmart.atlassian.net/servicedesk/customer/portal/4)
2. Selecione **"Concessão de acesso a sistema / banco de dados"**
3. Preencha os campos conforme os exemplos abaixo
4. Na justificativa, sempre mencione o fellow para espelhamento

---

## Pack de acessos obrigatórios

Abra um ticket para cada item abaixo. Use a justificativa modelo e adapte o sistema/perfil.

---

### 1. AWS — Amazon Q Developer

| Campo | Valor |
|---|---|
| Sistema | AWS |
| Instância | Hotmart |
| Perfil | Amazon Q Developer |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso do acesso ao Amazon Q Developer para
utilizar o assistente de IA durante o desenvolvimento e operação de
infraestrutura na AWS.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 2. AWS — Contas principais (Hotmart-Org, Usefedora, Notas-Org)

| Campo | Valor |
|---|---|
| Sistema | AWS |
| Instância | hotmart-org, usefedora, notas-org |
| Perfil | Devops-ReadOnly (inicial) |
| Justificativa | Ver modelo abaixo |

> Comece com **Devops-ReadOnly**. O perfil **AdministratorAccess** será solicitado depois, quando necessário, por conta inteira.

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso às contas AWS hotmart-org,
usefedora e notas-org com perfil Devops-ReadOnly para acompanhar a
infraestrutura, clusters EKS e dar suporte operacional.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 3. GitHub — Organizações

| Campo | Valor |
|---|---|
| Sistema | GitHub |
| Organizações | Hotmart-Org, usefedora, notas-org |
| Time | devops |
| Justificativa | Ver modelo abaixo |

> Você precisa criar um usuário GitHub corporativo seguindo o padrão: `<email sem ponto>-hotmart` (ex: `marcoribeiro-hotmart`)

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso às organizações Hotmart-Org,
usefedora e notas-org no GitHub para contribuir com repositórios de
infraestrutura, pipelines e documentação. Time: devops.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 4. JIRA — DevOps Components

| Campo | Valor |
|---|---|
| Sistema | JIRA |
| Grupo | DevOps - Components |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso ao JIRA (grupo DevOps -
Components) para acompanhar e executar tickets operacionais, participar
de sprints e acompanhar o backlog de infraestrutura.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 5. 1Password — DevOps Vault

| Campo | Valor |
|---|---|
| Sistema | 1Password |
| Vault | DevOps Vault |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso ao vault "DevOps Vault"
no 1Password para consultar credenciais compartilhadas do time durante
atividades operacionais.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 6. Vault (HashiCorp) — vault_admins

| Campo | Valor |
|---|---|
| Sistema | Vault |
| Grupo | vault_admins |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso ao Vault (grupo vault_admins)
para gerenciar secrets durante atividades de troubleshooting e suporte a
pipelines de CI/CD.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 7. SonarQube — sonar-administrators

| Campo | Valor |
|---|---|
| Sistema | SonarQube |
| Grupo | sonar-administrators |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso ao SonarQube (grupo
sonar-administrators) para acompanhar a qualidade de código nos projetos
de infraestrutura e pipelines que o time mantém.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 8. New Relic

| Campo | Valor |
|---|---|
| Sistema | New Relic |
| Perfil | DevOps |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso ao New Relic para
monitoramento de aplicações, investigação de incidentes e análise de
performance (APM, traces, logs).
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 9. Spot

| Campo | Valor |
|---|---|
| Sistema | Spot |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso de acesso ao Spot para acompanhar
e otimizar custos de infraestrutura cloud.
Em caso de dúvidas, pode espelhar o usuário do meu fellow: [email do fellow]
```

</details>

---

### 10. Email Groups

| Campo | Valor |
|---|---|
| Grupo de email | devops-sre@hotmart.com |
| Justificativa | Ver modelo abaixo |

<details>
<summary>📋 Copiar justificativa</summary>

```
Sou novo no time de DevOps e preciso ser adicionado ao grupo de email
devops-sre@hotmart.com para receber comunicações do time.
```

</details>

---

## Acessos adicionais por organização

Dependendo da PU que você for associado, pode precisar de acessos adicionais a contas AWS específicas:

| PU | Conta AWS | Perfil inicial |
|---|---|---|
| Teachable | aws-teachable / usefedora | Devops-ReadOnly |
| Financial | hotpay | Devops-ReadOnly |
| Content Flow | hotmart-org | Devops-ReadOnly |
| Platform Engineering | Todas | Devops-ReadOnly |

---

## Modelo de justificativa

Use este modelo base e adapte para cada sistema:

```
Sou novo no time de DevOps e preciso deste acesso para iniciar minhas
atividades. Em caso de dúvidas, pode espelhar o usuário do meu fellow:
[email do fellow]
```

---

## Checklist de acessos

Use este checklist para acompanhar o progresso das suas solicitações:

- [ ] AWS — Amazon Q Developer
- [ ] AWS — Contas principais (hotmart-org, usefedora, notas-org)
- [ ] GitHub — Organizações (Hotmart-Org, usefedora, notas-org)
- [ ] JIRA — DevOps Components
- [ ] 1Password — DevOps Vault
- [ ] Vault — vault_admins
- [ ] SonarQube — sonar-administrators
- [ ] New Relic
- [ ] Spot
- [ ] Email group — devops-sre@hotmart.com
- [ ] VPN (se necessário)

> Abra todos no primeiro dia. Acompanhe no Service Desk. Se passar de 2 dias úteis sem resposta, adicione um comentário pedindo atualização.

---

## Dicas para não travar

- **Abra tudo de uma vez.** Não espere um ticket ser resolvido para abrir o próximo.
- **Mencione o fellow.** Referenciar um colega cujo acesso pode ser espelhado acelera a aprovação.
- **Comece com ReadOnly.** Elevação de permissão pode ser solicitada depois.
- **Acompanhe os tickets.** Verifique o status diariamente nos primeiros dias.
- **Padrão de username GitHub:** `<email sem ponto>-hotmart` (ex: `marcoribeiro-hotmart`)
