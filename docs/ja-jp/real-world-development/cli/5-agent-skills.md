---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使用する"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、開発作業の一部にすぎません。これまで、コードが正常に動くことを手動で確認し、指示ファイルでコーディング規約に従うようにしてきました。テストや lint など、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には、**エージェントスキル**が役立ちます。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の内容を学びます。

- 既存の `quality-checks` スキルを確認します。
- 結果の表示形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使用しています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースが含まれます。基本的には、スキルの名前を付けたフォルダーと、その中の `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキルの名前と説明を定義する frontmatter、スキルの概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、スクリプトやほかのリソースを格納するサブフォルダーも追加できます。

> [!NOTE]
> 追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクト内の既存コマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用するリポジトリアセットとして `.github/skills` に配置できます。または、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明によって、Copilot はこのスキルを呼び出すタイミングを判断できます。
3. 指示を読み、テストと lint のプロセスをどのように案内しているか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。まず、3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何が実行され、成功したか、ツールがどのような結果を報告したかがわかるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に、次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルは、会話を再起動せずに Copilot CLI で再読み込みできます。更新した結果を確認しましょう。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次の作業を行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の表示形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

変更はフィルター機能の PR に含めます。次は、[Playwright MCP サーバーを通じて][next-lesson] Copilot がサイトを直接操作できるようにします。

## ほかのスキルの例

コミュニティによる次の例は参考資料であり、このレッスンで追加の作業を行う必要はありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
