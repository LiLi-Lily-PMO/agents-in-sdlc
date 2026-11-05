---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメントの規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。決められた方法で作業する必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードの内容だけでなく、その構成方法も記述できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメントの規約を追加して、コードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの指示とパスで適用範囲を指定した指示が、エージェントに読み込まれる方法を確認します。
- コーディング規約に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントには、コードの言い換えではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更されたときにコメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行するための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや好みを Copilot に伝え、より適切な提案やコードを生成できるようにします。リポジトリ全体、または特定のファイルやタスクを対象に、コーディング規約、ライブラリ、コメントの書き方などを指定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡す単一の指示ファイルです。多くのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの形式の指示とそのサポート状況は、環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、作業を始めるための指示ファイルがすでに含まれています。変更を加える前に、既存の内容とその影響を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングに関するガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、指示の適用対象となるファイルを指定する glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内のファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、追加の指示が必要です。`copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しを探します。およそ 35 行目にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存したら、フィルター機能のコンテキストを保ったまま会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチに切り替えて、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを読み取る方法を確認し、新しい規約を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパスで適用範囲を指定した指示が、エージェントに読み込まれる方法を確認しました。
- コーディング規約に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な quality-checks スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
