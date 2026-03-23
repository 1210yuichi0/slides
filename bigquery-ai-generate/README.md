# BigQuery AI.GENERATE関数の活用

LT資料（Marp形式）

## ファイル構成

```
.
├── slides.md                    # Marpスライド（メイン）
├── theme/
│   └── kanagawa-wave.css        # カスタムテーマ
├── images/
│   └── architecture-flow.svg    # アーキテクチャ図
├── docs/
│   ├── outline.md               # 技術的裏取り・構成メモ
│   └── architecture-flow.mmd    # Mermaidソースコード
├── archive/                     # 古いバージョン
└── README.md                    # このファイル
```

## スタイル

- **カラーテーマ**: Kanagawa Wave（VSCodeテーマに準拠）
- **フォントサイズ**: 本文24px, タイトル36px (lead: 48px), 見出し28px, コード16px

## PDFのエクスポート

```bash
npx -y @marp-team/marp-cli@latest slides.md --pdf --allow-local-files -o slides.pdf
```

## スライド構成（全13ページ）

1. カバー
2. こんなニーズありませんか？
3. BigQueryからGeminiを呼び出す
4. GENERATE関数ファミリーの全種類
5. output_schemaの威力
6. 実践例1: レビューの自動評価
7. 実践例2: ログデータの異常検知
8. dbtでの増分更新パターン
9. 管理型AI関数との組み合わせ
10. システム組み込みのベストプラクティス
11. 他手法との比較
12. まとめ
13. 参考リンク
