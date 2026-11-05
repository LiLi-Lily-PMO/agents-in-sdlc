---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cards dos jogos e integre-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conheceu o espaço de trabalho e experimentou um chat rápido, é hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e fará o merge no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e aprender como ela é estruturada;
- pedir ao agente que faça uma alteração pequena e específica no projeto;
- revisar a alteração na visualização de diff do espaço de trabalho;
- executar o aplicativo localmente para confirmar a alteração no navegador;
- abrir e fazer o merge do primeiro pull request.

## Cenário

Cada jogo na Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial, porém, mostram somente o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada card — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio espaço de trabalho isolado. Cada sessão recebe um **git worktree e uma branch dedicados**, o que permite trabalhar em várias sessões ao mesmo tempo — uma adicionando um recurso, outra corrigindo um bug — sem que as alterações entrem em conflito. As sessões aparecem na barra lateral, agrupadas por repositório. Selecione qualquer uma para acessá-la.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade de ferramentas** do agente enquanto explora e edita arquivos e a lista de **arquivos alterados** com os respectivos diffs.

## Iniciar uma sessão e solicitar a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar o recurso. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto por meio do repositório do GitHub. Agora, criaremos uma sessão para esse repositório e solicitaremos a alteração.

1. Abra o aplicativo GitHub Copilot ou volte a ele.
2. Na parte superior da barra lateral, selecione **+** para iniciar uma sessão.
3. Selecione o repositório `tailspin-toys`.
4. Abaixo da caixa de prompt, escolha uma **new working tree** e o modo **Interactive**. Use o prompt a seguir:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando um novo worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados para adicionar o recurso e cria o código necessário. Você acabou de adicionar uma funcionalidade com o aplicativo Copilot.

## Revisar o diff

Todas as alterações geradas por IA precisam ser revisadas antes do merge, mesmo as pequenas. Vamos examinar as alterações diretamente no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código adicionado a `GameCard.astro`, o arquivo principal que exibe os detalhes dos jogos. Ele deverá ser semelhante ao exemplo a seguir: um pequeno bloco que renderiza a avaliação quando ela existe e exibe `No rating yet` quando `starRating` é `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. No entanto, deverá ser relativamente semelhante.

## Verificar as alterações

Revise os resultados das verificações automatizadas do agente antes de abrir o navegador. Confirme se os testes cobrem um `starRating` numérico e a alternativa para `null`. Um pré-requisito ausente ou uma verificação ignorada não conta como aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que abra o site para examinar a interface do usuário (UI) atualizada. Para isso, podemos pedir que ele inicie o site e o abra em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do próprio aplicativo Copilot. Você explorará opções personalizadas e até criará seu próprio canvas mais adiante; por enquanto, usaremos o canvas de navegador integrado.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Confirme se os cards dos jogos avaliados exibem a nota de um total de cinco.
4. Quando terminar, use o prompt a seguir para pedir ao Copilot que interrompa o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abrir e fazer o merge do primeiro pull request

Você criou o recurso. Agora, abra um pull request (PR) para integrar o novo código à base de código existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para se autenticar.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes da criação do PR, confirme que ele deve seguir esse processo.
4. O Copilot começará a criar o PR.
5. Selecione o balão **PR** logo acima do chat para abrir o pull request no painel de revisão. Revise o PR conforme necessário.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para fazer o merge.

## Resumo e próximos passos

Parabéns. Você entregou sua primeira alteração com o aplicativo GitHub Copilot. Nesta lição, você:

- iniciou uma sessão de agente e aprendeu como ela é estruturada;
- orientou o agente para fazer uma alteração pequena e específica nos cards dos jogos;
- revisou a alteração na visualização de diff do espaço de trabalho;
- executou o aplicativo localmente para confirmar a avaliação por estrelas no navegador;
- abriu o PR 1, revisou as verificações e fez explicitamente o merge.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para desenvolver um recurso maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-e-configurar-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
