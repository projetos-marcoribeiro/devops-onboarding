# FAQ: Perguntas Frequentes do Onboarding

---

Perguntas que todo novo engenheiro DevOps faz nos primeiros dias. Se sua dúvida não está aqui, pergunte no canal do time e depois adicione a resposta aqui para o próximo.

---

## Acessos e Setup

**Quanto tempo demora para os acessos serem aprovados?**
Em geral, 1-2 dias úteis. Acessos com permissões elevadas (AdministratorAccess AWS) podem levar mais. Se passar de 2 dias, adicione um comentário no ticket pedindo atualização.

**Preciso criar um novo usuário no GitHub?**
Sim. Por compliance, você precisa criar um usuário corporativo seguindo o padrão: `<email sem ponto>-hotmart` (ex: `marcoribeiro-hotmart`).

**Não consigo acessar uma ferramenta pelo Okta, o que faço?**
Verifique se o ticket de acesso foi aprovado no Service Desk. Se sim, tente logout/login no Okta. Se persistir, abra um novo ticket referenciando o original.

**Qual perfil AWS devo solicitar primeiro?**
Comece com `Devops-ReadOnly`. É suficiente para a maioria das atividades do onboarding e do Hands-On. O `AdministratorAccess` será solicitado depois, quando necessário.

---

## Training Camp

**Preciso assistir todos os vídeos?**
Siga a ordem do Learning Path. Os vídeos das etapas 1-4 são obrigatórios. Os da etapa 5 (complementares) podem ser assistidos conforme necessidade.

**Os vídeos estão em português ou inglês?**
A maioria está em português. Alguns materiais de referência podem estar em inglês.

**Não consigo acessar um vídeo no Google Drive, o que faço?**
Verifique se está logado com sua conta corporativa Hotmart. Se o acesso estiver restrito, peça ao seu fellow para solicitar permissão ou procure o material na pasta compartilhada: [Treinamento DevOps](https://drive.google.com/drive/folders/1EbEAxfw6X6FBl-ARgiEbTWEOaiItUn_6).

---

## Hands-On

**Qual cluster EKS devo usar para o Hands-On?**
O cluster de staging. Use `hotctl cluster use <cluster-staging>` para configurar o acesso. Pergunte ao fellow qual é o nome exato do cluster.

**O pipeline falhou, o que faço?**
Verifique os logs do GitHub Actions step a step. Os erros mais comuns são: runner incorreto, secrets não configurados, ou base image errada. Se não conseguir resolver em 30 minutos, peça ajuda ao fellow.

**Posso usar uma linguagem diferente de Node.js no backend?**
O repositório handson_devops vem com Node.js, mas o foco é o processo de deploy, não a linguagem. Mantenha o Node.js para simplificar: a complexidade está na infraestrutura, não no código.

---

## Operacional

**Quando começo a pegar tickets do Service Desk?**
Após completar o Hands-On (~semana 3-4). Comece com tickets simples, sempre com orientação do fellow.

**Posso fazer `kubectl apply` direto em produção?**
Não. Toda mudança em produção passa pelo fluxo GitOps via ArgoCD. O kubectl em produção é para leitura e diagnóstico, nunca para modificação.

---

## On-Call

**Quando vou entrar na rotação de on-call?**
É subjetivo e depende do seu progresso. O fellow e o coordenador avaliam juntos quando você está pronto. Use o checklist de prontidão como guia de preparação.

**E se eu receber um alerta e não souber o que fazer?**
Faça acknowledge no PagerDuty, comunique no canal de incidentes que está investigando, e escale para o nível 2 se não conseguir resolver em 15-20 minutos. Escalar não é fraqueza: é responsabilidade.

---

> Se você resolveu uma dúvida que não estava aqui, adicione. O próximo engenheiro vai agradecer.
