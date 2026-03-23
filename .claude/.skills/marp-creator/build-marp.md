# Marp資料作成（ステップ3）

スライド設計書からMarp形式の最終資料を生成する。

## 入力

`02_slide-design_[資料名].md`

## 出力

`03_marp_[資料名].md`

## テーマ別 Front matter

### kanagawa-wave（デフォルト）

```yaml
---
marp: true
theme: kanagawa-wave
paginate: true
header: ''
footer: ''
---
```

### classmethod

```yaml
---
marp: true
theme: classmethod
paginate: true
title: [資料タイトル]
description: [資料の説明]
---
```

## テーマ別 表紙スライド

### kanagawa-wave

```markdown
<!-- _class: lead -->
<!-- _paginate: false -->

# [資料タイトル]

[サブテキスト・日付・所属など]
```

### classmethod

```markdown
<!-- _class: title -->
<!-- _paginate: false -->

![classmethod-logo w:400px](https://classmethod.jp/wp-content/themes/cmn/assets/images/common/logo_classmethod.svg)

# [資料タイトル]

[日付や執筆者情報]
```

## テーマ別 セクションスライド

```markdown
---

<!-- _class: section -->
<!-- _paginate: false -->

## [セクション名]
[説明文]
```

（kanagawa-wave / classmethod 両方同じ記法）

## 通常スライド

```markdown
---

# [スライドタイトル]

## [サブ見出し]

- ポイント1
- ポイント2
- ポイント3

**強調テキスト**（kanagawa-wave: オレンジ / classmethod: グレー）
```

## カラムレイアウト（classmethod のみ）

```markdown
---

<!-- _class: column-layout -->

# [タイトル]

<div class="column">

## 左
- 項目1

</div>

<div class="column">

## 右
- 項目1

</div>
```

## ワークフロー

1. 設計書を読み込み、テーマを確認
2. テンプレートは `_templates/` を参照（`_templates/sample-slide-kanagawa.md` または classmethod テーマなら `curl` で取得）
3. Front matter を作成
4. 表紙スライドを作成
5. 各スライドをレイアウトクラス付きで生成
6. ページネーション制御を適用（lead/title/section は false）
7. `03_marp_[資料名].md` を出力

## ビルドコマンド

```bash
# HTML
npx @marp-team/marp-cli [入力].md --html -o [出力].html

# PDF
npx @marp-team/marp-cli [入力].md --pdf -o [出力].pdf

# ウォッチモード
npx @marp-team/marp-cli --watch [入力].md --html -o [出力].html
```
