---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、より重要な作業に集中できます。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- 範囲を明確に定義した Issue の重要性
- Copilot に Issue を割り当てる方法

## シナリオ

Tailspin Toys は技術的負債に対応しようとしています。最初のサイトを構築した請負業者が、ドキュメントを適切に整備しませんでした。実際、ドキュメントはほとんどありません。まず、アプリケーションのエクスポートされた関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリのほかのゲームを表示し、閲覧を続けられるようにします。現時点では、ユーザー体験 (UX) の受け入れテストに使える簡単な実装が必要です。これは現在の作業をブロックしていますが、より優先度の高い Issue もあります。

どちらも優先度が下がりやすいタスクであり、Copilot cloud agent に割り当てるのに適しています。cloud agent に非同期で作業させれば、開発者はほかの作業を続け、後から Copilot の結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に進めて学習、実験、改善します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習に取り組んだ場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで簡単に確認します。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認して、後でその影響を確認できるようにします。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプルリクエストに TSDoc コメント、TypeScript の規約、コメントヘッダーがあるか確認します。これらの指示ファイルの内容が反映されています。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境を設定する

コードを作成するときは、誰が作業する場合でも、環境を整え、状態を確認するためのセットアップスクリプトを実行する必要があります。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェアエンジニアのように作業します。

cloud agent は GitHub Actions を使って作業環境を用意します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。ワークショップを円滑に進めるため、このラボ用の設定はすでに追加されています。単一の Astro アプリで、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順で必要となる最低限の権限を設定します。Copilot は自身の操作用トークンを受け取ります
    permissions:
      # セットアップ手順で依存関係をインストールするためにリポジトリをクローンする場合は、`contents: read` 権限が必要です。クローンしない場合は、セットアップ完了後に Copilot が自動的にクローンします
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを準備します
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様ですが、次の点に注意してください。

- `copilot-setup-steps` という 1 つのジョブが含まれています。Copilot がプルリクエストで作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメント自体がなかったりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に依頼するのに適したタスクです。バックグラウンドで必要なドキュメントを作成させ、後の演習で結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て用のダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルのプルリクエスト (PR) を開きます。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な対応方法を検討しています。

13. リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ウィンドウの更新が必要な場合があります。

16. ライブセッションをスクロールし、Copilot の作業を確認します。コードを調査して状態を把握し、計画を検討して適切な方法を決め、コードを作成しています。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行し、ほかの作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当てたら、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の大きな利点は、作業を分けられることです。デザインチーム向けの関連ゲームセクションの追加は時間がかからないかもしれませんが、その時間をほかの作業に使える場合もあります。この作業も Copilot cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て用のダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot がタスクに取り組んでいることを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェアエンジニアに似た方法で作業するため、常に監視する必要はありません。作業が完了したら確認します。次は、カスタムエージェントの作成と使用を学びます。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期に作業を実行できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

このレッスンでは、次の内容を学びました。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- 範囲を明確に定義した Issue の重要性
- Copilot に Issue を割り当てる方法

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用について学びます。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
