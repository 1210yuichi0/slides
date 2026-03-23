# Slides

技術発表・勉強会用スライド資料のリポジトリ（Marp形式）

## プロジェクト一覧

| フォルダ | タイトル | テーマ |
|---------|---------|--------|
| [bigquery-ai-generate](./bigquery-ai-generate/) | データ分析基盤でGeminiを使う話 | kanagawa-wave |

## テーマ

| テーマ名 | ファイル | 概要 |
|---------|---------|------|
| `kanagawa-wave` | `themes/kanagawa-wave.css` | ダーク系テーマ（デフォルト） |
| `classmethod` | `themes/classmethod.css` | クラスメソッドデザイン（ライト系） |

Front matter で指定:

```yaml
---
marp: true
theme: kanagawa-wave   # または classmethod
paginate: true
---
```

## テンプレート

`_templates/` フォルダにサンプルスライドがある。

| ファイル | 説明 |
|---------|------|
| `_templates/sample-slide-kanagawa.md` | kanagawa-wave 全レイアウトのサンプル |

## スライド作成（marp-creator スキル）

`/marp-creator` スキルを使うと3ステップで資料を作成できる。

```
メモ → 01_organized_*.md → 02_slide-design_*.md → 03_marp_*.md
```

### フォルダ構成（推奨）

```
{発表名}/
├── {発表名}.md              ← 最終成果物
├── {発表名}.pdf             ← ビルド成果物（gitignore対象）
├── images/                  ← 画像ファイル
├── docs/                    ← アウトライン・設計書
└── archive/                 ← 過去バージョン
```

## ビルド

```bash
# HTML 出力
npx @marp-team/marp-cli {input}.md --output {output}.html --html --theme themes/kanagawa-wave.css

# ウォッチモード（編集しながらプレビュー）
npx @marp-team/marp-cli --watch {input}.md --output {output}.html --html --theme themes/kanagawa-wave.css

# PDF 出力（推奨）
# Mermaid使用時も含め、これを標準コマンドとして使う
npx @marp-team/marp-cli --pdf --html --allow-local-files "{input}.md" -o "{output}.pdf" --theme themes/kanagawa-wave.css --no-stdin
```

## VS Code

[Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode) 拡張をインストールすると `.md` ファイルを開いた際に自動でプレビューが表示される。
