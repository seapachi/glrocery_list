# 🛒 買い物リスト

スーパーの売り場順で買い物メモを整理できる Web アプリです。
貼り付けたテキストをカテゴリごとに並べ替え、チェックしながら効率よく買い回りできます。

## 主な機能

- 入力テキストの自動カテゴリ分類（`、` / `,` / 改行区切り対応）
- 未分類アイテムのキーワード登録（次回以降の自動分類に反映）
- カスタムカテゴリの追加・削除
- 売り場順（カテゴリ順）の並び替え
- ユーザー設定のエクスポート / インポート（JSON）
- インポート競合時の一括解決UI（現在値 or 取込値）

## 起動方法

このアプリは `default-categories.json` を `fetch` で読み込むため、`file://` 直開きではなくローカルサーバーで起動してください。

```bash
git clone https://github.com/seapachi/glrocery_list.git
cd glrocery_list
python -m http.server 8000
```

ブラウザで `http://127.0.0.1:8000/` を開きます。

## デフォルトカテゴリの拡張

デフォルトカテゴリは `default-categories.json` に定義されています。
コード編集なしで、このJSONを編集するだけで拡張できます。

### スキーマ（version必須）

```json
{
  "version": 1,
  "otherCategoryName": "📦 その他",
  "categories": [
    {
      "name": "🥩 肉・魚介",
      "keywords": ["豚肉", "鶏"]
    }
  ]
}
```

### ルール

- `version` は `1`
- `otherCategoryName` は必須・空文字不可
- `categories` は1件以上
- `categories[].name` は重複不可
- `categories[].keywords` は文字列配列（空文字不可）
- `categories[].name` と `otherCategoryName` の重複は禁止

不正なJSONの場合は起動エラー画面が表示され、アプリは開始しません。

## ユーザー設定のエクスポート/インポート

設定画面（⚙️）から操作できます。

- エクスポート対象:
  - `customKeywords`
  - `customCategories`
  - `categoryOrder`
- インポート方式: マージ
- 競合時: 一覧で「現在の値」か「インポート値」を選択して一括適用
- 不正JSON: 全体中止（部分反映なし）

### インポート/エクスポートJSON形式

```json
{
  "version": 1,
  "exportedAt": "2026-03-02T12:34:56.000Z",
  "data": {
    "customKeywords": { "R-1": "🥛 乳製品・卵" },
    "customCategories": [{ "name": "🥐 パン", "keywords": [] }],
    "categoryOrder": ["🥩 肉・魚介", "🥐 パン", "📦 その他"]
  }
}
```

## localStorageキー

| Key | 内容 |
| --- | --- |
| `grocery-custom-keywords` | キーワード -> カテゴリの対応 |
| `grocery-custom-categories` | ユーザー作成カテゴリ一覧 |
| `grocery-category-order` | 売り場順（表示順） |

## ファイル構成

```text
.
├─ AGENTS.md
├─ PLAN.md
├─ README.md
├─ EXPLANATION.md
├─ default-categories.json
└─ index.html
```
