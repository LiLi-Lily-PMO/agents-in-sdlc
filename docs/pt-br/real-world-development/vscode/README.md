---
slug: pt-br/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** no VS Code leva o GitHub Copilot ao editor de código que você já usa. No Visual Studio Code (e no GitHub Codespaces), você trabalhará com o Copilot Chat no modo de agente, conectará ferramentas externas por meio do MCP e usará agentes personalizados — tudo sem sair do IDE, onde o Copilot tem acesso aos arquivos, ao terminal e aos problemas do projeto.

Você começará adicionando instruções personalizadas e observando o Copilot segui-las. Depois, usará o modo de agente para desenvolver uma funcionalidade de filtragem na interface, na camada de dados e nos testes. Em seguida, conectará o servidor MCP do Playwright e permitirá que o Copilot controle um navegador para testar o recurso antes de abrir um pull request. Por fim, revisará e usará um agente personalizado para acessibilidade e monitorará, orientará e iterará sobre as alterações do Copilot — tudo sem sair do editor.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Explore e atualize instruções e aplique-as a uma função auxiliar de acesso a dados |
| [2. Modo de agente][ex2] | Desenvolvimento de código | Desenvolva uma funcionalidade de filtragem com o modo de agente |
| [3. MCP com Playwright][ex3] | Ferramentas externas | Teste o recurso em um navegador com o servidor MCP do Playwright e abra um pull request |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revise e use um agente personalizado de acessibilidade |
| [5. Monitorar e gerenciar agentes][ex5] | Monitoramento | Acompanhe e oriente uma sessão de agente em andamento |
| [6. Iterar sobre o trabalho do Copilot][ex6] | Revisão | Publique suas alterações locais e revise o pull request |
| [Opcional: Integrar o Foundry][foundry-toolkit] | Agentes de IA | Prepare um modelo, implante um agente e conecte-o ao site em três módulos com o VS Code e o Foundry Toolkit |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] Acesso ao GitHub Codespaces

> [!TIP]
> Não tem um plano pago? Estudantes com verificação podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop, permitindo concluir todos os percursos.

[callout-student-plan-education]: https://github.com/education/students

## Começar

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
