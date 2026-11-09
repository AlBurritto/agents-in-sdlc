---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、より重要な作業に集中できます。

この演習では、Copilot cloud agent を使って次の内容を学びます。

- コード生成用の環境をカスタマイズする方法
- 操作を安全に行う方法
- 範囲を明確に定めた Issue の重要性
- Copilot に Issue を割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。Web サイトの初期版を作成した委託先がドキュメントを十分に作成していません。まず、アプリケーションのエクスポートされた関数すべてに TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリの他のゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い Issue がほかにもあります。

こうした作業は後回しになりやすいため、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期に作業している間、開発者は別のタスクに集中できます。その後、戻ってきて作業内容をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、いくつかの方法で作業を依頼できます。割り当てると Copilot は進行状況を追跡するドラフト pull request を作成し、環境をセットアップしてタスクを開始します。進行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法の力があるわけではありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく考えると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進めながら、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習を完了していれば `.github/copilot-instructions.md` の動作を確認済みです。まだの場合は、ここで簡単に確認しましょう。cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認しておくと、後で指示の影響を見つけやすくなります。

GitHub の Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の慣例と TSDoc のドキュメントコメントに関する指示を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターで、指示の適用先が `**/*.test.ts` ファイルに限定されていることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成される pull request に TSDoc のドキュメントコメント、TypeScript の慣例、コメントヘッダーが反映されているか確認してください。これらの指示ファイルが提供する内容です。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するときは、担当者に関係なく、環境の準備やスクリプトの実行が必要になることがあります。Copilot にタスクを割り当てる場合も同じです。Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を用意します。作業開始前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールと依存関係を利用できるように環境をカスタマイズできます。ラボを円滑に進め、学習に集中できるよう、このファイルはあらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが使用されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` アクセス許可が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを準備します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同様ですが、次の点が重要です。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動作するか試すときに便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントが不足していたりします。この種の技術的負債は放置されやすく、開発の効率を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成に役立つため、Copilot cloud agent に割り当てるのに適した Issue です。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で作業内容をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細画面](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot が環境を準備し (先ほど確認したワークフローを実行し)、プロジェクトを分析して、問題に取り組む方針を決めています。

13. リストと、実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を理解し、方針を考えて、コードを作成する様子を確認できます。

作業には数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期で処理し、その間に別の作業を進められるようにすることです。この機能を活用して、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加する作業に取り組みます。

## ゲームの詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の 1 つは、作業を分けられることです。開発者がある作業をしている間に、エージェントに別の作業を任せられます。デザインチームが求めている関連ゲームのセクションは、長時間かからないかもしれません。それでも、別の作業に時間を使えるよう cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当て、非同期に作業させることができます。技術的負債への対応、新しい機能の作成、別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次の内容を学びました。

- コード生成用の環境をカスタマイズする方法
- 操作を安全に行う方法
- 範囲を明確に定めた Issue の重要性
- Copilot に Issue を割り当てる方法

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
