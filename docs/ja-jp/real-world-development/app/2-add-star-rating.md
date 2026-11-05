---
title: "レッスン 2 - 星評価の追加: すぐに得られる成果"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加え、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースの構成を確認し、簡単なチャットも試しました。次は **エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の内容を学びます。

- エージェントセッションを開始し、その構成を確認します。
- エージェントに小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを開いてマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されません。ウォームアップとして、各カードに既存の評価を表示します。最初のセッションに適した、小規模で独立した変更です。

## セッションの構成

**セッション**は、独立したワークスペースでエージェントと行う会話です。セッションごとに専用の **Git worktree とブランチ**が割り当てられます。そのため、あるセッションで機能を追加しながら、別のセッションでバグを修正しても、変更が競合しません。セッションはサイドバーにリポジトリごとにまとめて表示されます。いずれかを選択して切り替えます。

セッション内では、エージェントとの**会話**、ファイルを調査・編集するエージェントの**ツール操作**、差分を含む**変更されたファイル**の一覧を確認できます。

## セッションを開始して変更を依頼する

プロジェクトを調べて機能を実装するため、新しいセッションを開始します。[アプリの設定][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選択します。
4. プロンプト欄の下で **new working tree** と **Interactive** モードを選択します。次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押して、Copilot にプロンプトを送信します。

Copilot app はまず新しい worktree、つまりプロジェクトの分離されたコピーを作成します。次にプロジェクトを調べ、新しい機能を追加するために更新が必要なファイルを見つけ、コードを作成します。これで Copilot app を使った機能の追加ができました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューします。Copilot app で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選び、変更されたファイルと差分を確認します。

   ![Create PR の右側に Toggle review panel ボタンがある、GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使われる中心的なファイル `GameCard.astro` に、コードが追加されています。評価がある場合は表示し、`starRating` が `null` の場合は `No rating yet` を表示する、次のような小さなコードです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> ほかの生成 AI ツールと同様に、Copilot は決定論的ではなく確率的に動作するため、コードは上記と異なる場合があります。大まかな構造が似ていることを確認してください。

## 変更を確認する

ブラウザーを開く前に、エージェントが自動で実行したチェックの結果を確認します。数値の `starRating` と、`null` の場合の代替表示の両方をテストがカバーしていることを確認します。前提条件が不足していたりチェックがスキップされたりしている場合、成功とはみなせません。インストールのリクエストを承認する前に確認してください。

コードを読んだだけで正常に動くと判断せず、Copilot に Web サイトを開かせて更新された UI を確認します。Web サイトを起動し、ブラウザーキャンバスで開くよう依頼します。

> [!TIP]
> Canvas は Copilot app の中で利用できるインタラクティブなウィジェットです。この後のレッスンでは独自の Canvas を使ったり作成したりしますが、ここでは組み込みのブラウザー Canvas を使います。

1. 次のプロンプトで、アプリを起動してブラウザー Canvas で開くよう Copilot に依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーが開きます。
3. 評価のあるゲームカードに、5 段階の評価が表示されていることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションのために起動した開発サーバーを停止するよう Copilot に依頼します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードを既存のコードベースにマージするため、プルリクエスト (PR) を作成します。

1. 右上の **Create PR** を選択します。
2. プロンプトが表示された場合は **Sign in with your browser** を選択し、認証手順に従います。
3. PR の作成前に対応する Issue が必要であると Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビュー用のペインで PR を開きます。必要に応じて内容をレビューします。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースできました。このレッスンでは、次の作業を行いました。

- エージェントセッションを開始し、その構成を確認しました。
- エージェントにゲームカードの小さく対象を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を開いてチェックを確認し、自分でマージしました。

次は、[フィルター機能の Issue から始め、Plan と Autopilot モードを使って][next-lesson]より大きな機能を構築します。

## リソース

- [GitHub Copilot app のエージェントセッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#install-and-configure-the-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
