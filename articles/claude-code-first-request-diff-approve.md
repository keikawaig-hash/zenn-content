---
title: "Claude Code に最初の依頼を出して、差分を読んでから承認するまで"
emoji: "🤖"
type: "tech"
topics: ["ClaudeCode", "AI", "初心者向け", "生成AI"]
published: false
---

## この記事で扱うこと

Claude Code を入れたあと、最初の1回で何を頼み、どの画面で何を確認するか。この記事では**「確認が入るモードにして、読み取りの依頼から始め、書き込みの差分を読んで承認する」**流れだけを説明します。画面は、練習用のフォルダ（祝日の CSV と、それを月別に数える短い Python スクリプトの2つ。Git で管理）で、2026年10月12日に実際に操作して撮影したものです。

## 1. 起動して、確認が入るモードにする

フォルダの中で `claude` と入力して起動します。撮影した環境では、起動直後の左下の表示が「auto mode on」（確認なしで進めるモード）でした。**Shift+Tab を1回押す**と「manual mode on」（変更やコマンドの前に確認が入るモード）に変わります。慣れるまでは manual にしておくと安心です。

![練習用フォルダで claude を起動し、Shift+Tab で manual mode on に切り替えた画面](https://crystal-method.com/wp2020/wp/wp-content/uploads/2026/10/claude-code-getting-started-01-launch-manual-m-20261012.png)

## 2. 読み取りだけの依頼から始める

最初の依頼は、変更範囲が小さく、結果を自分の目で確かめられるものを選びます。たとえば次のような依頼です。

```text
このフォルダに何があるか教えて
```

Claude Code は、ファイルの中身を調べるコマンドを実行する前に確認を出します。表示されたコマンドを読んで、「Yes」を選んで Enter を押します。

![コマンドを実行する前の確認画面。Do you want to proceed? に Yes・No などの選択肢が並んでいる](https://crystal-method.com/wp2020/wp/wp-content/uploads/2026/10/claude-code-getting-started-02-approve-command-m-20261012.png)

## 3. 書き込みを頼み、差分を読む

次に、小さな書き込みを頼みます。

```text
README.md を作って、このフォルダの概要と count_holidays.py の実行方法を書いて
```

「何を、どこに、どんな方針で」の3点が入っていると、動きが安定します。「直して」「テスト書いて」のような曖昧な依頼より、「ファイル名・期待する動作・守ってほしい方針」まで書くほうが、意図どおりの結果になります。

書き込む前に、次の確認画面が出ます。書き込む内容が行番号つきで表示され、その下で「Yes」「Yes（以後このセッションでは編集を自動で承認）」「No」から選びます。

![README.md を作る前の確認画面。書き込む内容が行番号つきで表示され、Do you want to create README.md? と聞かれている](https://crystal-method.com/wp2020/wp/wp-content/uploads/2026/10/claude-code-getting-started-04-approve-file-m-20261012.png)

## 4. 承認する前に見る3点

- **追加される行（+）**：意図したものか
- **消える行（−）**：消えてはいけない処理が含まれていないか（既存ファイルの書き換えのとき）
- **変更の範囲**：頼んでいないファイルが対象に入っていないか

意味が分からないときは、その場で「この変更の意図を説明して」と聞き返せます。差分が大きすぎると感じたら、依頼を小さく分けるサインです。「とりあえず承認」を繰り返すと、意図しない変更が積み重なって、あとから原因を追いにくくなります。

## 5. 終わったら git status で確かめる

作業のあとに `git status --short` を実行すると、増えたり変わったりしたファイルが、Claude Code の報告とは別に確かめられます。

```bash
git status --short
```

下は、上のセッションを終えたあとの結果です。増えたファイルは README.md だけです。

![Claude Code を終了したあとに git status --short を実行した画面。README.md が新しいファイルとして表示されている](https://crystal-method.com/wp2020/wp/wp-content/uploads/2026/10/claude-code-getting-started-07-git-status-m-20261012.png)

## 6. 間違えて承認したとき

Claude Code は、依頼を1つ送るたびにファイルの状態を控えています（チェックポイント）。入力欄が空の状態で `/rewind` と入力するか Esc を2回押すと、戻したい時点を選べます。ファイルと会話の両方、会話だけ、ファイルだけ、から選べます（公式ドキュメント「Checkpointing」、2026年10月16日確認）。

## まとめ

- Shift+Tab で manual mode にして、確認が入る状態にする
- 最初は「フォルダに何があるか」のような読み取りの依頼から
- 書き込みは、+ の行・− の行・変更の範囲の3点を読んでから承認する
- 終わったら `git status --short` で、増減したファイルを確かめる

- 始め方の全体（インストールから CLAUDE.md、権限モードまで）は → [Claude Code の始め方（初心者が最初にやること）](https://crystal-method.com/blog/claude-code-getting-started/)
- Claude Code でできること・料金の全体像は → [Claude Code の入門ガイド](https://crystal-method.com/blog/claude-code/)
