# Passo 5 — Hooks (Automações do Agente)

> **Objetivo:** Configurar automações orientadas a eventos com os Agent Hooks. Criar hooks para verificar lint ao salvar, auditar acessibilidade antes de escrever arquivos e rodar a compilação (*build*) após a conclusão de tarefas de especificação.

---

## 5.1 — Por que usar Hooks?

No Passo 4 você ensinou suas convenções ao Kiro com o Steering. Mas convenções só funcionam se forem aplicadas de verdade. Até agora, nada impedia o Kiro (ou você) de escrever código que violasse as regras estabelecidas.

Os Hooks resolvem esse problema. Eles são gatilhos automatizados disparados quando eventos acontecem na IDE — um arquivo é salvo, uma ferramenta está prestes a rodar, uma tarefa de spec é concluída. Ao disparar o gatilho, o Kiro executa um comando shell ou envia um prompt para si mesmo.

Pense nos hooks como uma esteira de CI/CD integrada ao seu editor. Em vez de esperar uma pipeline acusar falhas após o push, os hooks identificam e corrigem problemas *enquanto você programa*.

---

## 5.2 — Como os Hooks Funcionam

Cada hook é composto por duas partes:

1. **Quando (*When*)** — O evento disparador
2. **Então (*Then*)** — A ação que deve ser executada

### Eventos Disparadores

| Evento | É disparado quando... |
| :--- | :--- |
| `fileEdited` | Você salva um arquivo existente |
| `fileCreated` | Um novo arquivo é criado |
| `fileDeleted` | Um arquivo é excluído |
| `promptSubmit` | Você envia uma mensagem para o agente |
| `agentStop` | O agente conclui uma iteração/resposta |
| `preToolUse` | Antes de o Kiro usar uma ferramenta (leitura, escrita, terminal, etc.) |
| `postToolUse` | Logo após o Kiro executar uma ferramenta |
| `preTaskExecution` | Antes de iniciar uma tarefa da spec |
| `postTaskExecution` | Logo após concluir uma tarefa da spec |
| `userTriggered` | Você clica manualmente em um botão de disparo |

### Ações Disponíveis

| Ação | O que faz |
| :--- | :--- |
| `askAgent` | Envia um prompt para o próprio Kiro — o Kiro o interpreta e toma providências |
| `runCommand` | Executa um comando shell no terminal — o output é capturado e apresentado |

---

## 5.3 — Criando Hooks

Existem três maneiras de criar um hook:

### 1. Pedindo ao Kiro (Linguagem Natural)

1. Abra a seção **Agent Hooks** no painel do Kiro.
2. Clique no ícone `+`.
3. Selecione **Ask Kiro to create a hook**.
4. Descreva seu objetivo: "Execute o linter sempre que eu salvar um arquivo TypeScript".
5. Revise a configuração em JSON gerada e clique em **Save Hook**.

### 2. Formulário Manual

1. Clique em `+` → **Manually create a hook**.
2. Preencha os campos: título, descrição, tipo de evento, padrões de arquivo ou tipos de ferramenta, tipo de ação e o comando/prompt.
3. Clique em **Create Hook**.

### 3. Pela Paleta de Comandos

Pressione `Cmd+Shift+P` (ou `Ctrl+Shift+P`) → "Kiro: Open Kiro Hook UI".

Para este workshop, use a abordagem em linguagem natural — ela é mais rápida e demonstra a capacidade do Kiro em converter solicitações em configurações operacionais.

---

## 5.4 — Hook 1: Build ao Salvar (Build on Save)

Vamos começar de forma direta: executar o compilador TypeScript sempre que você salvar um arquivo `.ts` ou `.tsx` para capturar erros de tipagem instantaneamente.

### O que solicitar

Na janela de criação de hook:

```text
Execute "npm run build" sempre que um arquivo TypeScript for salvo.
```

### O que o Kiro gera

```json
{
  "name": "Build on Save",
  "version": "1.0.0",
  "description": "Runs the build when TypeScript files are saved to catch type errors early",
  "when": {
    "type": "fileEdited",
    "patterns": ["*.ts", "*.tsx"]
  },
  "then": {
    "type": "runCommand",
    "command": "npm run build"
  }
}
```

### Pontos para prestar atenção

- O hook é salvo como um arquivo JSON limpo dentro de `.kiro/hooks/`.
- A combinação `fileEdited` + `patterns` garante que o gatilho só dispare para arquivos TypeScript, e não a cada salvamento irrelevante.
- `runCommand` roda o build em segundo plano — a saída aparece na tela sem bloquear seu fluxo de trabalho.
- Se o build quebrar, o erro é exibido imediatamente.

### Veja o hook funcionando

Faça uma pequena alteração em um arquivo `.ts` — introduza propositalmente um erro de tipagem (como atribuir uma string a uma variável numérica). Salve o arquivo. Observe o hook disparar e o build acusar o erro detalhado.

Corrija o código, salve novamente e veja o build passar com sucesso. Ciclo de feedback instantâneo.

Caso o hook não dispare, peça auxílio diretamente ao Kiro pelo chat!

---

## 5.5 — Hook 2: Auditoria de Acessibilidade na Escrita

Este exemplo é mais avançado: fazer o Kiro revisar cada arquivo que ele escreve para buscar problemas de acessibilidade *antes* de confirmar a alteração no disco.

### O que solicitar

```text
Antes de o Kiro gravar qualquer arquivo, revise as alterações em busca de falhas de acessibilidade —
rótulos aria ausentes, HTML não-semântico, falhas de navegação por teclado.
Se houver problemas, corrija-os antes de gravar o arquivo.
```

### O que o Kiro gera

```json
{
  "name": "Accessibility Review",
  "version": "1.0.0",
  "description": "Reviews file writes for accessibility compliance before they happen",
  "when": {
    "type": "preToolUse",
    "toolTypes": ["write"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "Before writing this file, review the changes for accessibility issues: missing aria-labels on interactive elements, non-semantic HTML (div instead of button/nav/main), missing keyboard navigation support, insufficient color contrast considerations, and missing alt text on images. If you find issues, fix them in the content before proceeding with the write."
  }
}
```

### Pontos para prestar atenção

- O evento `preToolUse` atua *antes* da gravação do arquivo — operando como uma barreira de qualidade prévia, e não uma checagem posterior.
- `toolTypes: ["write"]` atinge apenas ações de escrita em disco (ignorando leituras e comandos de terminal).
- `askAgent` envia uma instrução para o próprio Kiro — fazendo com que ele audite a própria entrega antes de salvar.
- Funciona como ter um auditor de acessibilidade acoplado ao fluxo de código.

### Veja o hook funcionando

Peça ao Kiro para criar um novo componente visual — por exemplo, um botão de configurações ou um input para os nomes dos jogadores. Veja o hook disparar antes do arquivo ser salvo. O Kiro analisa o código sob a ótica da acessibilidade e corrige inconsistências antes de gravar.

Para testar de forma evidente, peça para ele criar um elemento interativo usando `<div onClick={...}>` e observe o hook interceptar a gravação e substituir pela tag semântica `<button>`.

---

## 5.6 — Hook 3: Build após Tarefas da Spec

Ao trabalhar em tarefas estruturadas de uma spec (como visto no Passo 2), queremos assegurar que nenhuma tarefa concluída quebre a compilação do projeto.

### O que solicitar

```text
Após a conclusão de cada tarefa da spec, execute o build para verificar se nada foi quebrado.
```

### O que o Kiro gera

```json
{
  "name": "Post-Task Build Check",
  "version": "1.0.0",
  "description": "Runs the build after each spec task to catch regressions early",
  "when": {
    "type": "postTaskExecution"
  },
  "then": {
    "type": "runCommand",
    "command": "npm run build"
  }
}
```

### Pontos para prestar atenção

- O evento `postTaskExecution` conecta-se diretamente ao ciclo de desenvolvimento de Specs do Passo 2.
- A cada tarefa finalizada no checklist, o build é executado de forma automática.
- Se uma tarefa causar regressão, você é notificado imediatamente — sem esperar acumular erros nas tarefas seguintes.
- Essencial para execuções sequenciais no modo Autopilot.

---

## 5.7 — Inspecione as Alterações: `#git diff`

Depois de configurar hooks e modificar o código, pause e inspecione tudo o que foi alterado utilizando o provedor de contexto `#git diff`.

No chat, envie:

```text
#git diff O que foi alterado neste passo? Faça um resumo dos hooks que adicionei e das modificações feitas no código.
```

O Kiro consulta o diff atual do Git e retorna um resumo estruturado. Esse é um excelente ponto de parada para revisar o escopo antes de realizar commits.

Outros provedores úteis para testar aqui:

- `#codebase` — "Como está estruturado o sistema de hooks?" (o Kiro localiza os arquivos relevantes sozinho)
- `#file .kiro/hooks/build-on-save.json` — Permite referenciar um hook específico na sua pergunta

---

## 5.8 — Gerenciamento de Hooks

### Visualizando os Hooks

Todos os hooks configurados são listados na aba **Agent Hooks** no painel lateral, exibindo:

- Nome e descrição
- Tipo de gatilho
- Status (ativo ou inativo)

### Ativando e Desativando

Utilize a chave seletora ao lado de qualquer hook para ativá-lo ou desativá-lo temporariamente sem precisar apagar o arquivo. Útil quando um hook se torna verboso em explorações livres, mas essencial em entregas formais.

### Arquivos no Disco

Os hooks são armazenados como arquivos JSON em `.kiro/hooks/`:

```
.kiro/hooks/
├── build-on-save.json
├── accessibility-review.json
└── post-task-build-check.json
```

Como são arquivos texto, podem ser versionados no Git — compartilhando as mesmas automações com toda a equipe do projeto.

---

## 5.9 — Padrões Recomendados de Hooks

Além dos exemplos criados, aqui estão padrões comuns para o dia a dia:

| Padrão | Evento | Ação | Caso de Uso |
| :--- | :--- | :--- | :--- |
| Formatação ao Salvar | `fileEdited` + `*.ts` | `runCommand`: `npx prettier --write {file}` | Formatação automática de código |
| Revisão de Mensagem de Commit | `preToolUse` + `shell` | `askAgent`: "Revise este commit para dados sensíveis" | Evitar vazamento de credenciais e chaves |
| Testes pós-alteração | `fileEdited` + `*.test.ts` | `runCommand`: `npm test` | Executar testes unitários ao salvar |
| Lembrete de Documentação | `postToolUse` + `write` | `askAgent`: "Verifique se esta mudança requer atualizar o README" | Manter a documentação alinhada |
| Verificação de Deploy Manual | `userTriggered` | `askAgent`: "Audite a base de código quanto à prontidão para deploy" | Auditoria sob demanda |

---

## 5.10 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Agent Hooks** | Criou três hooks utilizando diferentes tipos de disparadores |
| **Gatilho `fileEdited`** | Compilação automática ao salvar arquivos TypeScript |
| **Gatilho `preToolUse`** | Auditoria preventiva de acessibilidade antes da escrita em disco |
| **Gatilho `postTaskExecution`** | Validação de compilação após cada tarefa de spec concluída |
| **Ação `runCommand`** | Execução automática de comandos no terminal |
| **Ação `askAgent`** | Instruiu o Kiro a auditar a própria entrega antes de salvar |
| **Criação em Linguagem Natural** | Descreveu a regra em linguagem comum e o Kiro gerou a configuração JSON |
| **Gerenciamento de Hooks** | Aprendeu a ativar, pausar e organizar arquivos na pasta `.kiro/hooks/` |
| **Contexto `#git diff`** | Revisou todas as modificações realizadas na etapa |
| **Provedores de Contexto** | Usou `#codebase` e `#file` para referenciar itens da aplicação |

---

## Ponto Principal

O Steering ensina as regras ao Kiro. Os Hooks garantem que elas sejam cumpridas de forma contínua. Juntos, formam um ambiente onde as convenções não são apenas teoria, mas sim verificadas a cada salvamento, escrita ou tarefa finalizada.

Os melhores hooks são aqueles transparentes: operam em segundo plano e previnem falhas antes que elas se tornem problemas maiores.

---

## Próximos Passos

Agora você já conta com convenções (Steering) e automação (Hooks). No próximo passo, você estenderá as capacidades do Kiro com **Powers** — pacotes integrados que conectam o agente a serviços de nuvem e ferramentas externas.
