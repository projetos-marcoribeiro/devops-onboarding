# Kiro — Setup e Configuração

## 📑 Índice

- [Instalação no macOS](#instalação-no-macos)
- [Usando o Kiro no dia a dia](#usando-o-kiro-no-dia-a-dia)
- [Configurações MCP](#configurações-mcp)
- [Dicas de uso](#dicas-de-uso)

---

O Kiro é um assistente de desenvolvimento com IA desenvolvido pela AWS. Ele pode ser usado como IDE completa ou como ferramenta de linha de comando (CLI) para auxiliar na criação e manutenção de documentação, automações, código de infraestrutura e muito mais.

No contexto do time DevOps da Hotmart, o Kiro é especialmente útil para:
- Criar e manter documentação técnica
- Escrever e revisar código de infraestrutura (Terraform, Helm, scripts)
- Automatizar tarefas repetitivas com suporte de IA
- Explorar e entender codebases desconhecidos rapidamente

---

## Instalação no macOS

### Opção 1 — IDE (recomendado para uso completo)

O Kiro IDE é uma IDE completa baseada em VS Code com IA integrada. É a opção recomendada para quem quer usar o Kiro como ambiente principal de desenvolvimento.

1. Acesse [https://kiro.dev](https://kiro.dev)
2. Faça o download da versão para macOS
3. Abra o arquivo `.dmg` baixado
4. Arraste o Kiro para a pasta `Applications`
5. Abra o Kiro e faça login com sua conta AWS Builder ID ou credenciais corporativas

### Opção 2 — CLI

Se preferir usar o Kiro apenas como assistente de linha de comando, sem abrir a IDE completa:

```bash
# Instalar via npm
npm install -g @aws/kiro-cli

# Verificar instalação
kiro --version
```

Ou via Homebrew (quando disponível):

```bash
brew install kiro
```

Após instalar, autentique:

```bash
kiro auth login
```

---

## Usando o Kiro no dia a dia

### Como IDE

Abra o Kiro IDE e use o painel de chat lateral para interagir com o assistente. Você pode:

- Pedir para o Kiro explicar um arquivo ou trecho de código
- Solicitar a criação de documentação para um módulo Terraform
- Pedir sugestões de melhoria em pipelines CI/CD
- Gerar scripts de automação a partir de uma descrição em linguagem natural

### Como CLI

```bash
# Iniciar uma conversa no terminal
kiro chat

# Pedir ao Kiro para analisar um arquivo
kiro ask "explique o que esse Terraform faz" --file main.tf

# Gerar documentação
kiro docs generate --path ./modules/eks
```

---

## Configurações MCP

O Kiro suporta o protocolo MCP (Model Context Protocol), que permite conectar servidores externos para expandir as capacidades do assistente com ferramentas e contextos adicionais.

MCP servers podem ser configurados no arquivo `.kiro/settings/mcp.json` dentro do seu workspace:

```json
{
  "mcpServers": {
    "aws-docs": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Exemplos de MCP servers úteis para o time DevOps:

| Server | Descrição |
|---|---|
| `aws-documentation` | Acesso à documentação oficial da AWS diretamente no Kiro |
| `terraform-registry` | Consulta ao registry de providers e modules do Terraform |
| `kubernetes` | Interação com clusters Kubernetes via MCP |
| `github` | Acesso a repositórios e PRs do GitHub |

Para instalar servidores MCP que usam o comando `uvx`, você precisa ter o `uv` instalado:

```bash
# Instalar uv (gerenciador de pacotes Python)
brew install uv

# Após instalar, o uvx estará disponível automaticamente
uvx --version
```

---

## Dicas de uso

- Use o Kiro para entender rapidamente repositórios desconhecidos durante o onboarding. Abra o repositório na IDE e pergunte ao assistente como o projeto está estruturado.
- Para documentação, o Kiro consegue gerar rascunhos a partir de código existente. É um bom ponto de partida para documentar módulos Terraform ou pipelines.
- O Kiro tem acesso ao contexto dos arquivos abertos no editor. Quanto mais contexto você fornecer, melhor a qualidade das respostas.
