# サンプルスライドの取得

## kanagawa-wave テーマ

リポジトリの `_templates/` フォルダにテンプレートがある。

```
_templates/
└── sample-slide-kanagawa.md  ← kanagawa-wave 全レイアウトのサンプル
```

## classmethod テーマ

最新版はリポジトリから取得する（更新される可能性があるため毎回取得推奨）。

```bash
curl -s https://raw.githubusercontent.com/classmethod/classmethod-marp-theme/main/sample-slide.md
```

## テーマの選択

| 場面 | 推奨テーマ |
|------|-----------|
| 技術系の発表・LT | kanagawa-wave |
| クラスメソッドデザインが必要 | classmethod |
