---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。すべてをまとめてレビューし、pull request を作成しましょう。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを効率よく進めるため、フィルター機能と品質に関する一連の作業は、同じ会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージまでの作業を自動化する方法を確認します。
- 機能全体の変更と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビューの後にのみ Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルター機能の開発では、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認権限は開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は PR の妨げになっている内容を処理します。継続的インテグレーション (CI) のチェック失敗を修正し、レビューコメントに対応し、必要に応じてリベースした後、リポジトリで使用できる場合は GitHub の自動マージを有効にします。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューし、マージできる状態であると判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、アクセス許可の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発手順のいくつかを自動化しました。このレッスンでは、次の内容を学習しました。

- Agent Merge と、マージまでの作業を自動化する方法を確認しました。
- 機能全体の変更と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意の cloud agent への委任に使う便利な [Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI での pull request の管理][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
