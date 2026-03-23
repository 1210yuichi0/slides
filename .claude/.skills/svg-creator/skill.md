---
skill: svg-creator
description: SVGアイコン・図表の自動生成（kanagawa-wave テーマカラーパレット準拠）
version: 1.1.0
trigger: manual
---

# SVG Creator

Marpスライド用のSVGアイコン、フローチャート、グラフを自動生成するスキルです。
kanagawa-wave テーマのカラーパレットに準拠し、Marpへの直接埋め込みに最適化されています。

## 機能

### 1. アイコン生成（20種類）
- チェックマーク
- エラー
- 警告
- 情報
- 矢印（上下左右）
- クラウド
- データベース
- サーバー
- ユーザー
- 設定
- ダウンロード/アップロード
- 検索
- グラフ
- セキュリティ
- 通知

### 2. フローチャート生成
- 開始/終了ノード
- 処理ノード
- 判断ノード
- データノード
- 矢印/コネクタ
- 自動レイアウト

### 3. データビジュアライゼーション
- 棒グラフ
- 折れ線グラフ
- 円グラフ
- 積み上げグラフ

## 使用方法

```bash
# アイコン生成
claude code "svg-creator icon check --color accent1 --size 64"

# フローチャート生成
claude code "svg-creator flowchart process.yaml"

# グラフ生成
claude code "svg-creator chart bar data.json"
```

## カラーパレット（kanagawa-wave テーマ準拠）

```typescript
const COLORS = {
  bg: {
    base:    '#1F1F28',  // ベース背景
    surface: '#2A2A37',  // サーフェス
    code:    '#16161D',  // コード背景
  },
  fg: {
    default: '#DCD7BA',  // デフォルトテキスト
    muted:   '#727169',  // ミュートテキスト
    border:  '#54546D',  // ボーダー
  },
  accent: {
    blue:    '#7FB4CA',  // ブルー（h1）
    indigo:  '#7E9CD8',  // インディゴ（h2・ボーダー）
    orange:  '#FFA066',  // オレンジ（strong）
    purple:  '#957FB8',  // パープル（blockquote）
    green:   '#98BB6C',  // グリーン（成功）
    red:     '#E46876',  // レッド（エラー）
    yellow:  '#C0A36E',  // イエロー（警告）
    teal:    '#7AA89F',  // ティール
  }
};
```

## アイコンライブラリ

### 1. チェックマーク（成功）

```typescript
function generateCheckIcon(size = 64, color = COLORS.accent.accent6): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <circle cx="32" cy="32" r="30" fill="${color}" />
  <path d="M 20 32 L 28 40 L 44 24" stroke="white" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round" />
</svg>`;
}
```

**Markdown埋め込み**:
```markdown
![check icon](data:image/svg+xml;base64,PHN2ZyB4bWxucz0i...)
```

### 2. エラー（×マーク）

```typescript
function generateErrorIcon(size = 64, color = COLORS.accent.accent2): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <circle cx="32" cy="32" r="30" fill="${color}" />
  <path d="M 22 22 L 42 42 M 42 22 L 22 42" stroke="white" stroke-width="4" stroke-linecap="round" />
</svg>`;
}
```

### 3. 警告（！マーク）

```typescript
function generateWarningIcon(size = 64, color = COLORS.accent.accent4): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="M 32 4 L 60 56 L 4 56 Z" fill="${color}" />
  <path d="M 32 20 L 32 36" stroke="black" stroke-width="3" stroke-linecap="round" />
  <circle cx="32" cy="46" r="2" fill="black" />
</svg>`;
}
```

### 4. 情報（ i マーク）

```typescript
function generateInfoIcon(size = 64, color = COLORS.accent.accent1): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <circle cx="32" cy="32" r="30" fill="${color}" />
  <circle cx="32" cy="20" r="3" fill="white" />
  <path d="M 32 28 L 32 48" stroke="white" stroke-width="4" stroke-linecap="round" />
</svg>`;
}
```

### 5-8. 矢印（上下左右）

```typescript
function generateArrowIcon(direction: 'up' | 'down' | 'left' | 'right', size = 64, color = COLORS.accent.accent5): string {
  const paths = {
    up: 'M 32 12 L 32 52 M 32 12 L 20 24 M 32 12 L 44 24',
    down: 'M 32 52 L 32 12 M 32 52 L 20 40 M 32 52 L 44 40',
    left: 'M 12 32 L 52 32 M 12 32 L 24 20 M 12 32 L 24 44',
    right: 'M 52 32 L 12 32 M 52 32 L 40 20 M 52 32 L 40 44'
  };

  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="${paths[direction]}" stroke="${color}" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round" />
</svg>`;
}
```

### 9. クラウド

```typescript
function generateCloudIcon(size = 64, color = COLORS.accent.accent1): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="M 16 38 Q 8 38 8 30 Q 8 22 16 22 Q 16 14 24 14 Q 32 14 32 22 Q 40 18 48 22 Q 56 26 56 34 Q 56 42 48 42 Z"
        fill="${color}" />
</svg>`;
}
```

### 10. データベース

```typescript
function generateDatabaseIcon(size = 64, color = COLORS.accent.accent5): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <ellipse cx="32" cy="16" rx="20" ry="6" fill="${color}" />
  <rect x="12" y="16" width="40" height="32" fill="${color}" />
  <ellipse cx="32" cy="48" rx="20" ry="6" fill="${color}" />
  <ellipse cx="32" cy="16" rx="20" ry="6" fill="none" stroke="white" stroke-width="1" />
  <line x1="12" y1="32" x2="52" y2="32" stroke="white" stroke-width="1" />
</svg>`;
}
```

### 11. サーバー

```typescript
function generateServerIcon(size = 64, color = COLORS.secondary.dark): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <rect x="12" y="12" width="40" height="12" rx="2" fill="${color}" />
  <rect x="12" y="26" width="40" height="12" rx="2" fill="${color}" />
  <rect x="12" y="40" width="40" height="12" rx="2" fill="${color}" />
  <circle cx="20" cy="18" r="2" fill="${COLORS.accent.accent6}" />
  <circle cx="20" cy="32" r="2" fill="${COLORS.accent.accent6}" />
  <circle cx="20" cy="46" r="2" fill="${COLORS.accent.accent6}" />
</svg>`;
}
```

### 12. ユーザー

```typescript
function generateUserIcon(size = 64, color = COLORS.accent.accent1): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <circle cx="32" cy="20" r="10" fill="${color}" />
  <path d="M 12 56 Q 12 36 32 36 Q 52 36 52 56 Z" fill="${color}" />
</svg>`;
}
```

### 13. 設定（ギア）

```typescript
function generateSettingsIcon(size = 64, color = COLORS.secondary.dark): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="M 32 8 L 36 16 L 44 16 L 48 24 L 56 28 L 56 36 L 48 40 L 44 48 L 36 48 L 32 56 L 28 48 L 20 48 L 16 40 L 8 36 L 8 28 L 16 24 L 20 16 L 28 16 Z"
        fill="${color}" />
  <circle cx="32" cy="32" r="8" fill="white" />
</svg>`;
}
```

### 14-15. ダウンロード/アップロード

```typescript
function generateDownloadIcon(size = 64, color = COLORS.accent.accent1): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="M 32 12 L 32 44 M 32 44 L 20 32 M 32 44 L 44 32" stroke="${color}" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round" />
  <line x1="12" y1="52" x2="52" y2="52" stroke="${color}" stroke-width="4" stroke-linecap="round" />
</svg>`;
}

function generateUploadIcon(size = 64, color = COLORS.accent.accent1): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="M 32 44 L 32 12 M 32 12 L 20 24 M 32 12 L 44 24" stroke="${color}" stroke-width="4" fill="none" stroke-linecap="round" stroke-linejoin="round" />
  <line x1="12" y1="52" x2="52" y2="52" stroke="${color}" stroke-width="4" stroke-linecap="round" />
</svg>`;
}
```

### 16. 検索

```typescript
function generateSearchIcon(size = 64, color = COLORS.secondary.dark): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <circle cx="28" cy="28" r="16" fill="none" stroke="${color}" stroke-width="4" />
  <line x1="40" y1="40" x2="52" y2="52" stroke="${color}" stroke-width="4" stroke-linecap="round" />
</svg>`;
}
```

### 17. グラフ（トレンド）

```typescript
function generateGraphIcon(size = 64, color = COLORS.accent.accent1): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <polyline points="8,52 20,40 32,44 44,20 56,28" fill="none" stroke="${color}" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" />
  <circle cx="20" cy="40" r="3" fill="${color}" />
  <circle cx="32" cy="44" r="3" fill="${color}" />
  <circle cx="44" cy="20" r="3" fill="${color}" />
</svg>`;
}
```

### 18. セキュリティ（鍵）

```typescript
function generateSecurityIcon(size = 64, color = COLORS.accent.accent2): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <circle cx="32" cy="24" r="10" fill="none" stroke="${color}" stroke-width="4" />
  <rect x="20" y="30" width="24" height="20" rx="2" fill="${color}" />
  <circle cx="32" cy="40" r="3" fill="white" />
</svg>`;
}
```

### 19. 通知（ベル）

```typescript
function generateNotificationIcon(size = 64, color = COLORS.accent.accent4): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="M 32 12 Q 28 12 28 16 L 28 20 Q 20 20 20 28 L 20 40 L 16 48 L 48 48 L 44 40 L 44 28 Q 44 20 36 20 L 36 16 Q 36 12 32 12 Z"
        fill="${color}" />
  <path d="M 28 48 Q 28 52 32 52 Q 36 52 36 48" fill="${color}" />
</svg>`;
}
```

### 20. ドキュメント

```typescript
function generateDocumentIcon(size = 64, color = COLORS.accent.accent1): string {
  return `<svg xmlns="http://www.w3.org/2000/svg" width="${size}" height="${size}" viewBox="0 0 64 64">
  <path d="M 16 8 L 16 56 L 48 56 L 48 20 L 36 8 Z" fill="${color}" />
  <path d="M 36 8 L 36 20 L 48 20" fill="white" />
  <line x1="24" y1="32" x2="40" y2="32" stroke="white" stroke-width="2" />
  <line x1="24" y1="40" x2="40" y2="40" stroke="white" stroke-width="2" />
</svg>`;
}
```

## フローチャート生成

### ノードタイプ

```typescript
interface FlowchartNode {
  id: string;
  type: 'start' | 'end' | 'process' | 'decision' | 'data';
  label: string;
  x: number;
  y: number;
}

interface FlowchartEdge {
  from: string;
  to: string;
  label?: string;
}

function generateFlowchart(nodes: FlowchartNode[], edges: FlowchartEdge[], width = 800, height = 600): string {
  let svg = `<svg xmlns="http://www.w3.org/2000/svg" width="${width}" height="${height}" viewBox="0 0 ${width} ${height}">`;

  // エッジ描画
  edges.forEach(edge => {
    const fromNode = nodes.find(n => n.id === edge.from)!;
    const toNode = nodes.find(n => n.id === edge.to)!;
    svg += generateEdge(fromNode, toNode, edge.label);
  });

  // ノード描画
  nodes.forEach(node => {
    svg += generateNode(node);
  });

  svg += '</svg>';
  return svg;
}

function generateNode(node: FlowchartNode): string {
  const nodeWidth = 120;
  const nodeHeight = 60;

  switch (node.type) {
    case 'start':
    case 'end':
      // 角丸矩形
      return `<rect x="${node.x - nodeWidth/2}" y="${node.y - nodeHeight/2}"
                   width="${nodeWidth}" height="${nodeHeight}" rx="30"
                   fill="${COLORS.accent.accent6}" />
              <text x="${node.x}" y="${node.y + 5}" text-anchor="middle"
                    fill="white" font-size="14">${node.label}</text>`;

    case 'process':
      // 矩形
      return `<rect x="${node.x - nodeWidth/2}" y="${node.y - nodeHeight/2}"
                   width="${nodeWidth}" height="${nodeHeight}" rx="5"
                   fill="${COLORS.accent.accent1}" />
              <text x="${node.x}" y="${node.y + 5}" text-anchor="middle"
                    fill="white" font-size="14">${node.label}</text>`;

    case 'decision':
      // ひし形
      return `<path d="M ${node.x} ${node.y - nodeHeight/2}
                       L ${node.x + nodeWidth/2} ${node.y}
                       L ${node.x} ${node.y + nodeHeight/2}
                       L ${node.x - nodeWidth/2} ${node.y} Z"
                    fill="${COLORS.accent.accent4}" />
              <text x="${node.x}" y="${node.y + 5}" text-anchor="middle"
                    fill="black" font-size="14">${node.label}</text>`;

    case 'data':
      // 平行四辺形
      return `<path d="M ${node.x - nodeWidth/2 + 15} ${node.y - nodeHeight/2}
                       L ${node.x + nodeWidth/2 + 15} ${node.y - nodeHeight/2}
                       L ${node.x + nodeWidth/2 - 15} ${node.y + nodeHeight/2}
                       L ${node.x - nodeWidth/2 - 15} ${node.y + nodeHeight/2} Z"
                    fill="${COLORS.accent.accent2}" />
              <text x="${node.x}" y="${node.y + 5}" text-anchor="middle"
                    fill="white" font-size="14">${node.label}</text>`;
  }
}

function generateEdge(from: FlowchartNode, to: FlowchartNode, label?: string): string {
  const arrowSize = 10;
  const angle = Math.atan2(to.y - from.y, to.x - from.x);
  const arrowX = to.x - Math.cos(angle) * 60;
  const arrowY = to.y - Math.sin(angle) * 30;

  let svg = `<line x1="${from.x}" y1="${from.y}" x2="${arrowX}" y2="${arrowY}"
                  stroke="${COLORS.secondary.dark}" stroke-width="2" />`;

  // 矢印
  svg += `<path d="M ${arrowX} ${arrowY}
                   L ${arrowX - arrowSize * Math.cos(angle - Math.PI/6)} ${arrowY - arrowSize * Math.sin(angle - Math.PI/6)}
                   L ${arrowX - arrowSize * Math.cos(angle + Math.PI/6)} ${arrowY - arrowSize * Math.sin(angle + Math.PI/6)} Z"
               fill="${COLORS.secondary.dark}" />`;

  // ラベル
  if (label) {
    const midX = (from.x + to.x) / 2;
    const midY = (from.y + to.y) / 2;
    svg += `<text x="${midX}" y="${midY - 5}" text-anchor="middle"
                  fill="${COLORS.secondary.dark}" font-size="12">${label}</text>`;
  }

  return svg;
}
```

### 使用例（YAML定義）

```yaml
nodes:
  - id: start
    type: start
    label: 開始
    x: 400
    y: 50

  - id: input
    type: data
    label: データ入力
    x: 400
    y: 150

  - id: validate
    type: decision
    label: 検証OK?
    x: 400
    y: 250

  - id: process
    type: process
    label: 処理実行
    x: 400
    y: 350

  - id: end
    type: end
    label: 終了
    x: 400
    y: 450

edges:
  - from: start
    to: input

  - from: input
    to: validate

  - from: validate
    to: process
    label: Yes

  - from: validate
    to: input
    label: No

  - from: process
    to: end
```

## グラフ生成

### 棒グラフ

```typescript
interface BarChartData {
  labels: string[];
  values: number[];
}

function generateBarChart(data: BarChartData, width = 600, height = 400): string {
  const margin = { top: 40, right: 40, bottom: 60, left: 60 };
  const chartWidth = width - margin.left - margin.right;
  const chartHeight = height - margin.top - margin.bottom;

  const maxValue = Math.max(...data.values);
  const barWidth = chartWidth / data.labels.length * 0.8;
  const gap = chartWidth / data.labels.length * 0.2;

  let svg = `<svg xmlns="http://www.w3.org/2000/svg" width="${width}" height="${height}" viewBox="0 0 ${width} ${height}">`;

  // 軸
  svg += `<line x1="${margin.left}" y1="${margin.top}" x2="${margin.left}" y2="${height - margin.bottom}"
               stroke="${COLORS.secondary.dark}" stroke-width="2" />`;
  svg += `<line x1="${margin.left}" y1="${height - margin.bottom}" x2="${width - margin.right}" y2="${height - margin.bottom}"
               stroke="${COLORS.secondary.dark}" stroke-width="2" />`;

  // バー
  data.values.forEach((value, index) => {
    const barHeight = (value / maxValue) * chartHeight;
    const x = margin.left + index * (barWidth + gap) + gap/2;
    const y = height - margin.bottom - barHeight;

    svg += `<rect x="${x}" y="${y}" width="${barWidth}" height="${barHeight}"
                 fill="${COLORS.accent.accent1}" />`;

    // ラベル
    svg += `<text x="${x + barWidth/2}" y="${height - margin.bottom + 20}"
                 text-anchor="middle" font-size="12">${data.labels[index]}</text>`;

    // 値
    svg += `<text x="${x + barWidth/2}" y="${y - 5}"
                 text-anchor="middle" font-size="12">${value}</text>`;
  });

  svg += '</svg>';
  return svg;
}
```

### 折れ線グラフ

```typescript
interface LineChartData {
  labels: string[];
  values: number[];
}

function generateLineChart(data: LineChartData, width = 600, height = 400): string {
  const margin = { top: 40, right: 40, bottom: 60, left: 60 };
  const chartWidth = width - margin.left - margin.right;
  const chartHeight = height - margin.top - margin.bottom;

  const maxValue = Math.max(...data.values);
  const minValue = Math.min(...data.values);
  const xStep = chartWidth / (data.values.length - 1);

  let svg = `<svg xmlns="http://www.w3.org/2000/svg" width="${width}" height="${height}" viewBox="0 0 ${width} ${height}">`;

  // 軸
  svg += `<line x1="${margin.left}" y1="${margin.top}" x2="${margin.left}" y2="${height - margin.bottom}"
               stroke="${COLORS.secondary.dark}" stroke-width="2" />`;
  svg += `<line x1="${margin.left}" y1="${height - margin.bottom}" x2="${width - margin.right}" y2="${height - margin.bottom}"
               stroke="${COLORS.secondary.dark}" stroke-width="2" />`;

  // 折れ線
  let points = '';
  data.values.forEach((value, index) => {
    const x = margin.left + index * xStep;
    const y = height - margin.bottom - ((value - minValue) / (maxValue - minValue)) * chartHeight;
    points += `${x},${y} `;
  });

  svg += `<polyline points="${points}" fill="none" stroke="${COLORS.accent.accent1}" stroke-width="3" />`;

  // ポイント
  data.values.forEach((value, index) => {
    const x = margin.left + index * xStep;
    const y = height - margin.bottom - ((value - minValue) / (maxValue - minValue)) * chartHeight;
    svg += `<circle cx="${x}" cy="${y}" r="4" fill="${COLORS.accent.accent1}" />`;

    // ラベル
    if (index % Math.ceil(data.labels.length / 6) === 0) {
      svg += `<text x="${x}" y="${height - margin.bottom + 20}"
                   text-anchor="middle" font-size="12">${data.labels[index]}</text>`;
    }
  });

  svg += '</svg>';
  return svg;
}
```

## Marp埋め込み

### Base64エンコード方式

```markdown
# スライド

![icon](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI2NCIgaGVpZ2h0PSI2NCI+...)
```

### インラインSVG方式

```markdown
# スライド

<div style="text-align: center;">

<svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 64 64">
  <circle cx="32" cy="32" r="30" fill="#70AD47" />
  <path d="M 20 32 L 28 40 L 44 24" stroke="white" stroke-width="4" fill="none" />
</svg>

</div>
```

## ヘルパー関数

```typescript
// SVGをBase64エンコード
function svgToBase64(svg: string): string {
  return Buffer.from(svg).toString('base64');
}

// Markdown画像タグ生成
function svgToMarkdownImage(svg: string, alt = 'icon'): string {
  const base64 = svgToBase64(svg);
  return `![${alt}](data:image/svg+xml;base64,${base64})`;
}

// ファイルに保存
function saveSvg(svg: string, filename: string): void {
  fs.writeFileSync(filename, svg);
}
```

## バージョン履歴

- **v1.0.0** (2026-03-19)
  - 初版リリース
  - 20種類のアイコン実装
  - フローチャート生成機能実装
  - グラフ生成機能実装（棒・折れ線）
