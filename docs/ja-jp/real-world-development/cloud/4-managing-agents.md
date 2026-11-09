---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前のいくつかの演習では、ユーザーエクスペリエンスの改善や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]もその 1 つです。この Mission control では、オープンな pull request (PR) があるエージェントタスクをすべて確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次の内容を学習します。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するとよいと考えました。サイトのスタイル更新と切り替え機能の実装が進行中なので、ライトモードも含めるのが合理的です。エージェントに指示を追加し、ハイコントラストに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域に表示されたタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新
   - ゲームを変更する API の生成
   - Web サイトへのハイコントラストモードの追加
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような項目が含まれる場合があります。
   - リポジトリからのコードのチェックアウト
   - Copilot の作業環境の作成
   - MCP サーバーのセットアップ
   - 割り当てたタスクを完了するための各種手順

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される pull request (PR) の領域も確認します。PR と変更されたファイルを確認し、追加で監視できます。

## Cloud agent に指示を追加する

進行中のタスクを確認したので、Copilot がハイコントラストモードを実装する間に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セッションがセットアップを終えて作業を開始するまで数分待ちます。以下のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェントタスクを操作するダイアログが強調表示された、エージェントページの画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に反映したことを確認します。

## Copilot に作業を続けてもらう

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼をワークフローに反映します。

今回も数分かかるため、これまでに学んだことや確認したことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して方向付ける中心的な場所である Copilot agents ページを確認しました。この Mission control では、進行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を与えられます。

このレッスンでは、次の内容を学習しました。

- Mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は[作成された pull request を反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
