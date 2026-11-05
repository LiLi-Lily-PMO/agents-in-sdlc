---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント型の開発を 1 つの作業に集中できるワークスペースにまとめます。エージェントセッションの並行実行、セッションモードの切り替え、共有キャンバス、GitHub Issue とプルリクエストのネイティブな管理機能を利用できます。さらに **Agent Merge** を使うと、プルリクエストのリベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージまでを進められます。

このワークショップでは、Tailspin Toys で一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと作成済みのバックログを確認します。
2. 星評価の変更に対象を絞り、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の Issue から始め、**Plan** モードでアプローチを決め、**Autopilot** モードで構築して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を確認します。
7. 品質保証 (QA) カスタムエージェントを作成して、要件、カバレッジ、検証の証拠をレビューします。
8. フィルター機能全体をレビューし、2 つ目の PR に Agent Merge を使用します。
9. キャンバスを利用できるか確認したうえで、Database Explorer を使用し、リポジトリに保存されるトリアージキャンバスを作成してテストします。必要な機能を利用できない場合は、利用できないことを記録し、読み取り専用の代替手段を使うか、まとめのレッスンに進みます。

ワークショップを効率よく進めるため、PR は 2 つ作成します。1 つ目は星評価、2 つ目は指示、スキル、QA プロファイル、テストの更新を含むフィルター機能です。どちらも更新済みの `main` から開始します。フィルター機能と品質に関するワークフローでは、1 つのセッション、worktree、ブランチを共有するため、各ツールを学びながら作業を積み重ねられます。最後のキャンバス演習は同じセッション内で行い、PR ワークフローを繰り返さずに共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価の追加: すぐに得られる成果][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を行い、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使用する][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの観察 | Customize から MCP を設定し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使用する][ex7] | 要件とカバレッジ | 専門家プロファイルを作成して選択し、最終検証の証拠を集めます |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | フィルター機能、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使用します |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使用してから、リポジトリに保存するトリアージキャンバスを作成してテストします |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は codespace ではなく自分のコンピューターで動作します。そのため、[レッスン 0][ex0] ではアプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、アプリを使うには管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。この設定は Copilot CLI ポリシーとは別です。

## はじめに

**[レッスン 0「前提条件」から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
