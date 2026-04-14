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

## Ferramentas e Ambiente

**O que é o hotctl e por que preciso dele?**
O hotctl é a CLI interna da Hotmart. Ele simplifica o login via SSO (Okta), a configuração de acesso às contas AWS e a conexão com os clusters EKS. Sem ele, você precisaria configurar tudo manualmente. Instale seguindo o [guia do repositório](https://github.com/Hotmart-Org/hotctl).

**Preciso do Docker Desktop?**
Não. A Hotmart usa o Rancher Desktop como alternativa ao Docker Desktop. Ele fornece o Docker engine e o containerd sem necessidade de licença comercial.

**Qual é a diferença entre buildstaging e produção?**
Buildstaging é o ambiente de staging onde testes e validações são feitos antes de ir para produção. Toda mudança passa por buildstaging primeiro. É onde você vai trabalhar durante o Hands-On.

**O que é o Base Module?**
É o módulo Terraform central da Hotmart. Ele recebe um YAML declarativo e cria automaticamente todos os recursos AWS necessários para uma aplicação (ECR, IAM, ALB, Security Groups, KMS, Secrets). Os times de produto não escrevem Terraform: apenas declaram o que precisam.

---

## Repositórios e Código

**Quais repositórios devo acompanhar primeiro?**
Comece pelo [devops-helm-iac](https://github.com/Hotmart-Org/devops-helm-iac) (onde os deploys são configurados) e pelo [terraform-base-module](https://github.com/Hotmart-Org/terraform-base-module) (como infraestrutura é provisionada). Veja o [Mapa de Repositórios](mapa-de-repositorios) para a lista completa.

**Posso fazer push direto na main?**
Não. Toda mudança passa por Pull Request com revisão. Para repos de IAC, siga o [checklist de revisão](https://github.com/Hotmart-Org/devops-docs/blob/master/docs/process/pastel/iacs-pr-review.md).

---

## Comunicação e Suporte

**Onde peço ajuda quando travo?**
Primeiro, pergunte ao seu fellow. Se ele não estiver disponível, use o grupo geral dos DevOps no Google Chat. Perguntar no grupo tem o benefício de que outros membros podem contribuir e a resposta fica registrada.

**Os desenvolvedores me procuram direto no chat, o que faço?**
Cada PU tem um grupo no Google Chat onde desenvolvedores pedem ajuda. Problemas rápidos podem ser resolvidos ali mesmo. Se exigir mais tempo ou investigação, peça para abrirem um ticket no Service Desk para manter rastreabilidade.

**O que é o Smart Contract?**
É um documento interno que detalha as boas práticas e processos do time DevOps no dia a dia. Seu fellow vai te apresentar durante a fase operacional.

---

## Monitoramento

**Quais ferramentas de monitoramento a Hotmart usa?**
NewRelic (APM), Prometheus + Grafana (métricas), Sentry (error tracking), Kubecost (custos Kubernetes), Pingdom (uptime externo), PagerDuty (alertas e on-call), CloudWatch (métricas AWS). O Datadog é usado exclusivamente pela PU Teachable.

**Preciso de acesso ao Datadog?**
Apenas se você for associado à PU Teachable. Para as demais PUs, métricas de infraestrutura são coletadas via Prometheus e visualizadas no Grafana.

---

> Se você resolveu uma dúvida que não estava aqui, adicione. O próximo engenheiro vai agradecer.
