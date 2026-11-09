---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、登録済みのフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールを利用できます。作業を任せたり、変更を依頼したりできるため、開発の流れを保てます。まずはツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次の内容を学習します。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が普段使っているターミナルでその機能を利用できます。このレッスンでは、インストールと認証を済ませ、ワークショップの残りの内容に取り組む準備をします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js がプリインストールされているため、ここでは npm を使います。

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

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

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

4. 表示されたパスが Tailspin Toys リポジトリであることを確認します。**Yes, and remember this folder for future sessions** を選択して、信頼することを確認します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証済みの場合があります。認証を求められずに Copilot CLI が起動すれば、そのまま利用できます。

## 基本操作を確認する

通常のシェルプロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選んで <kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みのバージョンで利用できるコマンドを確認します。<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 動作確認として、Copilot に簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答の前にリポジトリを調査する様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルターの Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、Issue の内容を確認します。最初の変更に取り組む際にこの Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree が作成されるわけではありません。各変更の前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、許可された範囲外のツールを使う前に通常確認を求めます。このワークショップでは `--yolo` を使って再起動します。Codespace 内で承認プロンプトを省略できるため、演習に集中できます。

> [!CAUTION]
> `--yolo` を有効にすると、すべての権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) が自動的に許可されます。Codespace や VM などの隔離された環境でのみ使用し、日常の開発用に既定のエイリアスとして設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` も指定し、後のレッスンで Issue や pull request を操作するための読み取り・書き込み用 GitHub MCP ツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースにはアクセスできます。変更を公開またはマージする前に必ずレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問をし、会話が機能することを確認します。その後、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンでは、指示を変更したりエージェントを追加したりした後に `copilot --resume` を使って、同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次の作業を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルター Issue を見つけました。

次は、[最初の小さな変更][next-lesson]に取り組み、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
