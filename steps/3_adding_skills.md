# Passo 3 — Adicionando Skills

> **Objetivo:** Instalar skills da comunidade para elevar o nível das entregas do Kiro: a skill de design de frontend da Anthropic para uma interface elegante e a de boas práticas em React da Vercel para código em nível de produção. Em seguida, utilizá-las para redesenhar o jogo da velha.

---

## 3.1 — O que são Skills?

Skills são pacotes portáteis de instruções que seguem o padrão aberto [Agent Skills](https://agentskills.io/). Elas agregam ao Kiro conhecimentos especializados que não vêm de fábrica — diretrizes avançadas de design, boas práticas de frameworks, rotinas de deploy, entre outros.

O conceito central é a **revelação progressiva** (*progressive disclosure*):

1. **Descoberta (*Discovery*)** — Ao inicializar, o Kiro carrega apenas o nome e a descrição de cada skill instalada (operação leve).
2. **Ativação (*Activation*)** — Quando o seu pedido coincide com a descrição de uma skill, o Kiro carrega o conjunto completo de instruções no contexto.
3. **Execução (*Execution*)** — O Kiro segue as diretrizes, invocando scripts ou arquivos de referência apenas quando necessário.

Graças a esse modelo, você pode ter dezenas de skills instaladas sem estourar o limite de contexto de cada conversa. O Kiro carrega apenas o que é estritamente relevante para aquela requisição.

### Onde as skills ficam armazenadas

| Escopo | Localização | Cenário de uso |
| :--- | :--- | :--- |
| **Workspace** | `.kiro/skills/` | Fluxos específicos para este projeto |
| **Global** | `~/.kiro/skills/` | Padrões de trabalho compartilhados entre todos os seus projetos |

### Como utilizá-las

As skills são ativadas **automaticamente** assim que o seu prompt demanda aquele tipo de conhecimento. Você também pode explicitar o domínio da skill no seu texto (por exemplo: "siga as boas práticas de React" ou "use a skill de frontend design").

---

## 3.2 — As Duas Skills que Você Vai Instalar

### 1. Skill de Design de Frontend da Anthropic

**O que faz:** Orienta o Kiro a criar interfaces modernas e com acabamento de produção, evitando layouts genéricos e com cara de template automatizado. Estimula decisões sólidas de design — tipografia de impacto, esquemas de cores coesos, animações, composição espacial e cuidado com texturas.

**Por que você deve usar:** O jogo da velha atual provavelmente tem um visual comum. Esta skill força o Kiro a pensar como um designer de produto antes de sair escrevendo CSS — definindo uma identidade visual, escolhendo fontes adequadas e criando atmosfera.

**Diretrizes que ela ensina ao Kiro:**

- Refletir sobre propósito, tom e diferenciação da interface antes de programar
- Escolher tipografias autênticas (nada de padrões saturados como Inter, Roboto ou Arial)
- Definir uma paleta consistente com cores dominantes e contrastes bem marcados
- Inserir dinamismo — animações de carregamento, estados de hover e microinterações
- Criar profundidade através de texturas, gradientes e sombras
- Evitar designs padronizados e previsíveis

### 2. Skill de Boas Práticas em React da Vercel

**O que faz:** Um guia aprofundado de otimização de performance contendo 70 regras divididas em 8 categorias, mantido pela equipe de engenharia da Vercel. Aborda desde a eliminação de cascatas de requisições (*waterfalls*) até a redução do tamanho de bundle e controle de re-renderizações.

**Por que você deve usar:** O jogo da velha é simples, mas os padrões importam. Esta skill garante que o Kiro estruture os componentes como os engenheiros da Vercel recomendam — componentes desacoplados, gestão de estado limpa e ciclo de vida otimizado.

**Categorias fundamentais (por ordem de prioridade):**

| Prioridade | Categoria | Exemplos Práticos |
| :--- | :--- | :--- |
| CRÍTICA | Eliminação de Cascatas (*Waterfalls*) | `Promise.all()` para operações paralelas, adiar `await` para blocos específicos |
| CRÍTICA | Otimização de Pacotes (*Bundle Size*) | Importações diretas (evitar *barrel files*), importações dinâmicas para componentes pesados |
| ALTA | Performance no Servidor (*Server-Side*) | `React.cache()` para deduplicação, minimizar dados serializados enviados ao cliente |
| MÉDIA | Prevenção de Re-renderizações | `setState` funcional, cálculo de estado derivado durante a renderização, desacoplamento de hooks |
| MÉDIA | Performance de Renderização | `content-visibility` em listas, elevação de JSX estático, uso de ternários em vez de `&&` |

---

## 3.3 — Instalando as Skills

### Design de Frontend (via skills.sh)

Abra o terminal e execute:

```bash
npx skills add [https://github.com/anthropics/skills](https://github.com/anthropics/skills) --skill frontend-design
```

Ao ser solicitado, selecione o agente do **Kiro**.

Esse comando baixa a skill do repositório da Anthropic no GitHub e instala os arquivos dentro da pasta `.kiro/skills/`.

### Boas Práticas em React (da Vercel)

Execute no terminal:

```bash
npx skills add [https://github.com/vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) --skill vercel-react-best-practices
```

### Alternativa: Importação visual pelo painel do Kiro

Você também pode instalar diretamente pela interface gráfica:

1. Abra a seção **Agent Steering & Skills** no painel do Kiro.
2. Clique no ícone `+` e selecione **Import a skill**.
3. Escolha a opção **GitHub** e cole a URL do repositório.
4. Selecione a skill desejada para importação.

### Verificando a instalação

Após o procedimento, ambas as skills deverão aparecer listadas na barra lateral. Você também pode conferir diretamente na árvore de arquivos:

```
.kiro/skills/
├── frontend-design/
│   └── SKILL.md
└── vercel-react-best-practices/
    ├── SKILL.md
    ├── AGENTS.md
    └── rules/
        ├── async-parallel.md
        ├── bundle-barrel-imports.md
        └── ... (70 arquivos de regras)
```

---

## 3.4 — Usando as Skills: Redesenhe o Jogo

Agora coloque essas instruções para funcionar. Abra uma sessão no modo Vibe e envie:

```text
Redesenhe a interface do jogo da velha. Crie uma experiência visual incrível —
quero que a aplicação pareça um produto premium e moderno, não um projeto simples de tutorial.
Utilize as Skills instaladas.
Mantenha intactas todas as regras de jogo e as integrações existentes com o backend.
```

### O que observar

Com a skill **frontend-design** ativa, o Kiro irá:

- Iniciar por uma etapa de planejamento estético — alinhando conceito e estilo antes de gerar CSS
- Selecionar fontes tipográficas expressivas
- Definir uma paleta de cores harmoniosa através de variáveis CSS
- Aplicar transições — animação de abertura, colocação dos marcadores e efeito visual de vitória
- Adicionar profundidade com sombras suaves, gradientes e texturas
- Entregar um layout intencional e fluido

Com a skill **react-best-practices** ativa, o Kiro irá:

- Organizar a estrutura para evitar re-renderizações desnecessárias
- Adotar padrões recomendados de estado (`setState` funcional e estados calculados dinamicamente)
- Manter a hierarquia de componentes legível (sem criar componentes inline dentro de outros componentes)
- Empregar operadores ternários para renderização condicional em vez de sintaxes propensas a falhas como `&&`

### Compare os resultados

Se você tiver guardado um print de como o jogo estava antes, compare-o com a nova interface. A diferença é evidente: de uma demonstração básica de curso para uma aplicação com nível de acabamento profissional.

### Ferramentas Web em Ação (*Web Tools*)

Durante o redesign, o Kiro pode precisar consultar dados externos — buscar uma família tipográfica no Google Fonts, verificar técnicas de animação CSS ou buscar referências de paleta. Para isso, ele conta com **ferramentas web nativas** para realizar buscas na internet e extrair conteúdos de páginas em tempo real.

Faça um teste pedindo:

```text
Encontre uma combinação de fontes do Google Fonts com identidade retrofuturista para o jogo.
```

O Kiro realiza a busca na web, avalia as alternativas e aplica os estilos importados. Essa capacidade é útil sempre que você precisar de dados atualizados que vão além da base de treino do modelo.

---

## 3.5 — Como as Skills são Ativadas

A ativação das skills é **automática** por relevância semântica. Quando a descrição de uma skill (configurada no frontmatter do seu `SKILL.md`) corresponde ao objetivo do seu prompt, o Kiro carrega essas instruções para o contexto daquela conversa.

Exemplos práticos:

- Pedir para "redesenhar a interface" ou "melhorar o visual" aciona a skill **frontend-design**, pois suas especificações cobrem estilização, interfaces e acabamento visual.
- Escrever ou refatorar componentes aciona a skill **vercel-react-best-practices**, cujas diretrizes cobrem padrões de componentes, performance e refatoração em React.

Você não precisa de nenhum comando manual — basta descrever seu objetivo. Se quiser garantir a ativação, mencione o escopo explicitamente no prompt (ex.: "utilizando a skill de frontend design").

---

## 3.6 — Outras Formas de Enriquecer o Contexto do Kiro

As Skills representam apenas uma das formas de guiar o Kiro. As outras camadas incluem:

- **Skills**: Manuais de boas práticas da comunidade (o que instalamos agora). São ativadas por demanda conforme o contexto da conversa.
- **Steering**: Regras e convenções específicas do *seu* projeto criadas por você. Exemplos: "nossa API sempre devolve as respostas neste padrão" ou "utilize apenas componentes funcionais". Ficam em `.kiro/steering/` e serão vistas no **Passo 4**.
- **Powers**: Integrações com serviços de nuvem e ferramentas externas (hospedagem, bancos de dados, telemetria) acompanhadas de servidores MCP e instruções de melhores práticas. Veremos no **Passo 6**.

Além desses pilares, você pode alimentar o contexto de qualquer conversa pontualmente:

- Digite `#` no chat para vincular um **arquivo**, **pasta**, **saída do terminal**, **git diff** ou a lista de **problemas (*problems*)** da IDE
- Arraste e solte **imagens** ou **documentos** (PDF, DOCX) diretamente na caixa de entrada
- Use `#Problems` para passar diagnósticos de compilação ou linter para resolução imediata

O conhecimento do Kiro se constrói em camadas: diretrizes automáticas (skills, steering), dados explícitos apontados com `#` e ferramentas de integração (powers). Juntos, esses recursos mantêm a IA orientada sem exigir que você reexplique o projeto do zero a cada interação.

---

## 3.7 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Agent Skills** | Instalou pacotes da comunidade para expandir a capacidade técnica do Kiro |
| **skills.sh / npx skills** | Utilizou a linha de comando para instalar skills de repositórios do GitHub |
| **Revelação progressiva** | As skills foram carregadas de modo inteligente quando a solicitação demandou |
| **Ativação automática** | As instruções foram ativadas pela relevância do prompt sem comandos manuais |
| **Modo Vibe com Skills** | Redesenhou a interface aproveitando as regras ativas de UI e performance |
| **Ferramentas web** | O Kiro buscou fontes e estilos na internet durante o desenvolvimento |
| **Skills vs Steering vs Powers** | Compreendeu os papéis de cada sistema de conhecimento do ecossistema |

---

## Próximos Passos

Seu aplicativo agora conta com uma interface bonita e alinhada com as melhores práticas de desenvolvimento em React. No próximo passo, você configurará o **Steering** para ensinar ao Kiro as convenções exclusivas do seu projeto — para que qualquer implementação futura siga automaticamente os seus padrões técnicos.
