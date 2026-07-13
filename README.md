# vc-ja-optimizer-pg

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-d97757.svg)](https://code.claude.com/docs/en/skills)

音声入力した日本語を、そのまま使えるテキストに整形する Claude Code スキルです。誤変換・誤認識・フィラーを修正し、クリップボードにコピーした上で、その内容を Claude Code への命令として実行するかどうかを選べます。

> A Claude Code skill that cleans up Japanese voice-input text — fixes mis-recognitions, removes fillers, copies the result to the clipboard, and optionally runs it as a Claude Code instruction.

## デモ

```
/vc-ja-optimizer-pg えーと私は。スプリングブートで開発しています。そのクライアント側はネクストジェイエスです。ドッカーを使って〇〇しましょう。まずリードミーを直してほしいな。
```

上の入力は次のように整形・表示されます（クリップボードにもコピー済み）。

```
私はSpring Bootで開発しています。
そのクライアント側はNext.jsです。
Dockerを使って〇〇しましょう。
まずREADMEを直してほしいです。
```

最後に「この内容で実行しますか？」と確認され、次のどちらかを選べます。

1. **Claude Codeに命令として送る** — 整形後のテキストをそのまま指示として実行
2. **プロンプト表示のみ** — 何もせず終了（テキストはクリップボードにあるので手動で貼り付けて使える）

## 特徴

- **変換辞書**: よくある誤変換（「クロードコード → Claude Code」など）は [dictionary.md](dictionary.md) をもとに変換されます。語の追加はテーブルに1行足すだけです。
- **プロジェクト辞書**: 各プロジェクトの `.claude/vc-ja-dictionary.md` に同じ形式の表を置くと、プロジェクト特有の用語が共通辞書より優先して変換されます。
- **意味は変えない**: フィラー除去・誤変換修正・句読点の整理のみを行い、内容の追加・要約・言い換えはしません。
- **Sonnet で実行**: 整形処理は軽いタスクなので、フロントマターの `model: sonnet` で高速・低コストに実行されます。

## インストール

`~/.claude/skills/` にクローンするだけです。

```bash
git clone https://github.com/lacode-lab/vc-ja-optimizer-pg.git ~/.claude/skills/vc-ja-optimizer-pg
```

新しいセッションを開くと `/vc-ja-optimizer-pg` が使えるようになります（どのプロジェクトでも利用可能）。

## 使い方

```
/vc-ja-optimizer-pg <音声入力したテキスト>
```

音声入力ツール（macOS の音声入力、Superwhisper など）でプロンプト欄に吹き込んだテキストを、そのままこのスキルの引数として送るのが想定フローです。

### プロジェクト辞書の例

プロジェクトルートに `.claude/vc-ja-dictionary.md` を作成します。

```markdown
| 誤変換・音声認識結果 | 正しい表記 |
|---|---|
| らこーど | lacode |
| バックエンドエーピーアイ | backend-api |
```

## 動作要件

- Claude Code（スキル対応バージョン）
- クリップボードコピーは macOS（`pbcopy`）のみ。他 OS でも整形・表示は動作し、コピーだけスキップされます。

## ライセンス

[MIT](LICENSE)
