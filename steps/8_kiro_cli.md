# Passo 8 — Kiro CLI (Linha de Comando)

> **Objetivo:** Levar o Kiro além da IDE e utilizá-lo diretamente no terminal. Explorar a Kiro CLI como um ambiente de desenvolvimento completo — chat interativo, agentes personalizados, tradução de comandos shell, controle de sessões e operações MCP via linha de comando.

---

## 8.1 — O Kiro Além da IDE

Todas as atividades anteriores foram executadas no ambiente gráfico da IDE do Kiro. Contudo, muitos fluxos de desenvolvimento acontecem exclusivamente pelo terminal — seja trabalhando remotamente via SSH ou em conjunto com ferramentas como tmux, Neovim e Emacs.

A **Kiro CLI** oferece a experiência do agente diretamente no terminal. Não se trata de uma versão simplificada: ela compartilha os mesmos agentes, modelos de IA, diretrizes de Steering, automações de Hooks, configurações de MCP e pacotes de Skills disponíveis na IDE.

---

## 8.2 — Instalando a CLI

### macOS / Linux

```bash
curl -fsSL [https://cli.kiro.dev/install](https://cli.kiro.dev/install) | bash
```

### Windows (PowerShell)

```powershell
irm '[https://cli.kiro.dev/install.ps1](https://cli.kiro.dev/install.ps1)' | iex
```

Concluída a instalação, efetue a autenticação:

```bash
kiro-cli login
```

O comando abrirá uma janela no navegador para autenticação. É possível usar a mesma conta vinculada à sua IDE (Google, GitHub, Builder ID ou IAM Identity Center). Em terminais remotos ou sessões SSH, o sistema adota o fluxo de autenticação por dispositivo (*device code*), apresentando o código e a URL para validação externa.

### Validando a Instalação

```bash
kiro-cli whoami    # Exibe o status da conta conectada
kiro-cli version   # Apresenta a versão instalada da CLI
kiro-cli doctor    # Executa diagnóstico para identificar pendências no ambiente
```

---

## 8.3 — Chat Interativo no Terminal

A forma mais simples de inicializar a ferramenta:

```bash
cd tic-tac-toe
kiro-cli
```

Isso inicializa uma sessão interativa de chat no terminal equipada com destaque de sintaxe para código, painéis organizados e acompanhamento das ações das ferramentas em tempo real.

### Executando Perguntas Diretamente

Você pode passar a instrução diretamente como argumento sem precisar abrir a tela inicial interativa:

```bash
kiro-cli chat "Explique a lógica de funcionamento do Board.tsx"
```

### Testando na Prática

Abra o terminal, navegue até a pasta do projeto do jogo da velha e inicie o `kiro-cli`. Solicite uma análise da aplicação:

```text
Explique como funciona a identificação de vitória no jogo da velha
```

Em seguida, peça uma modificação no código:

```text
Adicione um contador de movimentos mostrando quantas jogadas já foram feitas na partida atual
```

Observe que ele lê arquivos, gera código, roda comandos e respeita os manuais de Steering configurados no Passo 4 — tudo diretamente pela linha de comando.

---

## 8.4 — Tradução de Comandos Shell

Um dos recursos mais úteis da CLI para agilizar rotinas operacionais: traduzir descrições em linguagem comum para comandos shell prontos para execução:

```bash
kiro-cli translate "localize todos os arquivos TypeScript alterados hoje"
```

O Kiro retorna a sintaxe exata do comando no terminal. Você também pode solicitar variações de alternativas:

```bash
kiro-cli translate -n 3 "compacte todos os arquivos de log com mais de 30 dias"
```

---

## 8.5 — Execução de Comandos Shell sem Sair do Chat

Durante uma sessão de chat interativa, utilize o prefixo `!` para executar comandos shell diretamente no seu sistema operacional:

```bash
!npm run build
!git status
!npm test
```

O retorno é transmitido em tempo real. Comandos com suporte a TTY interativo (como `vim`, `ssh` e `top`) operam normalmente. Saídas muito longas são resumidas automaticamente nas linhas iniciais e finais — pressione `Ctrl+O` para expandir a visualização completa.

Isso elimina a necessidade de suspender ou fechar a conversa para rodar comandos rápidos.

---

## 8.6 — Prompts em Múltiplas Linhas e Editor Externo

Para instruções mais elaboradas, você tem diferentes alternativas:

- **Shift+Enter** — Quebra a linha no cursor (compatível com iTerm2, Ghostty, Kitty, Warp, Zed)
- **Ctrl+J** — Insere uma quebra de linha (funciona em todos os terminais, incluindo sessões tmux)
- **Alt+Enter** — Insere uma quebra de linha (padrão em Terminal.app e Ghostty)
- **`/editor`** — Abre o seu editor de texto padrão (como vim ou nano) para escrever prompts longos

O comando `/editor` é ideal para estruturar solicitações detalhadas sem limitações do prompt de comando.

---

## 8.7 — Comandos de Barra (Slash Commands) no Chat

Dentro de uma conversa interativa, digite `/` para visualizar as opções disponíveis:

| Comando | Descrição |
| :--- | :--- |
| `/model` | Alterna o modelo de IA utilizado durante a sessão |
| `/tools` | Consulta e pesquisa ferramentas ativas no ambiente |
| `/agent` | Alterna para um agente configurado sob medida |
| `/compact` | Compacta o histórico para otimizar o uso da janela de contexto |
| `/context` | Gerencia os arquivos carregados no contexto |
| `/chat` | Controle de sessões (iniciar nova, retomar, salvar ou carregar) |
| `/editor` | Abre o editor de texto para escrever o prompt |
| `/reply` | Abre o editor trazendo a última resposta do agente citada |
| `/help` | Apresenta a lista de comandos suportados |

---

## 8.8 — Gerenciamento de Contexto

Você pode controlar quais arquivos farão parte do escopo da conversa através de padrões glob:

```bash
/context show               # Exibe os arquivos carregados e a contagem de tokens consumidos
/context add "src/**/*.ts"  # Adiciona arquivos com base em padrões glob
/context remove src/app.js  # Remove um arquivo específico do escopo
/context clear              # Limpa todas as regras personalizadas de contexto
```

---

## 8.9 — Gerenciamento de Sessões

A CLI armazena os históricos das conversas de forma automática, permitindo listar, continuar ou transferir sessões:

```bash
# Retoma a sessão mais recente vinculada a este diretório
kiro-cli chat --resume

# Abre um seletor visual com as sessões anteriores
kiro-cli chat --resume-picker

# Lista todas as conversas salvas para o diretório atual
kiro-cli chat --list-sessions

# Abre uma sessão específica utilizando seu identificador
kiro-cli chat --resume-id abc123-def456
```

Dentro de uma conversa aberta, você também pode usar:

```bash
/chat new                  # Inicia uma nova sessão salvando a atual
/chat new "adicione timer" # Inicia uma nova conversa já passando o prompt inicial
/chat resume               # Abre o seletor para alternar entre conversas
/chat save ./sessao.json   # Exporta os dados da conversa para um arquivo JSON
/chat load ./sessao.json   # Restaura uma conversa salva anteriormente
```

O histórico é indexado por diretório, mantendo os registros de cada projeto separados.

### Exportando Históricos (Comparativo com a IDE)

Na IDE, você pode exportar uma conversa clicando com o botão direito na aba do chat e escolhendo **Export Conversation** (que salva como Markdown). Na CLI, utiliza-se `/chat save`, exportando para o formato JSON. Ambos os métodos facilitam compartilhar o histórico da implementação com outros membros do time.

---

## 8.10 — Agentes Customizados

Você pode definir agentes voltados para finalidades específicas de engenharia:

```bash
kiro-cli agent list                        # Lista os agentes configurados
kiro-cli agent create revisor-codigo       # Cria um novo perfil de agente
kiro-cli agent edit revisor-codigo         # Edita os parâmetros e prompts do perfil
kiro-cli agent set-default revisor-codigo  # Define o perfil como agente padrão
```

Para chamá-lo diretamente:

```bash
kiro-cli chat --agent revisor-codigo "Revise as alterações recentes na lógica da partida"
```

Agentes personalizados permitem parametrizar system prompts exclusivos, permissões de ferramentas e comportamentos ajustados para revisão, documentação ou triagem de falhas.

---

## 8.11 — Gerenciamento de MCP pelo Terminal

Você pode configurar servidores MCP diretamente por linha de comando sem precisar abrir o arquivo JSON:

```bash
kiro-cli mcp list                                  # Lista os servidores MCP ativos
kiro-cli mcp add --name playwright \
  --command "npx" \
  --scope workspace                                # Registra um novo servidor
kiro-cli mcp status --name playwright              # Valida o estado da conexão
kiro-cli mcp remove --name playwright              # Descadastra o servidor
kiro-cli mcp import --file config.json workspace   # Importa definições de um arquivo JSON
```

---

## 8.12 — Sugestões Inline no Shell

A CLI suporta autocompletar em estilo texto-fantasma (*ghost text*) conforme você digita no terminal:

```bash
kiro-cli inline enable     # Ativa as sugestões automáticas inline
kiro-cli inline disable    # Desativa a exibição
kiro-cli inline status     # Confere o estado atual do recurso
```

---

## 8.13 — Roteador de Comandos do Kiro

Caso você utilize tanto a versão CLI quanto a interface desktop, o roteador permite parametrizar qual aplicação responderá ao comando `kiro`:

```bash
kiro-cli integrations install kiro-command-router

# Define a CLI como padrão para a chamada do comando `kiro`
kiro set-default cli

# Ou mantenha a IDE como resposta padrão
kiro set-default ide
```

Após essa definição:

- `kiro` → Abre o ambiente configurado como padrão (CLI ou IDE)
- `kiro-cli` → Sempre abrirá o terminal interativo
- `kiro ide` → Sempre abrirá a interface visual da IDE

---

## 8.14 — Comandos de Manutenção

Recursos auxiliares para administração da ferramenta:

### Atualização

```bash
kiro-cli update                    # Atualiza a CLI para a versão mais recente
kiro-cli update --non-interactive  # Atualiza sem solicitar confirmação (ideal para automações)
```

### Temas da Interface

```bash
kiro-cli theme --list    # Lista os esquemas visuais disponíveis
kiro-cli theme dark      # Aplica o tema escuro
kiro-cli theme light     # Aplica o tema claro
kiro-cli theme system    # Alinha a interface com a preferência do sistema operacional
```

### Configurações

```bash
kiro-cli settings list                      # Lista os parâmetros de configuração ativos
kiro-cli settings list --all                # Apresenta todas as variáveis com explicações
kiro-cli settings open                      # Abre o arquivo de parâmetros no seu editor padrão
kiro-cli settings telemetry.enabled false   # Modifica um parâmetro diretamente
```

### Diagnósticos e Reporte

```bash
kiro-cli doctor                                    # Checagem rápida de dependências do ambiente
kiro-cli diagnostic                                # Gera relatório com sistema, ambiente e configurações
kiro-cli issue "Autocompletar com falha no zsh"    # Cria uma issue no repositório do projeto
```

---

## 8.15 — Compatibilidade Total com a IDE

É fundamental ressaltar: a CLI utiliza a mesma base de configurações da interface gráfica:

- **Steering**: Os arquivos de `.kiro/steering/` são carregados da mesma forma
- **Hooks**: As regras de automação em `.kiro/hooks/` operam com os mesmos gatilhos
- **Servidores MCP**: As conexões de `.kiro/settings/mcp.json` continuam ativas
- **Skills**: Os pacotes de `.kiro/skills/` ativam-se sob demanda
- **Powers**: As Powers instaladas entram em execução pelas palavras-chave correspondentes
- **Modelos e Créditos**: O consumo e a seleção de modelos são unificados

Tudo o que foi parametrizado nas etapas anteriores funciona de forma idêntica no terminal, sem necessidade de reconfiguração.

---

## 8.16 — Autocompletar no Shell

A CLI integra-se ao seu interpretador de shell para oferecer completude de comandos com a tecla Tab:

```bash
kiro-cli integrations install    # Instala o suporte a autocompletar no seu shell
kiro-cli integrations status     # Valida se a integração está operando
```

---

## 8.17 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Kiro CLI** | Utilizou a experiência do agente diretamente pelo terminal |
| **Instalação via Script** | Configurou a CLI em linha única no sistema operacional |
| **Chat Interativo** | Executou consultas e alterações na aplicação sem abrir a IDE |
| **Tradução Shell** | Converteu linguagem natural em sintaxe de comandos |
| **Comandos com `!`** | Executou rotinas do sistema operacional sem interromper o chat |
| **Entrada Multilinha** | Utilizou atalhos e o comando `/editor` para prompts elaborados |
| **Comandos de Barra** | Gerenciou modelos, contexto e ferramentas com `/` |
| **Gestão de Sessões** | Retomou, salvou e exportou conversas locais |
| **Agentes Personalizados** | Parametrizou perfis de agente para tarefas específicas |
| **Administração de MCP** | Gerenciou conexões de servidores MCP via terminal |
| **Configuração Unificada** | Comprovou que diretrizes, hooks, skills e powers operam de forma equivalente na CLI |

---

## Resumo Geral do Workshop

Você cobriu toda a extensão da plataforma Kiro — desde a construção inicial guiada por uma imagem até especificações com Specs, instalação de Skills, padronização técnica com Steering, automações com Hooks, integrações via Powers, validação E2E com Playwright MCP e uso da ferramenta via terminal:

| Etapa | Foco | O que foi construído |
| :--- | :--- | :--- |
| 1 | **IDE + Modo Vibe** | Conheceu a IDE e estruturou o app a partir de uma foto |
| 2 | **Specs** | Implementou backend com o fluxo requisitos → design → tarefas |
| 3 | **Skills** | Adicionou boas práticas de design e performance em React |
| 4 | **Steering** | Formalizou as diretrizes e convenções de arquitetura da aplicação |
| 5 | **Hooks** | Criou automações de build e validações de qualidade |
| 6 | **Powers** | Conectou o projeto a serviços externos de nuvem e banco de dados |
| 7 | **MCP + Playwright** | Configurou MCP manual e testou a aplicação em um navegador real |
| 8 | **Kiro CLI** | Operou todas as capacidades da plataforma direto da linha de comando |
