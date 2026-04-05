# Sony Electronics 価格トラッカー

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Sony Electronics Price Tracker](https://img.shields.io/badge/Sony%20Electronics%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/sony-electronics)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/sony-electronics)

Sony Electronics の価格をリアルタイムで追跡 - Sony の消費者直販向けエレクトロニクス小売部門。開始方法は 2 つあります: **フルマネージド**のインテリジェンスプラットフォーム、または独自のパイプラインを構築するための**セルフサービス API**です。

---

## オプション 1: Bright Insights - AI 搭載価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/sony-electronics)** は、Bright Data のフルマネージドな小売インテリジェンスプラットフォームです。scraper を構築する必要も、インフラを保守する必要もありません。構造化され、分析可能な価格データが、ダッシュボード、データフィード、または BI ツールにそのまま配信されます。

**チームが Bright Insights を選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードとデータフィードで数分で運用開始
- 🤖 **AI 搭載の推奨機能** - 対話型 AI アシスタントが数百万のデータポイントを即座に実用的なインサイトへ変換
- ⚡ **リアルタイム監視** - 1 時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆる Web サイト、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイ統合** - AWS、GCP、Databricks、Snowflake など
- 🛡️ **フルマネージド** - Bright Data がスキーマ変更、サイト更新、データ品質を自動で処理

**主なユースケース:**
- ✅ Sony Electronics の**エレクトロニクス製品の値下げ**やセールイベントを監視
- ✅ 入手困難な商品の**在庫状況を追跡**
- ✅ **価格履歴チャートを作成**して最適な購入タイミングを把握
- ✅ MAP ポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで正規化されたデータを動的価格設定アルゴリズムや AI モデルに直接投入

> **月額 $250 から - [最適な見積もりを取得 →](https://brightdata.jp/products/insights/price-tracker/sony-electronics)**

---

## オプション 2: Web Scraper API によるセルフサービス

独自のパイプラインを構築したいですか？ Bright Data の **Web Scraper API** を使えば、proxy や scraping インフラを管理することなく、Sony Electronics の商品データ（価格、在庫状況、レビューなど）にプログラムからアクセスできます。

### 前提条件

- Python 3.9 以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Data の **API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Sony Electronics 商品用の Bright Data **Web Scraper ID**（[Web Scrapers control panel で確認](https://brightdata.jp/cp/scrapers)）

### セットアップ

1. **この repository を clone**

   ```bash
   git clone https://github.com/bright-jp/sony-electronics-price-tracker.git
   cd sony-electronics-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Web Scraper ID の確認方法**
   > [Bright Data Control Panel](https://brightdata.jp/cp/scrapers) にログインし、**Web Scrapers** に移動して、
   > 「Sony Electronics」を検索し、Web Scraper ID をコピーします（形式: `gd_xxxxxxxxxxxx`）。

---

## 使用方法

### 1. URL で特定の商品を追跡

Sony Electronics の商品 URL のリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.sony.com/product/sample-item-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリ URL で商品を閲覧

Sony Electronics のカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://sony.com/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには次のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 商品ページ URL |
| `title` | 商品名 |
| `brand` | ブランド / メーカー |
| `model_number` | モデル番号 |
| `initial_price` | 元の価格 |
| `final_price` | 現在の価格 |
| `currency` | 通貨コード |
| `discount` | 割引 |
| `in_stock` | 在庫状況 |
| `rating` | 平均評価 |
| `reviews_count` | レビュー数 |
| `sku` | SKU / 部品番号 |
| `description` | 商品説明 |
| `images` | 商品画像 |
| `timestamp` | 収集タイムスタンプ |

### サンプル出力

```json
[
  {
    "url": "https://www.sony.com/product/sample-item-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://sony.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返されるレコードの最大数 |
| `include_errors` | boolean | `true` | 結果にエラーレポートを含める |
| `notify` | string (URL) | - | snapshot の準備完了時に呼び出される webhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または `ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.sony.com/product/sample-item-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [Sony Electronics Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/sony-electronics)
- 🔧 [Sony Electronics Scraper API](https://brightdata.jp/products/web-scraper/sony-electronics)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp) で構築 - 業界をリードする Web データプラットフォーム。*