---
title: "Exercício 6 - Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário. Usou o modo de agente para implementar a filtragem no cliente e no servidor, o servidor MCP do Playwright para testar o recurso manualmente no navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — opções de alto contraste e modo claro — e orientar a sessão em andamento para ampliar o trabalho. Agora, é hora de publicar essas alterações locais e revisá-las como a equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. Ainda é necessário revisar o trabalho produzido pelo Copilot. Vamos enviar as alterações de acessibilidade do codespace, abrir um pull request e revisar o diff antes de envolver o restante da equipe.

## Publicar os recursos de acessibilidade

As opções de alto contraste e modo claro implementadas com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no codespace como alterações com commit. Vamos enviá-las em uma branch e abrir um pull request para que o restante da equipe possa revisar.

1. Volte ao codespace.
2. No VS Code, abra a visualização **Source Control**.
3. Confirme se as alterações de acessibilidade estão em commits. Se houver alterações sem commit do Exercício 5, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite a solicitação ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro, como `Add high-contrast and light-mode toggles`, e uma breve descrição do que foi feito e por quê.
7. Selecione **Create pull request**.
8. Quando o PR estiver aberto, selecione a guia **Files changed** para revisar todo o trabalho. Preste atenção especial a:
   - componentes da interface para alternar entre os modos;
   - uso do armazenamento local do navegador para salvar as preferências do usuário;
   - alterações de CSS ou de estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado;
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

9. Volte à guia **Conversation**.
10. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows. Se tudo estiver correto, eles serão aprovados.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Marque `@copilot` em um comentário no PR com uma solicitação, como "revise este PR para identificar outros problemas de WCAG" ou "sugira melhorias para a navegação por teclado". O Copilot iniciará uma nova sessão para atender ao comentário.

## Exercício opcional: continuar explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade que se desenvolve com a prática. Veja algumas ideias para novas sessões no VS Code:

- Criar um formulário para manifestar interesse em apoiar o jogo na página de detalhes.
- Implementar paginação na página de listagem de jogos.
- Adicionar validação de entrada e tratamento de erros às funções auxiliares de acesso a dados em `src/lib/`.
- Ampliar o escopo do agente de acessibilidade, por exemplo, para auditar a ordem de foco do teclado em todo o site.

## Resumo

Parabéns. Você concluiu o percurso do VS Code. Neste laboratório, você:

- **Usou o Playwright MCP para testar o recurso manualmente.** Adicionou o servidor MCP do Playwright e permitiu que o Copilot controlasse um navegador para verificar a filtragem antes de abrir um pull request.
- **Orientou o modo de agente em alterações coordenadas na pilha.** Desenvolveu em uma sessão um filtro que envolveu o cliente, o servidor e os testes.
- **Usou um agente personalizado.** Selecionou o agente de acessibilidade na lista de agentes e acompanhou a implementação do modo de alto contraste.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações propostas na conversa, aceitou as desejadas e ampliou a sessão com um pedido complementar para o modo claro.
- **Concluiu o fluxo com um pull request.** Publicou o trabalho local e revisou tudo como a equipe faria.

## Revisão e próximos passos

Este é o fim do percurso obrigatório do VS Code. Você pode encerrar o workshop aqui.

Para ampliar o que conhece sobre os recursos de agente do Copilot, os outros percursos apresentam cenários relacionados em diferentes interfaces:

- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente personalizado de QA, Playwright MCP, Agent Merge e transições com reconhecimento de contexto.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — atribua issues ao agente de nuvem, monitore sessões pela página de agentes e itere de forma assíncrona sobre pull requests.

Você também pode continuar o trabalho iniciado aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que você pode adaptar aos seus projetos.

Para uma extensão opcional, [Opcional: Integrar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar o Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Monitorar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
