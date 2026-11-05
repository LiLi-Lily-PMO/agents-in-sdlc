---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para desenvolver a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais completo. Felizmente, o aplicativo GitHub Copilot foi criado para trabalhar com o fluxo de uma organização e ajudar a desenvolver os recursos certos da maneira adequada. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começará usando uma issue para gerar um recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma sessão ao longo do fluxo de desenvolvimento do recurso. Normalmente, você usaria sessões ou PRs diferentes para os vários tipos de arquivo, mas neste workshop faremos um atalho para concentrar a atenção nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova sessão de agente a partir de uma issue do GitHub;
- definir requisitos no modo **Plan**;
- implementar o recurso com o modo **Autopilot**;
- revisar o código;
- validar o recurso manualmente em um canvas de navegador.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a programar. Você usará o modo Plan para resolver essas decisões e, depois, autorizará o modo Autopilot a executar uma implementação de escopo definido.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Abrir uma issue registrada com os detalhes do trabalho necessário.
2. Criar um plano para o que será desenvolvido.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue alguma variação do processo acima.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você revisa e aprova o plano antes que o agente o execute.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando sem aguardar orientação.

Comece no modo Plan, revise o plano e, em seguida, use Autopilot para implementá-lo.

## Iniciar uma sessão a partir da issue

Antes de começar, confirme se o PR de avaliações por estrelas foi mesclado e se sua branch local `main` está atualizada.

1. Em **Sessions** na barra lateral, selecione a pasta do repositório Tailspin Toys e, em seguida, **Issues** na navegação superior do repositório. Abra **Allow users to filter games by category and publisher**. Essa issue predefinida já existe; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na branch `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se eles funcionarem no terminal, mas não na sessão, verifique o PATH do aplicativo ou a configuração do gerenciador de versões e feche e reabra o aplicativo. Tente novamente a operação e as verificações que falharam; não reinstale o npm apenas porque o agente não consegue localizá-lo.

## Planejar o recurso de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você iniciou pela issue, o Copilot já tem a solicitação do recurso como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele inclui a filtragem por categoria e editora, controles acessíveis, alterações no acesso a dados e testes. Discuta qualquer comportamento pouco claro, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes de unidade, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha o escopo na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e mantenha à mão a URL da issue e os esclarecimentos acordados para a validação posterior.

## Aprovar explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confirme o modo ativo em vez de presumir que a aprovação o alterou. Verifique se o indicador de modo mostra **Autopilot**.

O Copilot começará a trabalhar na implementação. Ele percorrerá o plano estabelecido de forma iterativa, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto, revise o plano primeiro. Se o Copilot informar dependências ausentes ou um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Interrompa somente os servidores que você iniciou.

## Revisar e verificar a implementação

Depois que o código for gerado, ele precisa ser revisado antes do merge, como qualquer outro código. Vamos revisar o código e executar o site para confirmar se tudo está funcionando.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e com os esclarecimentos aprovados, incluindo combinações de várias categorias e editoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Revise os resultados do lint, dos testes de unidade, dos testes E2E e da verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma versão de pré-visualização e pode reutilizar um servidor local. Confirme se o servidor testado pertence a este worktree, e não a uma lição anterior.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como fizemos antes e abrir o site em um canvas de navegador.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme se os resultados correspondem a essa editora.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento acordado para a combinação.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, use o prompt a seguir para pedir ao Copilot que interrompa o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Manter esta sessão para as próximas lições

Ainda não crie nem faça o merge do PR de filtragem. Mantenha esta sessão, este worktree e esta branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias à skill e ao QA antes de criar e mesclar o PR combinado na Lição 8. Depois de interromper o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma nova sessão de agente a partir de uma issue do GitHub;
- definiu requisitos no modo **Plan**;
- implementou o recurso com o modo **Autopilot**;
- revisou o código;
- validou o recurso manualmente em um canvas de navegador.

Em seguida, vamos entender melhor como gerar código que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
