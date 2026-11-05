---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなど、さまざまな作業が必要です。開発者は、必要なファイルを探し、変更し、テストを実行し、デバッグし、更新し忘れたファイルを見つけてさらに変更する、といった作業を繰り返します。

このような場合に Copilot エージェントモードが役立ちます。

Copilot エージェントモードは、IDE 内でより自律的に作業できるように設計されています。開発者と同様に、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードでサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けに作られています。エージェントモードは IDE 内で作業し、すばやいフィードバックとやり取りを可能にします。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次の内容を学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法
- GitHub Copilot エージェントモードで UI とデータレイヤーにわたる新機能を実装する方法
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では発行元のヘルパーを追加しました。次は Copilot エージェントモードを使って、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を理解するため Tailspin Toys の Web サイトを確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧や詳細を表示します。単一の Astro アプリで、ビルド時に静的 HTML ページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準の DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後なので `main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーの URL `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace でターミナルから localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーをホストしている codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作していますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成すると、Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示させます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。エージェントモードは、ファイルをまたぐ変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップでは Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードの一貫性を高め、使いやすい体験を提供することが目的です。ただし、LLM は確率的なため、ワークショップの例とは異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が異なる MCP コマンドを使用する場合がありますが、タスクを完了できます。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを Copilot に与える指示ファイルを確認します。Web サイトの機能追加に関するガイダンスを含む [ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使用します。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、複雑な内容を小さく管理しやすい単位に分け、例やテンプレートを提示できます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) の残りのフィルター処理を追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

また、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードに作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連する Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストが見つかり、ファイルの変更とテストが開始されます。

   ![Copilot がプロジェクトファイルを調査している](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを変更して、テストを実行します。タスクやコードベースの複雑さによって、数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** のボタンが表示されることがあります。Copilot の作業が完了すると、すべての変更に **Keep** または **Undo** が表示されるため、進行中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat がテスト実行の許可を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加しました。次の内容を学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにわたる新機能を実装する方法
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法

次は、Playwright MCP サーバーで[機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装させてみます。次の点を検討してください。

- 指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選ぶか、固定値にするかなど、ページネーションの動作を決めます。
- 希望どおりの実装に必要なガイダンスをプロンプトに含めます。
- GitHub Copilot に追加のコンテキストを伝えて変更を依頼し、繰り返し作業することがあります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
