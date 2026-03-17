# Acesso via Okta

## 📑 Índice

- [O que é SSO](#o-que-é-sso)
- [Acessando o Okta](#acessando-o-okta)
- [Ferramentas acessadas pelo Okta](#ferramentas-acessadas-pelo-okta)
- [Fluxo de acesso](#fluxo-de-acesso)
- [Configurando MFA](#configurando-mfa)
- [Problemas comuns](#problemas-comuns)

---

A Hotmart utiliza o [Okta](https://hotmart.okta.com) como sistema centralizado de Single Sign-On (SSO). Isso significa que a maioria das ferramentas e sistemas internos são acessados através de uma única autenticação, sem precisar gerenciar múltiplas senhas.

---

## O que é SSO

Single Sign-On é um mecanismo de autenticação que permite ao usuário fazer login uma única vez e acessar múltiplos sistemas sem precisar se autenticar novamente em cada um. Na prática, você entra no Okta com suas credenciais corporativas e, a partir daí, acessa todas as ferramentas autorizadas com um clique.

---

## Acessando o Okta

URL: [https://hotmart.okta.com](https://hotmart.okta.com)

Use suas credenciais corporativas (e-mail e senha da Hotmart) para fazer login. Se for seu primeiro acesso, siga as instruções de configuração de MFA (autenticação multifator) que aparecerão na tela.

---

## Ferramentas acessadas pelo Okta

Após ter os acessos aprovados, as seguintes ferramentas aparecerão no seu painel do Okta:

| Ferramenta | Descrição |
|---|---|
| AWS | Console da Amazon Web Services |
| JIRA | Gestão de projetos e tickets |
| Vault | Gerenciamento de secrets |
| SonarQube | Análise de qualidade de código |
| Spot | Otimização de custos de infraestrutura cloud |

Outras ferramentas podem aparecer conforme você solicita acessos adicionais ao longo do onboarding.

---

## Fluxo de acesso

O processo para obter acesso a uma ferramenta segue sempre o mesmo caminho:

```
Você abre uma          Gestor ou           Acesso é            Ferramenta aparece
solicitação no    →    responsável    →    provisionado   →    no painel do Okta
Service Desk           aprova                                   e está pronto para uso
```

O tempo de aprovação varia por ferramenta e perfil de acesso. Em geral, acessos padrão são aprovados em 1 a 2 dias úteis. Acessos com permissões elevadas podem levar mais tempo por exigirem aprovação adicional.

---

## Configurando MFA

O Okta exige autenticação multifator. Na primeira vez que acessar, você será guiado para configurar um segundo fator. As opções disponíveis são:

- **Okta Verify** (app mobile) — recomendado
- **Google Authenticator**
- **SMS** (menos seguro, evite se possível)

Instale o app Okta Verify no seu celular antes de fazer o primeiro login para facilitar o processo.

---

## Problemas comuns

**Ferramenta não aparece no painel após aprovação**
Aguarde alguns minutos e atualize a página. Se o problema persistir após 30 minutos, abra um ticket no Service Desk informando o número da solicitação original.

**Erro de autenticação ao acessar uma ferramenta pelo Okta**
Tente fazer logout do Okta e login novamente. Se o erro persistir, verifique se o acesso foi aprovado no Service Desk.

**MFA não funciona**
Verifique se o horário do seu celular está sincronizado automaticamente. Tokens TOTP são sensíveis a diferenças de horário.
