# Passo 1 — Dando o Pontapé Inicial

> **Objetivo:** Explorar a IDE do Kiro, apresentar o modo Vibe e estruturar (*scaffold*) um app de jogo da velha em React + Vite a partir de uma única imagem.

---

## 1.1 — Explore a IDE do Kiro

Ao abrir a pasta deste projeto no Kiro, você notará uma interface familiar: ela é baseada no Code OSS, a mesma base de código aberto do VS Code. Seus temas, atalhos de teclado e a maioria das extensões continuam funcionando normalmente.

Abra o painel do Kiro na barra lateral esquerda. Você verá algumas seções exclusivas do Kiro que não existem no VS Code padrão:

| Seção do Painel | O que faz |
| :--- | :--- |
| **Specs** | Artefatos estruturados de desenvolvimento (requisitos → design → tarefas) |
| **Agent Hooks** | Automações orientadas a eventos — gatilhos disparados ao salvar arquivos, executar ferramentas, etc. |
| **Steering** | Conhecimento persistente do projeto — arquivos Markdown que ensinam ao Kiro como o seu projeto funciona |
| **Agent Skills** | Pacotes portáteis de instruções que o Kiro ativa sob demanda |
| **MCP Servers** | Integrações de ferramentas externas via *Model Context Protocol* |

Você usará a maior parte desses recursos ao longo do workshop. Por enquanto, apenas localize onde cada um fica.

### Abra o Painel de Chat

Pressione `Cmd+L` (Mac) ou `Ctrl+L` (Windows/Linux) para abrir o painel de chat. É aqui que você conversa com o Kiro. Você escreve em linguagem natural e o Kiro lê, escreve e executa código em seu nome.

No topo do chat, observe dois controles:

1. **Seletor de tipo de sessão** — Escolha entre sessões **Vibe** (livre/conversacional) e **Spec** (estruturada).
2. **Seletor de modelo** — Escolha qual modelo de IA utilizar. A opção **Auto** é o padrão recomendado.

### Alternando Modelos

Para este workshop, mantenha a opção em **Auto**: ela direciona automaticamente para o modelo ideal a cada tarefa. Você pode testar alternar para um modelo mais econômico (como Haiku ou Qwen3) para uma dúvida simples e, em seguida, mudar para o Sonnet ou Opus para a geração da base do projeto. Observe como o consumo de créditos muda no seu painel de uso. A lógica é simples: você tem o controle do equilíbrio entre custo e qualidade por interação.

---

## 1.2 — Modo Vibe: Apenas Converse e Construa

Para este passo, utilize o **modo Vibe**. Trata-se da forma livre e conversacional de trabalhar com o Kiro — sem burocracia, sem formalidades e sem especificações prévias.

O modo Vibe é excelente para:

- Criar a base de novos projetos (*scaffolding*)
- Prototipagem rápida
- Fazer perguntas e tirar dúvidas sobre o código
- Programação exploratória sem a necessidade de um plano rígido

Mais adiante mudaremos para o modo Spec quando quisermos mais estrutura. Por enquanto, o modo Vibe é perfeito.

### Autopilot vs Supervisionado

Observe o seletor **Autopilot** dentro do painel de chat. Ele possui dois modos:

- **Autopilot** (padrão): O Kiro trabalha de forma autônoma. Cria arquivos, escreve código, executa comandos no terminal e toma decisões sem pedir autorização a cada etapa. Você pode inspecionar as alterações, revertê-las ou interromper o processo a qualquer momento.
- **Supervisionado** (*Supervised*): O Kiro pausa após cada conjunto de edições nos arquivos e exibe as alterações como blocos individuais (*hunks*). Você aceita, rejeita ou discute cada trecho antes que ele prossiga.

Para gerar a estrutura inicial, garanta que o **Autopilot** esteja ativado. Você usará o modo supervisionado mais tarde, quando for revisar alterações minuciosamente.

---

## 1.3 — O Ponto de Partida: Uma Imagem

Neste momento, a pasta do projeto quase não contém arquivos — apenas estes guias.

Encontre ou crie uma imagem daquilo que deseja construir. Pode ser uma captura de tela, um esboço feito no papel e fotografado, um wireframe ou qualquer referência visual. Como primeiro teste, use o jogo da velha (*tic-tac-toe*). Analise o esboço: ele exibe uma interface com grade 3×3, marcadores X e O, um indicador de turno informando de quem é a vez e um botão para reiniciar.

Essa imagem será toda a sua especificação nesta etapa: arraste-a para o chat do Kiro e peça para ele construir o que está vendo.

---

## 1.4 — Construindo o Aplicativo

1. Abra o painel de chat (`Cmd+L` ou `Ctrl+L`).
2. Confirme que está no modo **Vibe** com o **Autopilot** ativado.
3. Arraste sua imagem para a área de entrada do chat (ou clique no ícone de anexo).
4. Digite um prompt nos seguintes termos:

   ```text
   Crie um jogo da velha baseado no layout desta imagem.
   Utilize React com Vite e TypeScript.
   Implemente toda a lógica da partida — identificando vitórias, empates e alternando turnos entre X e O.
   Adicione um botão de reiniciar para começar um novo jogo.
   ```

5. Pressione Enter e acompanhe o Kiro trabalhar.

### O que esperar

O Kiro irá:

- Inicializar a base do projeto com Vite + React + TypeScript (`npm create vite@latest`)
- Construir o componente do tabuleiro com grade 3×3
- Implementar a lógica do jogo (checagem de vitória, alternância de turnos e detecção de empate)
- Adicionar o indicador de status e o botão de reinício
- Estilizar a aplicação para coincidir com a imagem enviada
- Instalar dependências e validar a compilação (*build*)

### Pontos para prestar atenção durante o processo

- **Compreensão visual**: O Kiro interpreta o layout visual da imagem e gera os componentes correspondentes.
- **Comandos no terminal**: Acompanhe a saída do terminal enquanto o Kiro executa `npm install`, `npm run build`, etc.
- **Criação de múltiplos arquivos**: Componentes, estilos e regras do jogo são criados em conjunto.
- **Visualização de alterações**: Clique em "View all changes" no chat para inspecionar o diff de tudo o que foi gerado.

---

## 1.5 — Executando a Aplicação

Assim que o Kiro concluir, inicie o servidor de desenvolvimento:

```bash
npm run dev
```

O Kiro também pode manter servidores de desenvolvimento ativos em segundo plano sem travar o chat. Se ele já iniciou o servidor durante o processo, você verá que o processo já está rodando — permitindo continuar conversando enquanto ele serve o app. Esse é o recurso **Dev Servers**: processos de longa execução que não bloqueiam sua interação.

Abra o endereço no seu navegador (normalmente `http://localhost:5173`) e jogue uma partida. Clique nos quadrados, veja a alternância de jogadores, confirme se a detecção de vitória/empate funciona e teste o botão de reiniciar.

### Se algo não estiver certo

Basta orientar o Kiro pelo chat. Por exemplo:

```text
O visual do tabuleiro precisa de ajustes — aumente as células e coloque bordas bem visíveis.
```

ou:

```text
A validação de vitória não está funcionando para alinhamentos na diagonal. Corrija isso.
```

O Kiro analisa o código existente, identifica o problema e aplica a solução. Esse é o ciclo conversacional do modo Vibe: descrever, construir e iterar.

---

## 1.6 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Interface da IDE** | Conheceu as abas exclusivas (Specs, Steering, Hooks, MCP, Skills) |
| **Modo Vibe** | Utilizou o chat conversacional para criar um aplicativo completo |
| **Entrada por imagem** | Arrastou uma foto para o chat como "especificação" inicial |
| **Autopilot** | Permitiu que o Kiro tomasse ações autônomas para estruturar o projeto inteiro |
| **Terminal integrado** | O Kiro rodou comandos npm para criar o scaffold, instalar pacotes e compilar |
| **Alternância de modelos** | Entendeu as vantagens de selecionar modelos conforme o custo e complexidade |
| **Dev servers** | Manteve o servidor local rodando em segundo plano sem travar o chat |

---

## Próximos Passos

Você já tem um jogo da velha funcionando perfeitamente. No próximo passo, você usará o **Desenvolvimento Orientado a Especificações (Spec-driven development)** para planejar e construir recursos de backend com maior rigor e organização técnica.
