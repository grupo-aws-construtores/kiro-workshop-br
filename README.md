# Workshop Kiro

Um workshop prático (*hands-on*) que guia você por toda a plataforma Kiro criando, expandindo e testando um aplicativo de jogo da velha (*tic-tac-toe*). Você começa do zero — apenas com uma pasta vazia e uma imagem — e finaliza com uma aplicação full-stack testada, além de especificações (*specs*), *skills*, diretrizes (*steering*), automações (*hooks*), integrações (*powers*) e um fluxo de trabalho com agentes via terminal.

Este repositório contém os roteiros do workshop. O aplicativo do jogo da velha em si é construído durante o workshop, utilizando o Kiro.

## Pré-requisitos

- [Kiro](https://kiro.dev/download?trk=7fcac8e0-008e-4fe0-8e3d-f72d7381e919&sc_channel=el/) instalado (IDE, ou a CLI para o Passo 8)
- Node.js e npm instalados (para a base em Vite + React + TypeScript e o backend em Express)
- Uma conta no Kiro autenticada (via Google, GitHub, Builder ID ou IAM Identity Center)
- Um quadro branco (físico ou digital) para esboçar as funcionalidades esperadas do jogo da velha antes do Passo 1. Desenhe o tabuleiro, os marcadores X e O, indicador de turno, detecção de vitória/empate e um botão de reiniciar. Tire uma foto dele; essa imagem se tornará sua especificação inicial no Passo 1.

## Como Usar

Siga os passos em ordem sequencial. Cada arquivo dentro de `steps/` é autocontido e termina com uma recapitulação e o direcionamento para o próximo passo.

Comece por aqui: [`steps/1_kicking_things_off.md`](steps/1_kicking_things_off.md)

## Passos do Workshop

| #   | Passo                                                                     | Foco                                                                                                                             |
| --- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [Dando o Pontapé Inicial](steps/1_kicking_things_off.md)                  | Tour pela IDE do Kiro, uso do modo Vibe e Autopilot, estruturação (*scaffold*) do app React + Vite + TS a partir de uma imagem |
| 2   | [Desenvolvimento Orientado a Especificações](steps/2_spec_driven_development.md) | Criar uma Spec e navegar pelo fluxo de requisitos → design → tarefas para adicionar backend em Node.js + Express                 |
| 3   | [Adicionando Skills](steps/3_adding_skills.md)                            | Instalar a *skill* de design frontend da Anthropic e de boas práticas em React da Vercel para redesenhar a interface           |
| 4   | [Steering (Diretrizes e Convenções)](steps/4_steering.md)                 | Gerar diretrizes essenciais (produto, tecnologia, estrutura), definir convenções customizadas e modos de inclusão                |
| 5   | [Hooks (Automações)](steps/5_hooks.md)                                    | Automatizar rotinas com hooks `fileEdited`, `preToolUse` e `postTaskExecution` para builds e revisão de acessibilidade           |
| 6   | [Powers (Integrações e Infra)](steps/6_powers.md)                         | Instalar uma Power (Netlify, Amplify, Supabase, CDK ou outra) para deploy, dados ou infraestrutura                                |
| 7   | [MCP e Testes Automatizados](steps/7_mcp_and_testing.md)                  | Configurar o servidor MCP do Playwright e fazer o Kiro jogar e validar a aplicação em um navegador real                           |
| 8   | [Kiro CLI (Linha de Comando)](steps/8_kiro_cli.md)                        | Operar o Kiro pelo terminal: chat interativo, tradução de comandos shell, sessões, agentes customizados e gestão de MCP        |

## O que Você Vai Construir

Ao término do workshop, você terá:

- Um aplicativo web de jogo da velha (React + Vite + TypeScript) gerado a partir de uma imagem
- Um backend em Node.js + Express com endpoints para resultados de partidas, placar (*leaderboard*) e histórico
- Uma especificação do Kiro em `.kiro/specs/` documentando requisitos, design e tarefas
- Arquivos de diretrizes em `.kiro/steering/` definindo as convenções técnicas do seu projeto
- Automações em `.kiro/hooks/` executando builds e validações de código
- Uma ou mais *Powers* instaladas para deploy ou banco de dados
- O servidor MCP do Playwright configurado em `.kiro/settings/mcp.json` para testes automatizados no navegador

## Estrutura do Repositório

```text
.
├── README.md
├── .gitignore
└── steps/
    ├── 1_kicking_things_off.md
    ├── 2_spec_driven_development.md
    ├── 3_adding_skills.md
    ├── 4_steering.md
    ├── 5_hooks.md
    ├── 6_powers.md
    ├── 7_mcp_and_testing.md
    └── 8_kiro_cli.md

```

Arquivos e pastas gerados durante a execução do workshop (node_modules/, dist/, .vite/, .playwright-mcp/, etc.) já estão cobertos pelo .gitignore.
