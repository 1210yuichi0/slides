---
marp: true
theme: kanagawa-wave
lang: ja
paginate: true
html: true
header: ''
footer: ''
---

<script type="module">
import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';
mermaid.initialize({ startOnLoad: false, theme: 'default' });

const svgToDataURL = (svg) =>
  'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(svg);

window.addEventListener('DOMContentLoaded', async () => {
  const targets = document.querySelectorAll('pre.mermaid');
  let n = 0;
  for (const el of targets) {
    const code = (el.textContent ?? '').replace(/\u00A0/g, ' ').replace(/[\u200B\uFEFF]/g, '').trim();
    if (!code) continue;
    const id = (crypto?.randomUUID ? `m-${crypto.randomUUID()}` : `m-${Date.now()}-${n++}`);
    try {
      const { svg } = await mermaid.render(id, code);
      const img = document.createElement('img');
      img.src = svgToDataURL(svg);
      img.className = el.className;
      const style = el.getAttribute('style');
      if (style) img.setAttribute('style', style);
      el.replaceWith(img);
    } catch (e) {
      const warn = document.createElement('div');
      warn.style.cssText = 'padding:.5rem;border-left:4px solid #e53935;background:#fdecea;color:#b71c1c;';
      warn.textContent = `Mermaid error: ${e?.message || e}`;
      el.replaceWith(warn);
    }
  }
});
</script>

<!-- _class: lt-title -->
<!-- _paginate: false -->

# データ分析基盤で Gemini を使う話

<div class="lt-bottom">
<div class="lt-profile">

![avatar w:60px h:60px](./images/avatar.jpg)

<div class="lt-info">

**Yuichi** ![github w:16px](./images/github-icon.png)1210yuichi0　![x w:14px](./images/x-icon.png)1210yuichi0

</div>
</div>
<span class="lt-hashtag">#GoogleAIMeetupOsaka</span>
</div>

---

# この10分でわかること

- ## BigQuery の AI.GENERATE関数とは
- ## 関数ファミリーとユースケース
- ## システムに組み込む際の設計ポイント
- ## 使ってみた感想

---

# こんなニーズ、ありませんか？

- システムに溜まった大量のテキストデータ、どう扱っていますか？
- カスタマーレビューのネガポジを全件チェックしたい
- 人手でのラベリングはもう限界

## ▶︎ 「全レコードをGeminiで評価できたら？」

<pre class="mermaid">
flowchart LR
    DB[(システムDB)]
    AI{生成AI + プロンプト}
    DB -->|分析対象の抽出|AI
    AI -->|評価・属性の付与|DB
</pre>

---

# AI.GENERATE関数 とは

## BigQueryからVertex AI経由でGeminiを呼び出せる関数

<pre class="mermaid">
flowchart LR
    BQ[(BigQueryテーブル)]
    FN[AI.GENERATE関数]
    VAI[Vertex AI]
    GM{Gemini}
    OUT[(評価結果 型付き)]
    BQ --> FN --> VAI --> GM --> OUT
</pre>

1. **SQLだけで完結** 
    - BigQueryの中で処理実行が完結する
2. **全件一括処理**
    - テーブルの全レコードをGeminiが1行ずつ評価
3. **型付き出力**
    - output_schemaでSTRUCT型として受け取れるので後続処理がシンプル

---

# 実際にやってみた
SQLだけで完結できる
```sql
with base as (
    select
        text_column,
        ai.generate(
            'レビューから感情スコアと評価理由を分析して' || text_column,
            output_schema => 'score INT64, reason STRING'
        ) as ai_res
    from reviews
)
select text_column, ai_res.score, ai_res.reason from base
```
こんな結果が得られる

| レビュー文 | score | reason |
|---|---|---|
| 「デザインが非常に洗練されていて…初期設定に少し時間がかかったのが残念です。」 | 80 | デザインは高評価だが、セットアップの難易度がマイナス要因 |
| 「注文してから届くまでが遅すぎます…二度と利用しません。」 | 15 | 配送遅延・梱包の不満、リピート意向も低い |
| 「コスパ最高です！この価格でこの機能性は文句なし。」 | 95 | 価格・機能・操作性すべてにポジティブな評価 |

---

# ラベリング後のデータをBIダッシュボードに繋げると

<pre class="mermaid">
flowchart LR
    TXT[/テキスト 非構造化/]
    AI{AI.GENERATE関数}
    TBL[(評価テーブル score列など)]
    BI[BIダッシュボード]
    TXT --> AI --> TBL --> BI
</pre>

  ▶︎ **ネガティブレビューの急増をすぐ検知できる**

<div style="width: 28%; margin: 0 auto;">
<pre class="mermaid">
%%{init: {'themeVariables': {'xyChart': {'plotColorPalette': '#E85C4C'}}}}%%
xychart-beta
    title "ネガティブレビュー比率（月次推移）"
    x-axis ["1月", "2月", "3月", "4月", "5月", "6月"]
    y-axis "ネガティブ率 (%)" 0 --> 50
    line [20, 30, 16, 39, 13, 10]
</pre>
</div>

- 4月に急増 → **アラートで即検知・対応できる**
- AIラベリング前は「なんとなく悪い気がする」で終わっていたものが、数値で追えるようになる

---

# 型付きで返ってくるから後処理が楽

## 旧（ML.GENERATE_TEXT関数）
- 出力は固定のJSON文字列
- 型が不確定 → パース処理が必要

## 新（AI.GENERATE関数）

```sql
ai.generate(
  prompt,
  output_schema => 'sentiment STRING, score INT64'
)
```

→ 指定した型のSTRUCTで返ってくる

**対応型**：STRING、INT64、FLOAT64、BOOL、ARRAY、STRUCT

---

# 用途に応じてGeminiモデルを指定可能
SQLの中で、使用モデルを指定可能
```sql
select
  ai.generate(
    ~,
    endpoint => 'gemini-2.5-flash',
```
状況に応じたモデル選択
| モデル | コスト | 速度 | おすすめ用途 |
|---|---|---|---|
| gemini-2.5-pro | 高 | 遅い | 複雑な推論が必要な場合 |
| gemini-2.5-flash | 低 | 速い | 通常の分類・ラベリング |

- **分類・ラベリング用途なら flash で十分**
- **モデルのライフサイクルは注視**

---

# 目的別に使い分けるAI関数

<div class="two-col">
<div>

### 汎用

| 関数 | 用途 |
|---|---|
| **AI.GENERATE** | 要約・翻訳・抽出・分類。output_schema で型指定 |
| AI.GENERATE_TEXT | テーブル→テーブル。Claude等も利用可 |
| AI.GENERATE_TABLE | 複数項目の構造化抽出 |
| AI.GENERATE_BOOL/INT/DOUBLE | 型を絞ったシンプル版 |

</div>
<div>

### マネージド（軽量）

| 関数 | 用途 |
|---|---|
| AI.IF | WHERE / JOIN に直書き |
| AI.SCORE | スコアリング（内部最適化あり） |
| AI.CLASSIFY | ラベルリストを渡すだけ |

### エンベディング

| 関数 | 用途 |
|---|---|
| AI.EMBED | テキスト/画像→ベクトル |
| AI.SIMILARITY | 類似度スコア |
| AI.SEARCH | セマンティック検索 |

</div>
</div>

---

# ユースケース①：レビュー感情分析

```sql
with customer_reviews as (
  select 101 as review_id, 'デザインが洗練されていて最高です！ただ、バッテリーの持ちがもう少し良ければ完璧でした。' as review_text union all
  select 102, '期待外れ。動作が重すぎて使い物になりません。返品を検討しています。' union all
  select 103, '普通の製品です。可もなく不可もなくといったところ。'
)
select
  review_id,
  ai.generate(
    concat('このレビューの感情を分析してください: ', review_text),
    output_schema => 'sentiment STRING, confidence_score FLOAT64, key_points ARRAY<STRING>',
    endpoint => 'gemini-2.5-flash'
  ).*
from customer_reviews
```

非構造化テキストが型付きの構造化データに変わる。`sentiment` 列でそのままBIのフィルタ・集計に使える

| id | sentiment | confidence_score | key_points |
|---|---|---|---|
| 101 | mixed | 0.95 | ["洗練されたデザイン", "バッテリーの持ち"] |
| 102 | negative | 0.95 | ["期待外れ", "動作が重すぎる", "返品を検討"] |
| 103 | neutral | 0.95 | ["普通の製品", "可もなく不可もなく"] |

---

# ユースケース②：異常コメント抽出

```sql
with user_comments as (
  select 1 as comment_id, 'この記事、非常に参考になりました！ありがとうございます。' as comment_text union all
  select 2, '【限定】スマホで月収100万円！？今すぐこちらのURLをクリック！ http://example.com' union all
  select 3, 'この作者はアホなのか？レベルが低すぎて話にならない。'
)
select
  comment_id,
  ai.generate(
    concat('このコメントがスパム・誹謗中傷・不適切な内容かどうか判定してください: ', comment_text),
    output_schema => 'is_anomalous BOOL, reason STRING, severity INT64',
    endpoint => 'gemini-2.5-flash'
  ).*
from user_comments
```

`reason` で判定根拠も取得。`severity` でモデレーション優先度をBI上に可視化できる

| id | is_anomalous | reason | severity |
|---|---|---|---|
| 1 | false | 肯定的なフィードバック。問題なし | 0 |
| 2 | true | 高収入を謳いURLクリックを促す典型的なスパム | 5 |
| 3 | true | 誹謗中傷・侮辱・攻撃的な言葉を含む | 5 |

---

# データパイプラインに組み込む

<pre class="mermaid">
flowchart LR
    PM[(プロンプト管理テーブル)]
    SRC[(ソーステーブル)]
    AI{AI.GENERATE関数}
    DIM[(AI評価テーブル)]
    FACT[(ファクトテーブル)]
    BI[BIダッシュボード]
    PM -->  AI
    SRC -->|評価対象データのみ|AI --> DIM --> BI
    FACT --> BI
</pre>

## 責務の切り出し

- **プロンプト管理テーブル**: プロンプトをSQLにハードコードせず、マスタとして管理

- **AI評価ディメンションテーブル**: AIの出力結果を格納するテーブルを独立

- **メリット**: 後続のBIで「新しい評価項目を追加したい」という要望が出た際も、システム全体を作り直すことなく、プロンプトや評価テーブルの拡張のみで柔軟に対応可能

---

# コスト最適化と増分更新

- AI.GENERATE関数はトークン課金 — 全件再実行すると費用が跳ね上がる
- **既に評価済みのレコードはスキップ**、新規分だけ実行する増分設計が必須
- 増分更新やBigQueryのWHERE句で実現できる

## 実装の考え方

- 評価テーブルに結果が存在するレコードは処理対象から除外
- 定期バッチで1日1回実行するだけでコストを最小化

---

# 実際に触れてみて

## よかった

- SQLだけで完結。Pythonスクリプト不要
- output_schemaで型付き出力が想像以上に便利
- flashでも分類精度は十分実用的
- パイプラインに組み込んで定期実行できる

## 気をつけポイント

- プロンプトが甘いと出力がブレる → 3点セット（指示・フォーマット・判断基準）が必要
- コストのトラッキングがわかりにくい
- 増分更新を最初から設計しておかないと後で痛い目を見る
- 生成AIが判定できないケースも許容する
  
---

<!-- _class: no-header -->

<style scoped>
section {
  justify-content: center;
  text-align: center;
}
</style>

# SQLで全レコードをGeminiで評価できる時代に

# <!--fit--> **「AI.GENERATE関数、使ってみませんか？」**

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
img[alt~="avatar"] {
  border-radius: 50%;
  object-fit: cover;
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

<p><strong>Yuichi</strong><br>
<img src="./images/github-icon.png" width="16"> 1210yuichi0　<img src="./images/x-icon.png" width="14"> 1210yuichi0</p>

</div>
