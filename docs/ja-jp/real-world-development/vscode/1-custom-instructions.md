---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot にコード生成のガイダンスを伝える方法はいくつかあります。主なものは[指示ファイル][instruction-files]です。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される新しい規約を追加します。
- 同じプロンプトを再実行し、生成されるコードに新しい規約が反映される様子を確認します。
- 次の演習で Copilot が更新したガイダンスを使えるように、指示の更新とフィルター機能のコードを `main` にコミットします。

> [!CAUTION]
> 生成されるコードは、設定した規約の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に動作がどのように**変化するか**を確認します。出力が一字一句同じである必要はありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにして、モダンな見た目にします。
- コードに TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使うと、Copilot が適切な情報を得て、示された開発手法に沿ってタスクを実行できます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや好みを Copilot に伝え、コードの提案やスニペットをより適切なものにできます。推奨するコーディング規約やライブラリ、コードコメントの種類などを指定できます。リポジトリ全体や、特定のファイルまたはタスクを対象に指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡す単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、リポジトリでのチャットや CLI リクエストの多くに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象にできます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格に基づく指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳細はこのワークショップの範囲外ですが、サンプルプロジェクトには一般的な方法を示す例が含まれています。基本的には、次の点を意識します。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体に適用するコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイルの種類や、特定のタスクに関する指示に使用します。
- 自然な言葉で、わかりやすいガイダンスとコードの例を示します。

指示ファイルの作成方法は 1 つではなく、AI の利用方法も同様です。プロジェクトに合った方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の指示ファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。利用する画面によって方法は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。関連するレッスンで手順を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどが集められた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。中心となる `copilot-instructions.md` が 1 つあり、さまざまなタスク用の `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の詳細なガイダンスもあります。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、指示を適用するファイルを指定する glob を設定します。ここでは、`**/*.test.ts` に一致する TypeScript のテストファイルなどが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ファイルの末尾にある `unit-tests.instructions.md` などへのリンクと、プロジェクト内の既存ファイルを確認します。指示を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示すことができます。パスはリポジトリのルートではなく、指示ファイルの場所からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイルヘッダーコメントの規約を追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示の更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディタータブをすべて閉じ、Copilot が必要なコンテキストだけを読み込めるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを更新します。多くの場合、ヘルパーファイルとテストを変更します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取る型付き関数であり、発行元の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の**一貫性**の改善が、この演習の目的です。

## リポジトリ全体に適用する規約を追加する

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を与えます。コードの提案を改善するため、コーディング規約に新しい内容を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを探します。およそ 27 行目にあります。既存の規約は記載されていますが、コード内のドキュメントに関する規則はないため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の規約のすぐ下に次の Markdown 行を追加し、ファイルヘッダーコメントと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用される指示には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクに関する指示には `*.instructions.md` を使用します。ここで追加したドキュメントコメントの規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し直し、Copilot が適切なファイルに注目できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの冒頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォーム用の発行元データアクセスヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべての発行元の一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns 発行元オブジェクトの配列を解決する Promise です。
    */
   ```

新しいプロジェクト規約に従うよう Copilot をガイドし、次の演習でも利用するコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルはリポジトリのほかのアセットと同じように、ソース管理で管理します。変更をコミットしてブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基礎部分のために Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue と関連付けてプルリクエストを作成します。手動で行うこともできますが、接続済みの GitHub ツールを Copilot に使わせます。Issue を検索し、Issue をクローズする PR を作成して、マージを依頼します。

> [!NOTE]
> Copilot に GitHub 操作を依頼するのは、この演習が初めてです。プロジェクトテンプレートでは GitHub ツールがワークスペースに設定済みのため、セットアップは不要です。Copilot がツールを初めて使うときに、VS Code で GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。後の演習で Model Context Protocol を使った接続方法を学びます。

1. codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成し、指示の更新とフィルター機能に向けた発行元ヘルパーの追加を説明するよう Copilot に依頼します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探し、PR を作成します。
5. Copilot が GitHub 操作を許可するよう求めたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR のマージと、ローカルブランチの `main` への切り替えを依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの実行を許可するよう求めたら、コマンドを確認して必要に応じて **Allow** を選択します。

これで GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを読み取る方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を使ったコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される新しい規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい規約が反映されることを確認しました。
- 次の演習で作業を続けられるように、指示の更新とヘルパーの基礎部分を `main` にコミットしました。

次は、エージェントモードで指示を使い、Copilot にコードベース全体にわたる新機能を追加させます。[エージェントモードに進みます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
