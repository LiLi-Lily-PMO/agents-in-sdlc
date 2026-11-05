---
title: "レッスン 0 - 前提条件"
description: "Copilot CLI ワークショップの前提条件を確認し、Tailspin Toys の自分用リポジトリと GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要なものを準備します。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。次のレッスンでは、統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次の内容を学びます。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトを使える状態にします。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを作成します。[テンプレートリポジトリ][tailspin-template]から新しいリポジトリを作ると、ラボに必要なファイルがすべて含まれます。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template** を選択し、続けて **Create a new repository** を選択してリポジトリのコピーを作成します。
3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターから案内された手順に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. ワークショップの後半で参照するため、作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に用意されます。ワークショップではこの Issue を使って作業するため、自分で起票する必要はありません。

新しいワークショップテンプレートを使用してください。リポジトリには、必要な指示ファイル、アプリケーションコード、テスト、`quality-checks` スキル、バックログが含まれています。古いコピーを使用する場合は、必要なファイルがそろっているか、ファシリテーターに確認してください。

## Codespace を作成する

次は、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーから直接コードを作成、実行、デバッグできます。複数のプログラミング言語、拡張機能、ツールに対応したフル機能のエディターを利用できます。

1. 作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートによって、プロジェクトの依存関係、Playwright Chromium、ローカルデータベースがインストールされます。
5. **Do you trust the authors of the files in this folder?** と表示された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次の作業を行いました。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成しました。
- Codespace を作成し、プロジェクトが使える状態であることを確認しました。

次は、Codespace に[GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
