# Passo 4 — Steering (Diretrizes e Convenções)

> **Objetivo:** Ensinar ao Kiro as convenções do seu projeto utilizando arquivos de Steering. Gerar documentos fundamentais, criar diretrizes personalizadas para sua API e padrões de componentes, e entender como os modos de inclusão controlam o carregamento de contexto.

---

## 4.1 — Por que usar Steering?

No Passo 3 você instalou Skills — conhecimentos gerais compartilhados pela comunidade (design de interface e regras de performance em React). Skills são excelentes, mas não conhecem nada sobre as particularidades do *seu* projeto.

O Steering preenche essa lacuna. É por meio dele que você documenta para o Kiro:

- Qual stack tecnológica está sendo utilizada e as razões da escolha
- Como os arquivos do projeto estão distribuídos
- Quais regras de codificação a sua equipe adota
- Como os endpoints da sua API devem ser padronizados
- Quais padrões devem ser seguidos (e o que deve ser evitado)

Sem o Steering, cada conversa no chat começa como uma folha em branco — exigindo que você relembre suas preferências repetidamente. Com o Steering configurado, o Kiro mantém a memória do projeto ativa. É o equivalente a integrar um desenvolvedor na equipe que realmente lê a documentação técnica antes de escrever código.

---

## 4.2 — Onde os Arquivos de Steering Ficam Armazenados

Os arquivos de Steering são documentos Markdown salvos em diretórios dedicados:

| Escopo | Localização | Aplica-se a |
| :--- | :--- | :--- |
| **Workspace** | `.kiro/steering/` | Apenas a este projeto |
| **Global** | `~/.kiro/steering/` | A todos os seus projetos |

O Steering do workspace tem precedência sobre o global em caso de regras conflitantes. Isso permite que você tenha padrões globais ("sempre usar TypeScript") e personalize regras por repositório ("este projeto adota Tailwind em vez de CSS Modules").

---

## 4.3 — Gerando Arquivos de Steering Fundamentais

O Kiro pode mapear e gerar automaticamente três arquivos essenciais que descrevem o seu repositório. Vamos gerá-los agora:

### Como gerar

1. Abra a seção **Steering** no painel lateral do Kiro.
2. Clique no botão **Generate Steering Docs** (ou clique em `+` → **Protect steering files**).
3. O Kiro analisa o código-fonte da aplicação e gera três documentos fundamentais.

### Os três arquivos fundamentais

**`product.md`** — A proposta do projeto

O Kiro documenta a visão geral do jogo da velha: seu objetivo, público-alvo, funcionalidades centrais (tabuleiro, placar de líderes e histórico de partidas) e o contexto de ser um projeto prático para workshops.

**`tech.md`** — A stack de tecnologias

O Kiro lista as definições técnicas: React + Vite + TypeScript no frontend, Node.js + Express no backend, persistência em memória e as dependências instaladas durante o processo.

**`structure.md`** — A organização dos diretórios

O Kiro mapeia a arquitetura de pastas: localização de componentes, definição de rotas de API, organização das folhas de estilo e convenções de nomenclatura adotadas.

### Pontos para prestar atenção

- O Kiro compreende a *arquitetura* do projeto, reconhecendo o isolamento entre backend e frontend e separando componentes de interface daqueles com regras de negócio.
- Esses arquivos vêm com a política de inclusão padrão **sempre ativos (*always*)** — fornecendo contexto prévio em qualquer conversa futura.
- Eles são totalmente editáveis: caso algo precise de ajustes ou você queira incluir novas regras de arquitetura, altere o arquivo diretamente.

---

## 4.4 — Criando Arquivos de Steering Personalizados

Os documentos fundamentais estabelecem a base. Com arquivos personalizados, você define convenções técnicas detalhadas. Vamos criar dois exemplos.

### Arquivo personalizado 1: Convenções de API

Crie um arquivo para orientar o desenvolvimento de rotas da sua API backend:

1. Na seção Steering, clique no ícone `+`.
2. Selecione o escopo **Workspace**.
3. Defina o nome como `api-conventions.md`.

Insira o conteúdo a seguir:

````markdown
---
inclusion: fileMatch
fileMatchPattern: "server/**/*.ts"
---

# Convenções de API

## Estrutura de Endpoints

- Todos os endpoints devem utilizar o prefixo `/api/v1/`
- Utilize substantivos no plural para identificar recursos: `/api/v1/games`, `/api/v1/players`
- Siga estritamente os métodos HTTP: GET para leituras, POST para criação, PUT para atualizações e DELETE para remoção

## Formato Padrão de Resposta

Todas as respostas da API devem seguir este contrato JSON:

```json
{
  "success": true,
  "data": { ... },
  "error": null
}
```

Em cenários de erro:

```json
{
  "success": false,
  "data": null,
  "error": { "code": "NOT_FOUND", "message": "Partida não encontrada" }
}
```

## Tratamento de Falhas

- Retorne os códigos de status HTTP correspondentes (200, 201, 400, 404, 500)
- Nunca exponha detalhes internos ou stack traces do servidor para o cliente
- Registre erros no console do servidor com timestamp legível

## Nomenclatura

- Arquivos de rotas: `<recurso>.routes.ts`
- Funções manipuladoras (*handlers*): `get<Recurso>`, `create<Recurso>`, `update<Recurso>`
````

### Arquivo personalizado 2: Padrões de Componentes

Crie um segundo arquivo para padronizar os componentes React:

1. Na seção Steering, clique em `+`.
2. Escolha **Workspace** e nomeie como `component-conventions.md`.

Adicione o conteúdo:

```markdown
---
inclusion: fileMatch
fileMatchPattern: ["src/components/**/*.tsx", "src/components/**/*.ts"]
---

# Padrões de Componentes React

## Estrutura de Arquivos

- Mantenha apenas um componente por arquivo
- O nome do arquivo deve corresponder ao componente exportado: `GameBoard.tsx` exporta `GameBoard`
- Aloque as folhas de estilo junto com o componente correspondente: `GameBoard.tsx` + `GameBoard.css`

## Padrões de Código

- Utilize exclusivamente componentes funcionais com hooks (sem classes)
- Nomeie as interfaces de propriedades como `<Componente>Props`: `GameBoardProps`
- Desestruture as propriedades diretamente na assinatura da função
- Mantenha componentes concisos — se acumular muitas responsabilidades, fatore-o em componentes menores

## Gerenciamento de Estado

- Utilize `useState` para controle de estado puramente local
- Eleve o estado (*lift state*) para o componente pai comum mais próximo ao compartilhar dados
- Prefira a forma funcional do `setState` sempre que o novo valor depender do valor anterior

## Acessibilidade

- Elementos interativos devem conter rótulos descritivos (*aria-labels*)
- Utilize tags HTML semânticas (`<button>`, `<nav>`, `<main>`, `<section>`)
- Assegure navegação completa por teclado em todas as ações de jogo
```

---

## 4.5 — Modos de Inclusão Explicados

Observe os parâmetros `inclusion` e `fileMatchPattern` no cabeçalho (*frontmatter*) dos exemplos acima. O Steering permite quatro estratégias de carregamento para otimizar o uso do contexto:

### Sempre Ativo (`always` - padrão)

```yaml
---
inclusion: always
---
```

O arquivo é injetado em toda e qualquer interação. Recomendado para visões gerais de arquitetura e padrões corporativos indispensáveis (como os arquivos fundamentais).

### Condicional (`fileMatch`)

```yaml
---
inclusion: fileMatch
fileMatchPattern: "server/**/*.ts"
---
```

O arquivo é carregado apenas quando você estiver trabalhando em arquivos que correspondam ao padrão informado. O manual de rotas da API só entrará no contexto quando você estiver editando o backend — sem poluir a memória quando estiver mexendo no CSS do frontend.

### Manual (`manual`)

```yaml
---
inclusion: manual
---
```

Disponível sob demanda: você pode chamá-lo no chat digitando `#nome-do-steering` ou selecionando-o pelo menu de comandos `/`. Indicado para procedimentos esporádicos, como manuais de deploy ou rotinas de migração de dados.

### Automático (`auto`)

```yaml
---
inclusion: auto
name: equilibrio-do-jogo
description: Diretrizes sobre regras e balanceamento de partidas. Utilize ao alterar a lógica de turnos ou critérios de vitória.
---
```

Injetado de forma dinâmica sempre que a sua mensagem tiver relação semântica com a descrição informada — funcionando de maneira similar às Skills.

### Por que isso é relevante?

Sem políticas de inclusão, todos os manuais técnicos seriam anexados a todas as mensagens. Em um sistema com 15 documentos cobrindo testes, deploy, segurança e acessibilidade, a janela de contexto se esgotaria rapidamente. Os modos de inclusão mantêm o contexto limpo e relevante para a tarefa atual.

---

## 4.6 — Veja as Diretrizes em Ação

Vamos validar o funcionamento do Steering na prática. Abra uma conversa no modo Vibe e solicite a criação de uma rota:

```text
Adicione um novo endpoint para consultar uma partida específica pelo seu identificador (ID).
Siga rigorosamente as convenções de API estabelecidas.
```

### O que observar

Como o arquivo `api-conventions.md` está configurado com `fileMatch` para a rota `server/**/*.ts`, ele é incluído automaticamente assim que o Kiro manipula o backend. Você verá o Kiro:

- Definir a rota no padrão `/api/v1/games/:id` (evitando URLs não padronizadas como `/getGame`)
- Retornar o JSON na estrutura padrão `{ success, data, error }`
- Nomear a função como `getGame` (seguindo a regra `get<Recurso>`)
- Empregar os códigos HTTP corretos (200 para sucesso e 404 caso não localize)
- Salvar o arquivo respeitando a padronização existente

Se o Kiro não seguir alguma regra, isso representa uma oportunidade de melhoria: torne a instrução no arquivo de Steering mais objetiva e repita o teste.

---

## 4.7 — Referências Cruzadas de Arquivos

Arquivos de Steering podem referenciar outros arquivos do seu workspace usando uma sintaxe especial:

```markdown
#[[file:api/openapi.yaml]]
```

Isso instrui o Kiro a carregar o conteúdo daquele arquivo referenciado junto com o documento de Steering. É muito útil para:

- Apontar um contrato OpenAPI para que as rotas sigam a especificação
- Indicar um componente como modelo de referência para criação de novos componentes
- Conectar arquivos de configuração com parâmetros do projeto

Exemplo de uso dentro de `api-conventions.md`:

```markdown
Consulte o arquivo existente de rotas como exemplo de padrão: #[[file:server/routes/games.routes.ts]]
```

Dessa forma, o Kiro visualiza tanto as regras quanto um exemplo prático de implementação no mesmo contexto.

---

## 4.8 — Suporte ao Padrão AGENTS.md

Nota relevante: o Kiro suporta nativamente a especificação aberta `AGENTS.md`. Caso você já utilize um arquivo `AGENTS.md` na raiz do seu repositório (comum em fluxos com Claude Code ou outros agentes), o Kiro o reconhece e o carrega de forma contínua, sem necessidade de configurações adicionais.

Isso viabiliza manter convenções unificadas entre múltiplas ferramentas de IA — usando `AGENTS.md` para padrões gerais e `.kiro/steering/` para fluxos específicos do Kiro.

---

## 4.9 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Steering Fundamental** | Gerou automaticamente os documentos product.md, tech.md e structure.md |
| **Arquivos Personalizados** | Escreveu regras para a API e padronizou o desenvolvimento em React |
| **Modos de Inclusão** | Configurou políticas `always` (geral) e `fileMatch` (condicional por arquivos) |
| **Referências a Arquivos** | Vinculou trechos de código do projeto diretamente dentro das instruções |
| **AGENTS.md** | Identificou a compatibilidade com o padrão aberto entre agentes |
| **Steering na Prática** | Criou um novo endpoint que seguiu os padrões arquiteturais de forma automática |

---

## Ponto Principal

As Skills fornecem ao Kiro conhecimento técnico geral. O Steering ensina as diretrizes específicas da *sua* aplicação. Em conjunto, garantem que a IA entregue código tecnicamente consistente e integrado à arquitetura do seu projeto.

Os arquivos fundamentais são gerados em segundos, e redigir regras personalizadas leva poucos minutos. O ganho é ter todas as próximas implementações alinhadas às suas convenções de forma automática.

---

## Próximos Passos

Você já estabeleceu a documentação técnica do projeto com o Steering. No próximo passo, você implementará automações com **Hooks** — configurando gatilhos automáticos disparados ao salvar arquivos, usar ferramentas ou finalizar tarefas de desenvolvimento.
