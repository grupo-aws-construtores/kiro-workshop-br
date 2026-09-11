# Passo 6 — Powers (Integrações e Infraestrutura)

> **Objetivo:** Expandir as capacidades do Kiro com Powers — pacotes selecionados de ferramentas que conectam o agente a serviços externos. Instalar uma Power, utilizá-la em um caso prático com o jogo da velha e entender como elas se ativam dinamicamente com base no contexto.

---

## 6.1 — Por que usar Powers?

Até este ponto, todas as operações foram puramente locais — o Kiro leu arquivos, gerou código e executou comandos na sua máquina. Contudo, aplicações reais dependem de serviços de nuvem: plataformas de hospedagem, bancos de dados gerenciados, ferramentas de observabilidade e gateways de pagamento.

Você *poderia* configurar servidores MCP manuais para cada um desses serviços. Mas isso geraria dois entraves:

1. **Sobrecarga de contexto**: Cinco servidores MCP podem carregar mais de 100 definições de ferramentas a cada conversa, ocupando até 40% da sua janela de contexto antes mesmo da primeira instrução.
2. **Ausência de conhecimento especializado**: O Kiro ganha as ferramentas, mas não necessariamente as melhores práticas. Ele consegue chamar a API do Stripe, mas pode não saber que precisa configurar chaves de idempotência.

As Powers solucionam ambos os problemas agregando:

- **Configuração de servidor MCP**: As ferramentas e os parâmetros de conexão necessários
- **`POWER.md`**: Diretrizes de Steering que ensinam ao Kiro *como* usar essas ferramentas da forma correta
- **Hooks e Steering opcionais**: Automações e fluxos de trabalho guiados

Elas são carregadas de maneira **dinâmica**: ativam-se somente quando a sua conversa cita palavras-chave pertinentes. Ao falar de "deploy", a Power de implantação entra em ação. Ao migrar para "banco de dados", a Power correspondente assume, liberando o contexto anterior. Tudo sem chaves manuais.

---

## 6.2 — O Ecossistema de Powers

O Kiro possui um catálogo crescente de Powers homologadas. Veja alguns exemplos divididos por categoria:

### Implantação e Infraestrutura (Deployment & Infrastructure)

| Power | Provedor | O que faz |
| :--- | :--- | :--- |
| **Deploy with Netlify** | Netlify | Faz deploy de aplicações React/Next.js/Vue para a CDN global da Netlify |
| **AWS Infrastructure as Code** | AWS | Constrói infraestrutura com CDK e CloudFormation |
| **AWS Amplify** | AWS | Cria aplicações full-stack com autenticação, dados, storage e funções |
| **Terraform** | HashiCorp | Provisiona infraestrutura como código utilizando Terraform |
| **AWS SAM** | AWS | Modela e implementa aplicações serverless com AWS SAM |
| **ECS Express Mode** | Comunidade | Faz deploy de containers no AWS ECS com terminação HTTPS |

### Bancos de Dados (Databases)

| Power | Provedor | O que faz |
| :--- | :--- | :--- |
| **Supabase (hosted)** | Supabase | Fornece Postgres em nuvem, autenticação, storage e assinaturas em tempo real |
| **Supabase (local)** | Supabase | Sobe ambiente local de desenvolvimento com Supabase |
| **Neon** | Neon | Entrega Postgres serverless com branching e redução de consumo a zero |
| **Aurora PostgreSQL** | AWS | Aplica as melhores práticas de arquitetura específicas para o Amazon Aurora |
| **ClickHouse** | ClickHouse | Gerencia bancos de dados voltados para processamento analítico |

### Design e Frontend

| Power | Provedor | O que faz |
| :--- | :--- | :--- |
| **Figma** | Figma | Converte designs e telas do Figma em código pronto para produção |
| **Miro** | Miro | Utiliza quadros do Miro como base de requisitos e arquitetura |
| **Bria AI** | Bria | Gera, edita e remove fundos de imagens utilizando inteligência artificial |

### Observabilidade e Segurança

| Power | Provedor | O que faz |
| :--- | :--- | :--- |
| **Datadog** | Datadog | Consulta logs, métricas e rastreamentos de telemetria para depuração |
| **Snyk** | Snyk | Realiza varreduras de segurança e auxilia na mitigação de vulnerabilidades |
| **AWS Observability** | AWS | Integração com CloudWatch, CloudTrail e Application Signals |

### Pagamentos

| Power | Provedor | O que faz |
| :--- | :--- | :--- |
| **Stripe** | Stripe | Gerencia pagamentos, faturamento recorrente e assinaturas |
| **Checkout.com** | Checkout.com | Integração global de APIs para meios de pagamento |

---

## 6.3 — Instalando uma Power

As Powers podem ser instaladas com um clique — sem necessidade de lidar com arquivos JSON complexos ou parâmetros manuais de CLI.

### Pelo painel do Kiro

1. Abra a seção **Powers** no painel lateral do Kiro (ou use `Cmd+Shift+P` → "Kiro: Configure Powers").
2. Navegue pelas opções disponíveis no catálogo.
3. Clique em **Install** na Power desejada.
4. O Kiro configura a conexão do servidor MCP de forma automática.

### Pelo portal kiro.dev

1. Acesse o catálogo em [kiro.dev/powers](https://kiro.dev/powers/).
2. Localize a integração que procura.
3. Clique em **Install** — seu navegador solicitará abertura no Kiro para concluir a instalação.

---

## 6.4 — Prática: Escolha um Caminho

Você já possui o jogo da velha com backend integrado. Agora, escolha uma direção prática para utilizar uma Power:

### Opção A: Deploy na Netlify

Instale a Power **Netlify** e solicite no chat do Kiro:

```text
Faça o deploy do jogo da velha na Netlify.
Configure os parâmetros de build necessários e forneça a URL pública do projeto.
```

O Kiro irá:

- Ativar a Power da Netlify (identificada pelos termos "deploy" e "Netlify")
- Acionar as ferramentas MCP da Netlify para registrar o projeto e compilar
- Adotar as práticas recomendadas contidas no arquivo `POWER.md`
- Disponibilizar o link funcional no final

### Opção B: Deploy na AWS via AWS Amplify

Instale a Power **AWS Amplify** e solicite:

```text
Faça o deploy do jogo da velha na AWS utilizando o Amplify.
Configure o hosting do frontend e o backend serverless.
```

O Kiro irá:

- Ativar a Power do Amplify
- Inicializar o Amplify Gen 2 com TypeScript
- Provisionar hospedagem, funções de backend e modelagem de dados
- Publicar a aplicação na sua infraestrutura da AWS

### Opção C: Adicionar Banco de Dados com Supabase

Instale a Power **Supabase** e peça:

```text
Substitua o armazenamento em memória pelo Supabase.
Crie um banco de dados Postgres para persistir os resultados das partidas e o placar.
Utilize a autenticação do Supabase para o login dos jogadores.
```

O Kiro irá:

- Ativar a Power do Supabase
- Criar a modelagem das tabelas de jogos e participantes
- Configurar as políticas de segurança a nível de linha (*Row Level Security*)
- Integrar o cliente do Supabase no frontend React
- Atualizar os controladores Express para consultar o Postgres

### Opção D: Infraestrutura na AWS com AWS CDK

Instale a Power **AWS Infrastructure as Code** e envie:

```text
Crie uma stack de CDK para implantar o backend do jogo da velha como uma função Lambda
integrada a um API Gateway, utilizando uma tabela do DynamoDB para persistência.
```

O Kiro irá:

- Ativar a Power de AWS IaC
- Elaborar a stack em CDK contemplando Lambda, API Gateway e DynamoDB
- Aplicar os princípios do AWS Well-Architected Framework
- Validar a sintaxe do template CloudFormation gerado

### Opção E: Outras Alternativas

O catálogo possui dezenas de integrações. Você pode testar análises de segurança com o Snyk, telemetria com Datadog ou geração visual com Bria AI de acordo com sua preferência.

---

## 6.5 — Como Funciona a Ativação Dinâmica

Após adicionar uma Power, você pode observar o gerenciamento de contexto em ação:

1. **Inicie uma conversa neutra** — "Explique a lógica de turnos no App.tsx". A Power permanece inativa e as ferramentas não consomem tokens.
2. **Cite o escopo da integração** — "Faça o deploy disso na Netlify". A Power entra em operação, carregando as ferramentas MCP e o manual `POWER.md`.
3. **Mude novamente de assunto** — "Adicione um novo modo de jogo". A Power é descarregada, liberando a janela de contexto.

Esse mecanismo diferencia as Powers de conexões MCP comuns, evitando a sobrecarga contínua de ferramentas que não serão usadas naquele momento.

---

## 6.6 — O que Compõe uma Power?

Estruturalmente, cada Power é um diretório contendo:

```
minha-power/
├── POWER.md           # Diretrizes: o que as ferramentas fazem, quando usá-las e boas práticas
├── mcp.json           # Configuração técnica do MCP (ferramentas e parâmetros de conexão)
└── steering/          # Opcional: roteiros complementares de fluxo de trabalho
    └── deployment.md
```

- **`POWER.md`**: Fornece o discernimento técnico — ensina ao Kiro quando usar cada operação, boas práticas e a ordem correta de execução.
- **`mcp.json`**: Fornece a capacidade operacional — define o servidor MCP que expõe os métodos de chamada.

---

## 6.7 — Comparativo: Powers vs Skills vs Steering vs MCP

Veja como os quatro sistemas de conhecimento se posicionam:

| | Powers | Skills | Steering | MCP Manual |
| :--- | :--- | :--- | :--- | :--- |
| **Definição** | Ferramentas + conhecimento técnico | Manuais de instrução portáteis | Diretrizes do seu projeto | Servidores de ferramentas |
| **Carregamento** | Dinâmico (palavras-chave) | Sob demanda (descrição do prompt) | Configurável (4 modos) | Sempre ativo no contexto |
| **Conteúdo** | MCP + diretrizes + automações | Orientações + scripts de apoio | Documentação em Markdown | Apenas ferramentas técnicas |
| **Ferramentas Externas** | Sim | Não | Não | Sim |
| **Boas Práticas** | Embutidas no pacote | Embutidas no pacote | Redigidas por você | Não inclusas |
| **Cenário Ideal** | Integração com plataformas e nuvem | Metodologias de desenvolvimento | Padrões do repositório | Ferramentas personalizadas |

Em resumo: **Steering** é a documentação interna da sua aplicação. **Skills** são os manuais da comunidade. **Powers** são integrações com provedores de nuvem acompanhadas de conhecimento especializado. **MCP Manual** é a solução para integrações customizadas que ainda não possuem pacote pronto.

---

## 6.8 — Recapitulação

| Recurso do Kiro | Como você utilizou |
| :--- | :--- |
| **Powers** | Instalou e utilizou integrações prontas para deploy, banco de dados ou infra |
| **Instalação Simplificada** | Instalou recursos com um clique a partir do painel do Kiro |
| **Ativação Dinâmica** | Acompanhou o carregamento sob demanda baseado nas palavras-chave do chat |
| **Integração MCP** | As Powers gerenciaram as conexões de ferramentas automaticamente |
| **Diretrizes POWER.md** | O Kiro seguiu as práticas recomendadas contidas na Power |
| **Catálogo de Powers** | Explorou o ecossistema de soluções disponíveis |

---

## Ponto Principal

As Powers conectam o Kiro ao ecossistema externo, garantindo que o agente utilize serviços de nuvem com precisão técnica. Graças ao carregamento sob demanda, você pode dispor de diversas integrações sem comprometer o limite de contexto da sua conversa.

---

## Próximos Passos

Você já explorou o modo Vibe, Specs, Skills, Steering, Hooks e Powers. No próximo passo, vamos direto para os testes práticos com o **Passo 7: MCP Manual e Testes com Playwright**, operando um navegador real através da IA.
