# Passo 7 — MCP e Testes Automatizados com Playwright

> **Objetivo:** Configurar manualmente um servidor MCP puro, instalar a integração do Playwright MCP e utilizá-la para validar o jogo da velha fazendo com que o Kiro jogue a aplicação dentro de um navegador real.

---

## 7.1 — O que é o MCP?

Você já utilizou o MCP de forma indireta — as Powers utilizam servidores MCP internamente. Contudo, você também pode configurar servidores MCP diretamente para ferramentas que ainda não possuem uma Power pronta ou quando deseja controle total das definições.

O **Model Context Protocol (MCP)** é um padrão aberto que viabiliza a comunicação entre o Kiro e servidores externos para acessar ferramentas especializadas, prompts e recursos de contexto. Pense nele como um sistema de plugins: cada servidor MCP expõe um conjunto de métodos que o Kiro pode invocar durante a conversa.

Com o MCP, o Kiro pode:

- Navegar na web e manipular páginas ativas
- Consultar bancos de dados relacionais e analíticos
- Consumir endpoints de APIs externas
- Consultar bases de conhecimento corporativas
- Executar scripts e ferramentas desenvolvidas por você

---

## 7.2 — Por que usar o Playwright MCP?

Você estruturou o jogo da velha, adicionou backend, redesenhou a interface, formalizou diretrizes e criou automações. Agora, precisamos testar a experiência do usuário de ponta a ponta.

O **servidor Playwright MCP** permite que o Kiro controle um navegador web real — navegando por links, clicando em elementos, preenchendo formulários, capturando prints e inspecionando a árvore do DOM. Isso possibilita pedir para o Kiro:

- Acessar a URL local da sua aplicação
- Disputar uma partida completa clicando nas casas do tabuleiro
- Confirmar se a verificação de vitória e empate está funcionando
- Validar se os dados do leaderboard estão sendo atualizados corretamente
- Capturar telas como evidência dos testes executados

É o equivalente a ter um engenheiro de QA interagindo com a sua aplicação em tempo real.

---

## 7.3 — Configurando o Servidor Playwright MCP

Os servidores MCP são parametrizados em arquivos `mcp.json`. Existem dois níveis de configuração:

| Escopo | Localização | Aplica-se a |
| :--- | :--- | :--- |
| **Workspace** | `.kiro/settings/mcp.json` | Apenas a este projeto |
| **Usuário (Global)** | `~/.kiro/settings/mcp.json` | A todos os seus projetos |

### Adicionando o Servidor do Playwright

Crie ou edite o arquivo `.kiro/settings/mcp.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Salve o arquivo. O Kiro identifica o novo servidor automaticamente e estabelece a conexão.

### Verificando o Status da Conexão

1. Abra a aba **MCP Servers** no painel lateral do Kiro.
2. Localize a entrada "playwright" acompanhada de um indicador verde de conexão ativa.
3. Clique sobre o nome do servidor para inspecionar os métodos disponíveis.

### Ferramentas Fornecidas pelo Playwright MCP

O servidor disponibiliza um catálogo completo para automação web:

| Ferramenta | Descrição |
| :--- | :--- |
| `browser_navigate` | Direciona o navegador para uma URL específica |
| `browser_snapshot` | Captura a árvore de acessibilidade da tela (mais precisa que prints para interações) |
| `browser_click` | Executa o clique em um elemento da página |
| `browser_type` | Insere texto em campos de formulário |
| `browser_take_screenshot` | Realiza a captura visual da tela (print) |
| `browser_evaluate` | Executa código JavaScript no contexto da página |
| `browser_console_messages` | Realiza a leitura de logs e erros do console |
| `browser_network_requests` | Analisa as requisições de rede trafegadas |
| `browser_hover` | Simula a passagem do cursor do mouse sobre elementos |
| `browser_select_option` | Seleciona opções em caixas de seleção (*dropdowns*) |
| `browser_fill_form` | Preenche múltiplos inputs de formulário simultaneamente |
| `browser_wait_for` | Aguarda a exibição ou desaparecimento de elementos ou textos na tela |

---

## 7.4 — Testando a Aplicação na Prática

Certifique-se de que o servidor local esteja ativo (`npm run dev`), abra uma conversa no modo Vibe e teste as instruções abaixo:

### Teste 1: Jogar uma Partida Completa

```text
Abra o jogo da velha em http://localhost:5173 no navegador.
Jogue uma partida completa — intercalando movimentos entre X e O.
Faça o jogador X vencer fechando uma diagonal.
Tire uma captura de tela (screenshot) da tela de vitória.
```

### O que observar

O Kiro irá:

1. Abrir a aplicação pelo endereço informado
2. Executar um snapshot para mapear os elementos da interface
3. Clicar nas posições do tabuleiro, alternando as marcações
4. Identificar a mensagem indicando a vitória de X
5. Capturar o print demonstrando o estado final do tabuleiro

Você poderá visualizar o Kiro operando o navegador de forma autônoma.

### Teste 2: Validar a Atualização do Leaderboard

```text
Após finalizar a partida, navegue até a tela de leaderboard.
Confirme se a vitória foi computada corretamente para o jogador X.
Capture um screenshot da tabela de classificação.
```

### Teste 3: Checagem de Casos de Borda (Empate)

```text
Jogue uma partida que termine em empate — preencha todas as casas sem que haja um vencedor.
Verifique se a mensagem de empate é apresentada.
Em seguida, clique no botão de reiniciar e confirme se o grid foi limpo.
```

### Teste 4: Auditoria de Acessibilidade em Tempo de Execução

```text
Faça um snapshot da página do jogo e avalie se todos os elementos interativos
possuem rótulos de acessibilidade adequados. Gere um relatório com os apontamentos.
```

O método `browser_snapshot` inspeciona a árvore de acessibilidade do navegador, permitindo ao Kiro identificar ausência de labels, elementos não-semânticos e lacunas de navegação por teclado.

---

## 7.5 — Bugfix Specs: Quando os Testes Encontram Falhas

Se durante a execução com Playwright o Kiro apontar uma inconsistência — como pontuação incorreta no placar ou falha na detecção de vitórias diagonais —, utilize uma **Bugfix Spec**.

No Passo 2 exploramos as Feature Specs. As Bugfix Specs são voltadas exclusivamente para diagnóstico e correção controlada de bugs.

### Como criar uma Bugfix Spec

1. Inicie uma nova sessão escolhendo o tipo **Spec** no chat.
2. Selecione a opção **Bug** (em vez de Feature).
3. Descreva a falha encontrada:

```text
O placar de líderes está computando vitórias de forma errada. Quando o jogador X ganha uma partida,
o sistema por vezes atribui a vitória ao jogador O.
Identifiquei este comportamento ao simular as partidas pelo Playwright e inspecionar o leaderboard.
```

### O que o Kiro gera

Em vez do arquivo `requirements.md`, o Kiro gera o documento `bugfix.md` estruturado em:

- **Comportamento Atual (*Current behavior*)**: O erro identificado no sistema
- **Comportamento Esperado (*Expected behavior*)**: O comportamento corrigido
- **Comportamento Inalterado (*Unchanged behavior*)**: O que **não** deve sofrer alterações (checagem de empate, histórico e reset)

A partir daí, segue-se o mesmo fluxo `design.md` → `tasks.md`, direcionado à correção da falha.

### Por que isso é importante?

As Bugfix Specs evitam o problema comum de corrigir uma parte do sistema e quebrar outra. Ao explicitar o que precisa ser mantido intacto, evitam-se regressões no código. O fluxo de testar → encontrar a falha → criar a spec de correção → aplicar o fix → retestar consolida o ciclo de qualidade.

---

## 7.6 — Detalhes da Configuração de Servidores MCP

### Estrutura do arquivo mcp.json

```json
{
  "mcpServers": {
    "<nome-do-servidor>": {
      "command": "npx",
      "args": ["<pacote>@latest"],
      "env": {
        "CHAVE_API": "sua-chave-aqui"
      },
      "disabled": false,
      "autoApprove": ["ferramenta_1", "ferramenta_2"]
    }
  }
}
```

| Campo | Finalidade |
| :--- | :--- |
| `command` | Executável a ser chamado (ex.: `npx`, `uvx`, `node`) |
| `args` | Argumentos repassados para a execução do comando |
| `env` | Variáveis de ambiente injetadas no processo do servidor |
| `disabled` | Ativa ou desativa o servidor sem apagar a configuração |
| `autoApprove` | Lista de ferramentas que podem rodar sem confirmação manual |

### Precedência de Escopos

Quando o mesmo servidor for definido nas configurações do workspace e nas configurações globais do usuário, a definição do workspace prevalece.

### Utilizando `#mcp` no Chat

Você pode invocar métodos e recursos de servidores MCP diretamente nas mensagens utilizando `#mcp`:

```text
#mcp:playwright capture um screenshot da página aberta no navegador
```

---

## 7.7 — Comparativo: MCP Manual vs Powers

Com a experiência de configurar um servidor manual, a distinção entre as abordagens fica nítida:

| | MCP Manual | Powers |
| :--- | :--- | :--- |
| **Configuração** | Definição manual de arquivo JSON | Instalação com um clique |
| **Carregamento de Ferramentas** | Fixo e contínuo no contexto | Dinâmico baseado em palavras-chave |
| **Boas Práticas** | Não acompanham o pacote | Embutidas no arquivo `POWER.md` |
| **Hooks e Automações** | Não inclusos | Podem vir empacotados |
| **Melhor Aplicação** | Ferramentas customizadas ou sem Power pronta | Serviços de nuvem com rotinas homologadas |

Utilize servidores MCP puros quando precisar de ferramentas específicas (como o Playwright). Adote Powers sempre que houver integração pronta para o serviço desejado.

---

## 7.8 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Configuração de MCP** | Parametrizou manualmente o servidor do Playwright em `mcp.json` |
| **Ferramentas de Automação** | Empregou métodos de navegador para validar as funcionalidades do jogo |
| **Testes End-to-End** | Validou regras de vitória, placar e empates com simulação de usuário |
| **Snapshot de Acessibilidade** | Analisou a conformidade de a11y com o método `browser_snapshot` |
| **Evidências Visuais** | Registrou capturas de tela dos testes executados |
| **Provedor `#mcp`** | Invocou comandos de ferramentas externas dentro do chat |
| **Bugfix Specs** | Estruturou correções delimitando comportamentos esperados e inalterados |
| **MCP vs Powers** | Identificou quando optar por configurações puras ou pacotes integrados |

---

## Ponto Principal

O MCP estabelece o protocolo de comunicação entre o Kiro e ferramentas externas. Para rotinas de qualidade, o servidor do Playwright transforma o agente em um avaliador capaz de interagir com o sistema, simular fluxos e verificar o comportamento da aplicação em um navegador real.

A parametrização exige apenas um arquivo JSON, viabilizando testes orientados por instruções simples em linguagem natural.

---

## Próximos Passos

Até o momento, todo o trabalho ocorreu dentro do ambiente visual da IDE. No próximo passo, você levará o Kiro para o terminal utilizando a **Kiro CLI** — explorando a experiência do agente para desenvolvedores focados em linha de comando.
