---
marp: true
theme: marp-tailwind
class: cover
paginate: false
---

# BigQuery AI.GENERATE関数の活用
# コスト管理と実践的な使い方

<div class="author">

データエンジニア

</div>

---
class: content
---

<!-- _footer: 2 -->

# はじめに

## BigQueryでの生成AIの課題

- 外部API呼び出しはコストと管理が複雑
- データセキュリティとガバナンスの懸念
- スケーラビリティとパフォーマンスの問題
- SQLだけで完結したい

**AI.GENERATE関数**がこれらを解決する

---
class: content
---

<!-- _footer: 3 -->

# AI.GENERATE関数とは

## 概要

- BigQueryネイティブの生成AI関数
- SQL内で直接LLMを呼び出し可能
- Vertex AIのモデルを統合利用
- マルチモーダル対応（テキスト、画像、動画）

## 主な特徴

- **シンプル**: SQLだけで完結
- **セキュア**: データはGCP内で処理
- **スケーラブル**: BigQueryの分散処理を活用

---

<!-- _footer: 4 -->

# セットアップ

## 1. プロジェクト設定

```sql
-- Vertex AI APIの有効化が必要
-- GCPコンソールで Vertex AI API を有効化

-- リモート接続の作成
CREATE OR REPLACE MODEL `project.dataset.gemini_model`
REMOTE WITH CONNECTION `us.gemini-connection`
OPTIONS (endpoint = 'gemini-1.5-flash');
```

## 2. 必要な権限

- `aiplatform.endpoints.predict`
- `bigquery.connections.use`

---

<!-- _footer: 5 -->

# 基本的な使い方

```sql
-- テキスト生成の基本例
SELECT
  ML.GENERATE_TEXT(
    MODEL `project.dataset.gemini_model`,
    '日本の四季について100文字で説明してください',
    STRUCT(
      0.7 AS temperature,        -- 生成の多様性
      100 AS max_output_tokens,  -- 最大トークン数
      'text/plain' AS mime_type  -- 出力形式
    )
  ) AS generated_text;
```

**ポイント**: temperature で創造性を調整（0.0-1.0）

---

<!-- _footer: 6 -->

# 構造化出力の活用

<div class="two-columns">

<div class="column">

### 通常のテキスト出力

```sql
SELECT ML.GENERATE_TEXT(
  MODEL model_name,
  'ユーザー情報を要約',
  STRUCT('text/plain' AS mime_type)
);
```

**結果**: 自由形式テキスト

</div>

<div class="column">

### JSON構造化出力

```sql
SELECT ML.GENERATE_TEXT(
  MODEL model_name,
  'ユーザー情報を要約',
  STRUCT('application/json' AS mime_type)
);
```

**結果**: パース可能なJSON

</div>

</div>

**推奨**: データ処理には`application/json`を使用

---

<!-- _footer: 7 -->

# 類似関数との使い分け（生成系）

| 機能 | AI.GENERATE | ML.GENERATE_TEXT | 外部API |
|------|-------------|------------------|---------|
| 統合性 | ネイティブ | モデル定義必要 | 外部依存 |
| セットアップ | 最小限 | 中 | 複雑 |
| コスト | 低 | 低-中 | 高 |
| レイテンシ | 低 | 低 | 高 |
| データ移動 | なし | なし | あり |

**推奨**: 新規実装は`AI.GENERATE`を優先

---

<!-- _footer: 8 -->

# 類似関数との使い分け（マネージド型）

| 項目 | AI.GENERATE | Vertex AI Remote | Cloud Functions |
|------|-------------|------------------|-----------------|
| SQL統合 | ◎ | ◯ | △ |
| 管理負荷 | 低 | 中 | 高 |
| カスタマイズ | △ | ◯ | ◎ |
| モデル選択 | Gemini系 | 多様 | 自由 |
| コスト効率 | ◎ | ◯ | △ |

**使い分け**: シンプルな用途はAI.GENERATE、高度なカスタマイズは他の選択肢を検討

---

<!-- _footer: 9 -->

# 実用例

```sql
-- 大量データに対する感情分析
SELECT
  review_id,
  review_text,
  ML.GENERATE_TEXT(
    MODEL `project.dataset.gemini_model`,
    CONCAT('次のレビューの感情を"positive/negative/neutral"で分類: ',
           review_text),
    STRUCT(
      'application/json' AS mime_type,
      0.2 AS temperature  -- 一貫性重視
    )
  ) AS sentiment
FROM `project.dataset.customer_reviews`
LIMIT 1000;  -- バッチ処理
```

**ポイント**: 構造化出力で後続処理が容易

---

<!-- _footer: 10 -->

# コスト管理とベストプラクティス

- **コスト最適化**
  > LIMIT句で処理量を制限
  > temperature を低めに設定（不要な生成を削減）
  > max_output_tokens を適切に設定

- **パフォーマンス改善**
  > バッチ処理でまとめて実行
  > 不要な再計算を避ける（結果をテーブルに保存）

- **品質向上**
  > プロンプトを明確に記述
  > 構造化出力（JSON）を活用

---
class: center
---

# まとめ

## AI.GENERATE関数の強み

- SQLだけで生成AIを活用可能
- 低コスト・高パフォーマンス
- BigQueryのスケーラビリティを活用

## 推奨アプローチ

1. まずAI.GENERATEを試す
2. コスト管理を徹底（LIMIT、temperature調整）
3. 構造化出力で後続処理を簡素化

---

<!-- _footer: 12 -->

# 参考文献

## 公式ドキュメント

- [BigQuery AI.GENERATE関数](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text)
- [Vertex AI Gemini API](https://cloud.google.com/vertex-ai/docs/generative-ai/model-reference/gemini)

## 記事・ブログ

- [BigQuery AI関数の使い方](https://zenn.dev/topics/bigquery)
- [生成AIのコスト最適化](https://qiita.com/tags/bigquery)

## 関連リソース

- [BigQuery MLドキュメント](https://cloud.google.com/bigquery/docs/bqml-introduction)
