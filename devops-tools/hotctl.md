# hotctl

## 📑 Índice

- [Por que o hotctl existe](#por-que-o-hotctl-existe)
- [Instalação](#instalação)
- [Configuração inicial](#configuração-inicial)
- [Principais funcionalidades](#principais-funcionalidades)
- [Instalação durante o onboarding](#instalação-durante-o-onboarding)
- [Contribuindo com o hotctl](#contribuindo-com-o-hotctl)

---

O `hotctl` é a CLI interna desenvolvida pelo time DevOps da Hotmart. Ela abstrai operações comuns da plataforma que, sem ela, exigiriam múltiplos comandos de diferentes ferramentas ou conhecimento detalhado da estrutura interna da infraestrutura.

🔗 [Repositório oficial do hotctl](https://github.com/Hotmart-Org/hotctl)

---

## Por que o hotctl existe

A plataforma Hotmart tem múltiplos clusters EKS em diferentes contas AWS, com convenções de nomenclatura e configurações específicas. Sem o hotctl, configurar o acesso a um cluster específico exigiria saber o ARN exato do cluster, a conta AWS correta, a role a ser assumida e os parâmetros do `aws eks update-kubeconfig`.

O hotctl encapsula esse conhecimento e expõe uma interface simples e consistente para as operações mais comuns.

---

## Instalação

As instruções de instalação estão no repositório oficial. Como o hotctl é distribuído internamente, siga as instruções do repositório para obter a versão mais recente:

🔗 [https://github.com/Hotmart-Org/hotctl](https://github.com/Hotmart-Org/hotctl)

Após instalar, verifique:

```bash
hotctl version
```

---

## Configuração inicial

Após instalar, configure o hotctl com suas credenciais e preferências:

```bash
hotctl configure
```

O comando vai guiar você pela configuração inicial, incluindo autenticação e seleção de ambiente padrão.

---

## Principais funcionalidades

### Gerenciamento de clusters

Uma das principais funções do hotctl é facilitar o acesso aos clusters EKS da Hotmart, configurando automaticamente o kubeconfig com as credenciais corretas.

```bash
# Listar clusters disponíveis
hotctl cluster list

# Configurar acesso a um cluster específico
hotctl cluster use <cluster-name>

# Ver cluster atualmente configurado
hotctl cluster current
```

Após executar `hotctl cluster use`, o kubectl estará configurado para apontar para o cluster selecionado e você pode usar os comandos normais do kubectl.

### Operações de infraestrutura

O hotctl pode incluir comandos para operações específicas da plataforma Hotmart que não têm equivalente direto em ferramentas padrão. Consulte o repositório e o `--help` para ver os comandos disponíveis na versão atual:

```bash
# Ver todos os comandos disponíveis
hotctl --help

# Ver ajuda de um subcomando específico
hotctl cluster --help
```

---

## Instalação durante o onboarding

A instalação e configuração do hotctl é uma das primeiras tarefas do onboarding. Sem ele, você não consegue configurar o acesso aos clusters EKS da Hotmart de forma simples.

**Checklist de setup do hotctl:**

- [ ] Repositório clonado ou binário baixado conforme instruções do repositório
- [ ] `hotctl version` executando sem erro
- [ ] `hotctl configure` executado com sucesso
- [ ] `hotctl cluster list` mostrando os clusters disponíveis
- [ ] `hotctl cluster use <cluster-name>` configurando o kubectl corretamente
- [ ] `kubectl get nodes` funcionando após configurar o cluster

---

## Contribuindo com o hotctl

O hotctl é mantido pelo time DevOps. Se você identificar uma operação repetitiva que poderia ser automatizada ou uma melhoria na ferramenta, abra uma issue ou PR no repositório.

Contribuir com ferramentas internas é uma forma valiosa de impactar a produtividade de todo o time.
