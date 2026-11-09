---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加を目的とする複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の実装、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を依頼しました。コードの変更内容を確認し、必要に応じて Copilot にフィードバックを与えて作業を改善しましょう。

### シナリオ

これまでに何度も確認したとおり、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。ここでは、チームの他のメンバーにもレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視されずにタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権は作業に使用するブランチ**のみ**にあります。
- Cloud agent は GitHub Actions 内に作業用の独立した一時環境を作成して実行されます。
- GitHub Actions のワークフローは、人間が承認するまで実行されません。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初の pull request (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールによってブロックされたという警告が表示されることがあります。これは**想定どおり**です。Copilot は既定で外部リソース (外部 MCP サーバーの呼び出しを含む) へのアクセスが制限されています。必要に応じて [Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、pull request の一覧を開きます。
3. **Add missing documentation**、またはそれに近いタイトルの pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. pull request の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントなど、更新されたコードを確認します。具体的な変更内容は異なる場合があります。TypeScript の慣例やコメントヘッダーも確認してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。
6. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

pull request で Copilot と作業するのは、一方通行ではありません。チームメンバーに依頼するのと同じように、pull request のコメントやコードのインラインコメントで **@copilot** を指定できます。Copilot はコメントを認識し、対応するための新しいセッションを開始します。結果は確定的ではないため、依頼内容を一律に指定することはできません。たとえば、次のような変更を依頼できます。

- それぞれのコードファイルの先頭に、内容を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、他のユーザーを指定するときと同じように **@copilot** を指定します。上記の例や、コードベースに必要なドキュメントについての別の提案を使います。
2. **View Session** を選択して、Copilot の作業を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると、pull request に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して改善してください。問題がなければ、ドラフトの PR をレビュー可能な状態に変更して、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page**、またはそれに近いタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があるか確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **オプション:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。これまでと同様に、Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、関連ゲームの表示件数を変更したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます (この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブに新しいコメントを追加すると、Copilot が認識して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission control で追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website**、またはそれに近いタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 特に次の点に注意して、実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存するためのローカルストレージの使用
   - ハイコントラストモードとライトモードの CSS やスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **オプション:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用のカスタムエージェントが、Copilot をアクセシビリティのベストプラクティスに沿って機能を実装するよう導いたことを確認してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** を指定して変更を依頼できます。

## オプションの演習 — 委任を続ける

Cloud agent に実際のバックログ項目を任せ、その間に別の作業を進めると効果的です。リポジトリにさらにいくつか Issue を登録し、Copilot に割り当ててみましょう。次のような作業を依頼できます。

- ゲームの詳細ページに支援者向けの興味登録フォームを作成する。
- ゲーム一覧ページにページネーションを追加する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンを通じて、次の作業を行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し**、cloud agent の出力に及ぼす影響を確認しました。
- **Copilot cloud agent に Issue を割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- **カスタムエージェントを作成して使用し**、アクセシビリティ機能としてハイコントラストモードとライトモードを追加しました。
- **Copilot agents ページを Mission control として使い**、アクセシビリティのセッションを監視して作業中に指示を追加しました。
- **cloud agent が作成した pull request をレビューして反復的に改善し**、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、他のハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から離れずに Copilot Chat のエージェントモードと MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使って、レビュー済みの変更を 2 つターミナルからリリースします。

自分のリポジトリでは、次の作業も試してください。

- リファクタリングやテスト範囲の拡充に関する Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールの設定][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
