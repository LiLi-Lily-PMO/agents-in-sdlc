---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザーエクスペリエンスの改善や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期に自律して動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。たとえば、GitHub.com の[エージェントのページ][agents-page]を使うと、オープン中のプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次の作業を行います。

- エージェントのページを確認し、cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能を作業中のため、ライトモードも一緒に実装するのが合理的です。ハイコントラストに加えてライトモードも追加されるよう、エージェントの作業に指示を与えます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイコントラストモードの追加
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような内容があります。
   - リポジトリのチェックアウト
   - 作業用の環境の作成
   - MCP サーバーの設定
   - タスク完了に向けたさまざまな作業

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、監視対象の PR と変更されたファイルを確認できます。

## cloud agent に指示を与える

実行中のタスクを確認できたので、ハイコントラストモードを作成している Copilot に、ライトモードの切り替え機能も依頼します。

1. ハイコントラストモードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![mission control に表示されるアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セッションがセットアップを完了し、作業を開始するまで数分待ちます。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![cloud agent タスクの画面で、作業中のセッションに指示を与えるダイアログが強調表示されている](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot の作業を続ける

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えると、新しい依頼が作業フローに加わります。

作業には数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、ガイドするための中心的な場所である Copilot agents ページを確認しました。この mission control を使うと、進行中や完了済みのタスクを追跡し、作業内容を確認して、進行中のタスクに追加の指示を与えられます。

このレッスンでは、次の内容を学びました。

- mission control とエージェントのページを確認し、cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
