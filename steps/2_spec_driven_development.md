# Passo 2 — Desenvolvimento Orientado a Especificações

> **Objetivo:** Apresentar as Specs como a abordagem estruturada do Kiro para criar funcionalidades. Criar uma spec para adicionar um backend ao jogo da velha, navegar pelo fluxo de trabalho de três fases e executar as tarefas.

---

## 2.1 — Por que usar Specs?

No Passo 1 você usou o modo Vibe. Você entregou uma imagem para o Kiro e disse "construa isso" — e funcionou muito bem para estruturar a base. Mas o que acontece quando a funcionalidade se torna mais complexa?

- Quais são exatamente os requisitos?
- Como a API de backend deve ser estruturada?
- Como tratar erros, casos extremos (*edge cases*) e modelos de dados?
- Como acompanhar o progresso em múltiplas tarefas sequenciais?

É aqui que entra o **Desenvolvimento Orientado a Especificações** (*Spec-driven development*). Em vez de partir direto para o código, o Kiro ajuda você a planejar a funcionalidade primeiro — para então construí-la de forma sistemática.

### Vibe vs Spec: Quando usar cada um

| | Vibe | Spec |
| :--- | :--- | :--- |
| **Estrutura** | Conversa livre e aberta | Fluxo estruturado em três fases |
| **Artefatos gerados** | Apenas código direto | `requirements.md` → `design.md` → `tasks.md` |
| **Ideal para** | Protótipos rápidos, mudanças simples, Q&A | Funcionalidades complexas, trabalho em equipe, código para produção |
| **Acompanhamento** | Sem rastreamento formal | Progresso tarefa por tarefa com atualizações em tempo real |

Pense da seguinte maneira: **O Vibe é o rascunho no quadro branco. A Spec é a planta baixa arquitetônica.**

---

## 2.2 — O que Você Vai Construir

Seu jogo da velha funciona, mas roda puramente no navegador do cliente (*client-side*). Vamos adicionar um backend para que seja possível:

- **Salvar resultados das partidas**: Registrar quem venceu, quem perdeu e os empates
- **API de Leaderboard**: Fornecer estatísticas consolidadas de vitórias/derrotas
- **Histórico de partidas**: Armazenar e consultar jogos anteriores

Esse é um exemplo perfeito para usar uma Spec, pois envolve múltiplas camadas (design de API, modelo de dados, integração no frontend) e possui requisitos claros que podem ser definidos com antecedência.

---

## 2.3 — Criando uma Spec

### Como começar

Existem duas maneiras de inicializar uma Spec:

1. **Pelo painel do Kiro** — Clique no botão `+` dentro da seção **Specs** na barra lateral.
2. **Pelo chat** — Abra uma nova conversa e, no seletor de tipo de sessão, escolha **Spec** em vez de **Vibe**.

Em ambos os casos, o Kiro solicitará três definições:

1. **Descreva o que deseja fazer** — Este é o seu prompt inicial
2. **Funcionalidade (*Feature*) ou Correção (*Bug*)?** — Selecione **Feature**, pois estamos criando algo novo
3. **Requisitos (*Requirements*) ou Design Técnico (*Technical Design*)?** — Escolha **Requirements**, pois queremos levantar os requisitos primeiro

### O prompt

Digite algo como:

```text
Adicione um backend ao jogo da velha.
Quero uma API em Node.js com Express que:
- Salve o resultado dos jogos (vencedor, jogadores, jogadas, data/hora)
- Forneça um endpoint de leaderboard com estatísticas de vitórias, derrotas e empates
- Forneça um endpoint de histórico para consultar partidas anteriores
- Utilize armazenamento em memória por enquanto (sem necessidade de banco de dados externo)
O frontend em React deve exibir o leaderboard e o histórico de jogos.
```

Envie o prompt. Confirme que se trata de uma **Feature** e que deseja iniciar pela etapa de **Requirements**. Em seguida, acompanhe o processo.

---

## 2.4 — Fase 1: Requisitos (Requirements)

O Kiro gera o arquivo `requirements.md` dentro de `.kiro/specs/<nome-da-spec>/`.

Esse arquivo contém **histórias de usuário** (*user stories*) com **critérios de aceitação** definidos em formato estruturado. Você verá itens como:

- **Como jogador**, quero que meus resultados sejam salvos automaticamente para que eu possa acompanhar meu desempenho.
- **Como jogador**, quero visualizar um leaderboard para poder comparar minhas estatísticas com outros jogadores.
- **Como jogador**, quero consultar o histórico de partidas para revisar jogos anteriores.

Cada história de usuário conta com critérios de aceitação claros — condições mensuráveis que definem quando o item está "pronto".

### Pontos para prestar atenção

- O Kiro não se limitou a repetir o seu prompt: ele **expandiu** a ideia em requisitos técnicos estruturados com casos de borda que talvez você não tivesse previsto.
- O padrão segue a sintaxe EARS (*Easy Approach to Requirements Syntax*).
- Você pode **editar os requisitos diretamente**: adicionar novas histórias, remover itens indesejados e ajustar critérios.
- Quando estiver satisfeito, ordene que o Kiro avance para a fase de design.

### Refinando requisitos pela conversa

O processo é colaborativo. Caso falte algo, basta instruir no chat:

```text
Adicione um requisito para zerar/reiniciar o leaderboard.
Além disso, o histórico de partidas deve conter o estado completo do tabuleiro em cada jogada, e não apenas o placar final.
```

O Kiro atualiza o arquivo de requisitos. Quando estiver de acordo, abra o `requirements.md`, escolha **Continue** e selecione **Generate Design**.

---

## 2.5 — Fase 2: Design

O Kiro gera o arquivo `design.md` — detalhando a arquitetura técnica para implementar os requisitos definidos.

Geralmente, o documento inclui:

- **Arquitetura do sistema**: Como frontend e backend se comunicam
- **Design da API**: Endpoints, formatos de requisição/resposta e códigos HTTP
- **Modelo de dados**: Como os objetos de partida e dados do leaderboard serão estruturados
- **Diagramas de sequência**: Fluxo de persistência da partida e consulta do leaderboard
- **Tratamento de erros**: Comportamento esperado em casos de falha
- **Estratégia de testes**: O que testar e qual metodologia utilizar

### Pontos para prestar atenção

- O documento de design faz referência direta aos requisitos, garantindo rastreabilidade entre o que foi planejado e como será implementado.
- Os diagramas de sequência utilizam a sintaxe Mermaid, renderizados visualmente pela IDE.
- A API fica formalmente desenhada com contratos de endpoint antes de qualquer linha de código ser escrita.
- Você pode continuar **iterando**: "Adicione paginação ao endpoint de histórico" ou "Ajuste a estrutura dos objetos retornados".

Quando estiver tudo alinhado, abra o `design.md`, clique em **Continue** e escolha **Generate Tasks**.

---

## 2.6 — Fase 3: Tarefas (Tasks)

O Kiro gera o arquivo `tasks.md` — contendo a lista de tarefas discretas e acionáveis de implementação.

Cada tarefa é:

- **Específica**: "Criar o servidor Express com o endpoint POST de resultados"
- **Ordenada**: Respeita a ordem de dependência (backend preparado antes da integração com frontend)
- **Rastreável**: Atualiza o status em tempo real (*not started* → *in progress* → *completed*)

### Executando as tarefas

Você tem dois caminhos:

1. **Executar todas as tarefas (*Run all tasks*)** — Clique no botão de play e o Kiro executará cada uma sequencialmente.
2. **Executar uma por vez (*Run one at a time*)** — Clique na tarefa individual para executá-la pontualmente.

### Modo Supervisionado para execução de tarefas

Este é o momento ideal para **alternar para o modo Supervisionado**. Para isso, basta desativar o botão do **Autopilot** no painel de chat. Pronto: você está no modo supervisionado.

Com o modo supervisionado ativo, execute uma tarefa. O Kiro irá:

1. Escrever o código referente àquela tarefa
2. Pausar e exibir as alterações em blocos de código (*hunks*)
3. Aguardar você **aceitar** (*accept*), **rejeitar** (*reject*) ou **discutir** (*discuss*) cada bloco

Isso garante controle minucioso. Quer aceitar a rota da API, mas rejeitar a forma como ele tratou o erro? Você pode. Quer discutir uma linha específica? Basta clicar em "Chat inline" naquele trecho.

### O que observar durante a execução

- Acompanhe a mudança de status das tarefas em tempo real no painel de Specs.
- Verifique o diff: o Kiro cria os arquivos de backend, atualiza o frontend e adiciona as chamadas de rede.
- Se uma tarefa falhar ou produzir um comportamento inesperado, avise no chat para ele recalcular a rota.
- As tarefas seguem à risca o documento de design planejado.

### Pontos de Restauração (Checkpoints): Sua Rede de Segurança

Conforme executa tarefas, o Kiro cria **checkpoints** — instantâneos (*snapshots*) do estado do projeto aos quais você pode retornar a qualquer momento. Se uma tarefa quebrar o código ou produzir um resultado ruim:

1. Clique em **Restore** para voltar ao ponto imediatamente anterior àquela tarefa
2. Isso desfaz tanto as alterações nos arquivos quanto o contexto adicionado naquela interação
3. Execute a tarefa novamente ou dê novas orientações ao Kiro

Diferente de um simples `git checkout`, os checkpoints restauram também o histórico de contexto do chat. Pense nisso como um ponto de salvamento em um jogo: se a batalha der errado, recarregue e tente outra abordagem.

**Faça o teste:** Após concluir uma tarefa, peça intencionalmente uma alteração ruim ("reescreva a API para usar XML em vez de JSON"). Em seguida, reverta para o checkpoint e veja o projeto voltar exatamente ao estado anterior.

Depois do teste, você pode reativar o **Autopilot** e selecionar **Run all Tasks**.

---

## 2.7 — Os Três Arquivos

Ao final do ciclo, você terá três artefatos salvos em `.kiro/specs/<nome-da-spec>/`:

```
.kiro/specs/tic-tac-toe-backend/
├── requirements.md    # O que está sendo construído (histórias de usuário + critérios)
├── design.md          # Como será construído (arquitetura + API + modelos)
└── tasks.md           # Plano de implementação passo a passo (com controle de status)
```

Eles não são mensagens descartáveis de chat: são **documentos vivos** que:

- Documentam tecnicamente a funcionalidade
- Podem ser versionados e revisados por outros desenvolvedores
- Oferecem contexto histórico ("por que decidimos construir dessa forma?")
- Podem ser referenciados no chat a qualquer momento pelo marcador de contexto `#spec`

---

## 2.8 — Executar e Validar

Com todas as tarefas concluídas:

1. Inicie o servidor backend: `npm run dev` (ou conforme configurado no projeto)
2. Jogue algumas partidas no navegador
3. Abra a tela de leaderboard — suas vitórias e derrotas devem estar computadas
4. Abra o histórico de partidas — os jogos anteriores devem ser listados

Se algo falhar, use `#terminal` no chat para enviar a mensagem de erro diretamente ao Kiro e peça para ele solucionar.

---

## 2.9 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Sessões Spec** | Criou uma especificação completa dividida em três fases |
| **Fase de Requisitos** | Gerou histórias de usuário com critérios de aceitação a partir de um prompt |
| **Fase de Design** | Definiu arquitetura, contratos de API, diagramas e modelos de dados |
| **Fase de Tarefas** | Desmembrou o trabalho em tarefas práticas com status em tempo real |
| **Execução de Tarefas** | Executou rotinas alternando entre Autopilot e Modo Supervisionado |
| **Modo Supervisionado** | Avaliou mudanças de código bloco a bloco antes de aplicar |
| **Provedores `#`** | Usou `#terminal` para compartilhar erros; `#file`, `#folder` e `#problems` para alimentar o chat |
| **Refinamento Iterativo** | Ajustou requisitos e arquitetura antes de escrever código |
| **Checkpoints** | Reverteu o projeto com segurança quando uma tarefa gerou resultados indesejados |

---

## Ponto Principal

O modo Vibe é ágil e divertido — ótimo para começar. Mas quando a funcionalidade é crítica, as Specs oferecem a estrutura necessária para planejar antes de construir. Requisitos evitam regras esquecidas. Documentos de design impedem erros arquiteturais. Tarefas entregam um caminho previsível do plano ao código.

O melhor cenário é combinar ambos: use **Vibe para explorar** e **Spec para construir**.

---

## Próximos Passos

Agora você tem um jogo da velha full-stack com backend operacional. Na próxima etapa, você aprenderá a ensinar ao Kiro os padrões e convenções da comunidade usando **Skills** — pacotes portáteis de instruções que refinam a forma como a IA escreve código.
