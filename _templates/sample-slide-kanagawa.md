---
marp: true
theme: kanagawa-wave
lang: ja
paginate: true
html: true
header: '<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script><script>mermaid.initialize({startOnLoad:true});</script>'
footer: ''
---

<!-- _class: lt-title -->
<!-- _paginate: false -->

# スライドタイトル

<div class="lt-bottom">
<div class="lt-profile">

![avatar w:60px h:60px](./images/avatar.jpg)

<div class="lt-info">

**名前** ![github w:16px](./images/github-icon.png)github_id　![x w:14px](./images/x-icon.png)x_id

</div>
</div>
<span class="lt-hashtag">#ハッシュタグ</span>
</div>

---

<!-- _class: section -->
<!-- _paginate: false -->

## セクション区切り

章や大きなトピックの切り替わりに使用。背景色が変わる。

---

# 基本レイアウト（デフォルト）

h1 がヘッダーとして左上に固定される。

## h2 見出し（クリスタルブルー）

通常のテキストと **強調（オレンジ）** と `インラインコード` と [リンク](https://example.com)

- リスト項目1
- リスト項目2
  - ネストしたリスト
- リスト項目3

1. 番号付き1
2. 番号付き2

---

# テーブル

| 項目 | 説明 | 評価 |
|---|---|---|
| **項目A** | 説明テキスト | ◎ |
| 項目B | 説明テキスト | ○ |
| 項目C | 説明テキスト | △ |

---

# コードブロック

```sql
with base as (
  select
    id,
    ai.generate(
      'このレビューを評価してください: ' || review_text,
      output_schema => 'sentiment STRING, score INT64'
    ) as ai_res
  from reviews
)
select id, ai_res.sentiment, ai_res.score from base
```

> blockquote（バイオレット左ボーダー）

---

# Mermaidダイアグラム

<div class="mermaid">
flowchart LR
    A[(入力テーブル)]
    B{処理}
    C[(出力テーブル)]
    A --> B --> C
</div>

説明テキスト。

---

# 2カラムレイアウト

<div class="two-col">
<div>

## 左カラム

- 項目1
- 項目2
- 項目3

</div>
<div>

## 右カラム

| 関数 | 用途 |
|---|---|
| 関数A | 説明 |
| 関数B | 説明 |

</div>
</div>

---

<!-- _class: no-header -->

# ヘッダーなし（no-header）

このスライドではヘッダーが非表示になる。フルスクリーンでコンテンツを見せたい場合に便利。

---

<!-- _class: no-header -->

<style scoped>
section {
  justify-content: center;
  text-align: center;
}
</style>

# <!--fit--> **クロージングメッセージ**

---

<!-- _class: lt-title -->
<!-- _paginate: false -->

<style scoped>
section.lt-title {
  justify-content: center;
  align-items: center;
  text-align: center;
  gap: 48px;
}
.fin-profile {
  display: flex;
  align-items: center;
  gap: 16px;
}
.fin-profile p {
  margin: 0;
  text-align: left;
  color: rgba(255,255,255,0.85);
  font-size: 20px;
}
</style>

# Thank you

<div class="fin-profile">

![avatar w:64px h:64px](./images/avatar.jpg)

<p><strong>名前</strong><br>
<img src="./images/github-icon.png" width="16"> github_id　<img src="./images/x-icon.png" width="14"> x_id</p>

</div>
