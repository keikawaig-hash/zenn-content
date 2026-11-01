---
title: "Windows の Claude Code：ネイティブ版か WSL 2 かは「プロジェクトの置き場所」で決める"
emoji: "🤖"
type: "tech"
topics: ["ClaudeCode", "Windows", "WSL", "AI"]
published: true
---

## この記事で扱うこと

Windows で Claude Code を使うとき、ネイティブ Windows で動かすか、WSL 2 で動かすかで迷う人は多いと思います。この記事では、公式ドキュメントの情報をもとに、**プロジェクトを置く場所で選ぶ**という判断の仕方を1つだけ説明します。情報は2026年10月時点の公式ドキュメントによります。

## 公式が示す2つの判断材料

公式のセットアップの案内では、選ぶ材料は次の2つです。

| 環境 | 追加で必要なもの | サンドボックス | 向いている用途 |
| --- | --- | --- | --- |
| ネイティブ Windows | なし（Git for Windows は任意） | 非対応 | Windows 向けのプロジェクトやツールを使う開発 |
| WSL 2 | WSL 2 の有効化 | 対応 | Linux のツールを使う開発、コマンドをサンドボックス内で動かしたい場合 |
| WSL 1 | WSL 1 の有効化 | 非対応 | WSL 2 が使えない環境での代替 |

![公式ドキュメント Set up on Windows の表。ネイティブ Windows・WSL 2・WSL 1 の追加要件とサンドボックス対応が並んでいる](https://crystal-method.com/wp2020/wp/wp-content/uploads/2026/10/cc-windows-docs-native-vs-wsl.jpg)

サンドボックスに対応するのは WSL 2 だけです。つまり、コマンドをサンドボックスの中で動かしたいなら WSL 2、そうでなければネイティブ Windows、というのが第一の分け方です。

## もう1つの材料：ファイルの置き場所

サンドボックスが要らない場合でも、WSL 2 を選ぶときには注意が1つあります。公式のトラブルシューティングによると、WSL から Windows 側のファイルシステム（/mnt/c/ 以下）を扱うと、ディスクの読み取りが遅くなります。その結果、Claude Code の検索が期待より少ない件数しか見つけられないことがあります。検索自体は動くので、claude doctor は検索を「OK」と表示します。

公式が挙げる対処は3つです。

1. **プロジェクトを Linux 側に置く**：WSL のホーム（/home/ユーザー名/ 以下）に置き、そこで claude を起動する。公式が最初に勧める対処です
2. **検索の範囲を絞って頼む**：フォルダやファイルの種類を指定して探させる
3. **ネイティブ Windows で動かす**：Windows 側のフォルダ（C:\Users\名前\projects など）が仕事場なら、WSL を経由しないほうがファイルシステムの性能面で有利、という案内です

## 判断のまとめ

3つ目の対処から、次のように整理できます。

- **プロジェクトが Windows 側のフォルダにある** → ネイティブ Windows
- **プロジェクトを Linux 側に置く／置ける** → WSL 2
- **サンドボックスが必要** → WSL 2（プロジェクトは Linux 側に置く）

まず「どこにプロジェクトがあるか」を確かめ、次に「サンドボックスが要るか」を確かめる、という順で考えると迷いません。

## インストールのときの注意

WSL 2 を選んだ場合、インストールのコマンドは **WSL のターミナルの中で**実行します。PowerShell や CMD のウィンドウからではありません。

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

ネイティブ Windows の場合は、PowerShell で次の1行を実行します。管理者権限も Node.js も不要です。

```powershell
irm https://claude.ai/install.ps1 | iex
```

## まとめ

- サンドボックスが要るなら WSL 2、要らないならプロジェクトの置き場所に合わせる
- Windows 側のフォルダが仕事場なら、ネイティブ Windows が公式の案内
- WSL 2 なら、プロジェクトは /home/ 側に置く

エラーの直し方や、更新・アンインストールの手順は、元の解説記事にまとめています。

## 関連リンク

- [Claude Code を Windows で始める手順（元記事）](https://crystal-method.com/blog/claude-code-windows/)
- [Claude Code の入門ガイド](https://crystal-method.com/blog/claude-code/)
