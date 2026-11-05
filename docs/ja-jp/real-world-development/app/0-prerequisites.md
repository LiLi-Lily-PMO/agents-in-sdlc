---
title: "レッスン 0: 前提条件"
description: "GitHub Copilot app のレッスンに備えて、Tailspin Toys プロジェクト用に Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub を利用するためのデスクトップアプリケーションです。Issue とプルリクエストにすばやくアクセスでき、GitHub Copilot を使った開発も行えます。このワークショップではローカル環境で作業し、GitHub Copilot app を使って Astro で構築された Tailspin Toys アプリを更新します。始める前に、ローカル環境に Node.js がインストールされていることを確認し、次に Copilot app をインストールします。

このレッスンでは、次の作業を行います。

- プロジェクトのテストをコンピューターで実行できるよう、Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を構築して、ローカルで Tailspin Toys のテストスイートを実行させます。プロジェクトに必要なランタイムは **[Node.js][nodejs]** だけです。現在の **LTS** リリースをインストールします。

すべてのプラットフォームで最も簡単なのは、公式インストーラーを使う方法です。

1. Windows Terminal、macOS のターミナルなど、普段使用するターミナルウィンドウを開きます。
2. 次のコマンドを実行し、インストールされている Node.js のバージョンを確認します。

   ```shell
   node --version
   npm --version
   ```

3. 両方のコマンドが成功し、Node.js のバージョンがプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進みます。

> [!TIP]
> Node.js がインストールされていない場合や、更新が必要な場合にのみ、次の手順を実行します。

4. [Node.js のダウンロードページ][node-download]を開きます。
5. 使用するオペレーティングシステム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を受け入れます。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルウィンドウを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

   ```bash
   node --version
   npm --version
   ```

9. インストールされた Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントからも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストール後や PATH の変更後は、アプリを完全に終了して再起動します。セッションでどちらかのコマンドが失敗する場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]を実行します。

> [!IMPORTANT]
> E2E チェックを行うには、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree を準備するときは Tailspin Toys リポジトリの README に従い、インストールを承認する前にリクエストを確認します。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレートリポジトリ][template-repository]からコピーを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれており、アプリをインストールするときに接続します。

1. 新しいブラウザーウィンドウで、ラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` に移動します。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、続けて **Create a new repository** を選択して自分用のコピーを作成します。

   ![Use this template ボタン。ドロップダウンでは Create a new repository が選択されている](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターから案内された手順に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![Create a new repository フォーム。テンプレートとして github-samples/tailspin-toys が設定され、リポジトリ名が入力されている](../../../_images/app-0-create-repository.png)

4. ラボの後半で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に用意されます。ワークショップではこの Issue を使用するため、自分で起票する必要はありません。

新しいワークショップテンプレートを使用してください。リポジトリには指示ファイル、アプリケーションコード、テスト、`quality-checks` スキル、既存の Canvas 拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使用する場合は、必要なファイルがそろっているか、ファシリテーターに確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次の作業を行いました。

- コンピューターでプロジェクトをビルドしてテストできるよう、Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は [GitHub Copilot app をインストール][next-lesson]し、作成したリポジトリを接続して、ワークスペースの使い方を確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
