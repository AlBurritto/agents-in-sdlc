---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連のワークフローを進めながら、GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター Issue を見つけました。
- 星評価を追加し、ポートフォワードしたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルター Issue を起点に、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) 用のカスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge にフィルター PR のマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

ワークショップでは、更新された `main` からそれぞれ独自のブランチを作成し、2 つの PR を完了しました。

1. **星評価:** 既存の `starRating` と未評価の場合の状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能を計画してから PR を作成するまで、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA はこれらの結果に要件とテスト範囲のレビューを加え、PR の前にまとめて確認しました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で追加テストが不要と判断される場合もあります。マージを許可する前に、コードと検証結果をレビューしてください。変更後は、影響を受ける検証結果を再取得します。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は構築前に方針を検討するとき、**Interactive** は範囲を絞った変更に関与し続けたいとき、**Autopilot** は範囲が明確なタスクに適しています。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選んでください。

ツールの基盤と同じくらい、コンテキストも重要です。何を作るか、なぜ作るか、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI の機能として、ほかにも次のコマンドを確認できます。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点から意見を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールも、使い続けることで上達します。実際の製品コード、趣味のコード、長年考えていた小さなアプリなどで活用してください。チームで学んだことを共有し、互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境を試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI での pull request の管理][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
