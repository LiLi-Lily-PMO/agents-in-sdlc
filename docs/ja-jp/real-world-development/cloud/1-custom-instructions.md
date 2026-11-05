---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。タスクを決まった方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される新しい規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue を非同期に処理するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成するプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys では、次のような開発手法に関するガイドラインや要件があります。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにして、モダンな見た目にします。
- コードに TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使うと、Copilot が適切な情報を得て、示された開発手法に沿ってタスクを実行できます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや好みを Copilot に伝え、コードの提案やスニペットをより適切なものにできます。推奨するコーディング規約やライブラリ、コードコメントの種類などを指定できます。リポジトリ全体や、特定のファイルまたはタスクを対象に指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡す単一の指示ファイルです。Copilot に送るチャットや CLI リクエストの多くに関係する、技術スタック、開発中の内容、ベストプラクティス、全体的なガイダンスを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象にできます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの規格に基づく指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳細はこのワークショップの範囲外ですが、サンプルプロジェクトには一般的な方法を示す例が含まれています。基本的には、次の点を意識します。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体に適用するコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイルの種類や、特定のタスクに関する指示に使用します。
- 自然な言葉で、わかりやすいガイダンスとコードの例を示します。

指示ファイルの作成方法は 1 つではなく、AI の利用方法も同様です。プロジェクトやシナリオに合った方法を、試しながら見つけてください。

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
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。ここにある指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、指示を適用するファイルを指定する glob を設定します。ここでは、`**/*.test.ts` に一致する TypeScript のテストファイルなどが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ファイルの末尾にある `unit-tests.instructions.md` などへのリンクと、プロジェクト内の既存ファイルを確認します。指示を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示すことができます。パスはリポジトリのルートではなく、指示ファイルの場所からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイルヘッダーコメントの規約を追加します。

## リポジトリ全体に適用する規約を追加する

ここで編集するのは、コード内のドキュメントを TSDoc コメントとファイルレベルのヘッダーコメントとして記載するプロジェクト全体の規則です。cloud agent は、このハーネスの後半で Issue に取り組むときに、この規則を読み込みます。

編集の前に作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、変更を整理してコミットできるようにします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の手順で説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するため、コーディング規約を文書化します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の規約が記載されていますが、コード内のドキュメントについての指示はまだありません。
3. 既存の規約のすぐ下に、次の Markdown 行を追加し、ファイルヘッダーコメントと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用される指示には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクに関する指示には `*.instructions.md` を使用します。ここで追加したドキュメントコメントの規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象となるブランチから指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチを作成します。したがって、cloud agent が新しい指示を読み込めるように、変更を `main` に反映する必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で Issue を割り当てるとき、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> ワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットします。ブランチを使う手順は、実際のプロジェクトでの作業方法を再現するために含まれています。

## まとめと次のステップ

このプロジェクトの指示ファイルを使って Copilot にコンテキストを与える方法を確認し、リポジトリ全体に適用される規約を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプルリクエストをレビューするときに、規約が適用されていることを確認します。

次は、Copilot cloud agent に[最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
