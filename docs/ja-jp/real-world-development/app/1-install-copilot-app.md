---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースの使い方を確認し、簡単なチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント型開発のためのデスクトップアプリケーションです。GitHub Copilot CLI を基盤として GitHub とネイティブに統合されており、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分ですべての作業を行うのではなく、それぞれ独立したワークスペースで複数のエージェントを並行して管理し、繰り返し作業を自動化するワークフローに適しています。Node.js をインストールし、プロジェクトの自分用コピーを準備したら、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次の内容を学びます。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートから用意されたバックログなど、ワークスペースの構成を確認します。
- アプリについて簡単なチャットで質問します。

## シナリオ

チームでは、増え続けるバックログに AI エージェントを活用し始めています。Copilot app を使うと、Issue の取得、エージェントの実行、変更のレビュー、プルリクエストのマージなどを 1 か所で行えます。このレッスンでは、アプリをインストールしてプロジェクトに接続し、プロジェクトについての会話を始める方法を学びます。

> [!NOTE]
> 対象の Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用できます。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして設定する

GitHub Copilot app を使うには、まずインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用するプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、プロンプトに従って認証します。データレジデンシーが有効な GitHub Enterprise Cloud を使用する場合は、**Use GitHub Enterprise** を選択し、プロンプトが表示されたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリの選択が表示された場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、オンボーディング後にプロジェクトを追加することもできます。
6. プロンプトが表示されたらテーマを選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されているか確認します。すでに表示されている場合は、追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとめられ、その下に個々のセッションが表示されます。

## ワークスペースの使い方を確認する

プロジェクトを接続したら、アプリの構成を確認します。サイドバーには、いくつかの領域があります。

- **New** — Copilot との新しいチャットセッションを開始します。
- **Pull requests** — アプリの GitHub 統合でプルリクエストを表示します。一覧の閲覧と絞り込み、CI 状況の確認、レビューをアプリ内で行えます。
- **Issues** — Issue の閲覧と絞り込み、Issue からのセッション開始ができます。
- **Automations** — スケジュールまたはオンデマンドで実行する保存済みのエージェントタスクです。To-do リスト、定期的なプロジェクト保守など、任せたい繰り返し作業に役立ちます。まとめのレッスンで次のステップとして紹介します。ワークショップ内で別の演習は行いません。
- **Customize** — MCP サーバー、プラグイン、スキルなどを追加して Copilot app の機能を拡張します。Playwright MCP の設定で使用します。
- **Chats** — 独自のブランチやワークスペースを必要としない、簡単な質問やブレインストーミングのための会話です。最初のチャットを開始した後に表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。
- **Sessions** — リポジトリフォルダーと、その中のエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリビューが開き、その下のセッションを選択すると会話に戻ります。セッションごとに独立したワークスペースを使うため、複数を同時に実行しても変更が競合しません。星評価の追加で最初のセッションを開始します。

ワークショップを進めながら、ワークスペースをさらに詳しく確認します。

> [!TIP]
> 方法がわからない場合や機能の有無が不明な場合は、Copilot に質問してください。作業方法を案内してくれます。

### 登録済みのバックログを確認する

ほとんどのプロジェクトにバックログがあり、Tailspin Toys も例外ではありません。テンプレートの作成時に用意されたバックログを確認します。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を前提とせず、タイトルで次の項目を探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue からエージェントセッションを開始できます。最初の変更が完了したら、フィルター機能の Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目のみが自動的に表示されます。ほかのリポジトリの作業項目も確認する場合は、そのリポジトリをアプリに追加してください。

## 簡単なチャットを試す

アプリの使い方に慣れるには、*アプリ自体*について質問するのがよいでしょう。**簡単なチャット**を使うと、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを作るほどではない、すぐに終わる質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選択して、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。その後 **Chats** が表示されます。
2. アプリ自身のセッションの仕組みについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで応答を読みます。各セッションが独立した Git worktree で実行されるため、複数のエージェントを並行して動かしても変更が競合しません。会話を続けることも、いつでも新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールしてプロジェクトを接続し、ワークスペースを確認しました。次の内容を学びました。

- アプリをインストールして GitHub にサインインする方法
- GitHub リポジトリからプロジェクトを追加する方法
- ワークスペースの構成を確認し、**Issues** で登録済みのバックログを見つける方法
- 簡単な質問をチャットで行う方法

次は[最初のエージェントセッションを開始][next-lesson]し、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を始める][getting-started]
- [GitHub Copilot app のエージェントセッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
