---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コマンドラインからコードベースを探索し、コードを生成し、コマンドを実行し、外部ツールとやり取りできます。タスクを任せたり、変更を依頼したりしながら、作業の流れを維持できます。最初の手順はツールのインストールです。使い慣れたツールでインストールできます。

このレッスンでは、次の内容を学びます。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーでフィルター機能の Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに AI エージェントを活用し始めています。Copilot CLI を使うと、多くの開発者が普段使っているターミナルでその機能を利用できます。このレッスンでは、インストールと認証を済ませ、以降のワークショップで利用できるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js が事前にインストールされているため、ここでは npm を使用します。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初めて起動すると、Copilot CLI から GitHub アカウントで認証するように求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. プロンプトが表示された場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されているパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じて認証済みの場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま続行できます。

## 使い方を確認する

通常のシェルプロンプトで実行するコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選んで <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力してインストール済みのバージョンで利用可能なコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 動作確認のため、Copilot に簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーを利用できることを確認します。
6. Copilot にフィルター機能の Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の変更に進むときに使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使用し、自動的に worktree を作成しません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、許可された範囲外のツールを使う前に通常は確認を求めます。このワークショップでは `--yolo` を使って再起動します。Codespace 内では承認のプロンプトを省略でき、演習に集中できます。

> [!CAUTION]
> `--yolo` を指定すると、完全な自動許可 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) が有効になります。Codespace や仮想マシンなど、分離された環境でのみ使用し、日常の開発で既定として使うエイリアスには設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、後のレッスンで Issue やプルリクエストを操作するため、`--enable-all-github-mcp-tools` を指定して読み取り/書き込み可能な GitHub MCP ツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されていますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作することを確かめるため、プロジェクトについてもう 1 つ質問し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりしたら、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次の作業を行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルター機能の Issue を見つけました。

次は、[最初の小さな変更を始め][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
