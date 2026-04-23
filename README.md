# MySQL on Docker

## ファイル構成

```
MySQL/
├── docker-compose.yml       # コンテナ定義
├── .env                     # 環境変数（Git除外）
├── .env.example             # .envのテンプレート（Git管理）
├── .gitignore
├── conf/
│   └── my.cnf               # MySQLカスタム設定
└── init/
    └── 01_schema.sql        # 初回起動時に自動実行されるSQL
```

## 主な設計ポイント

| 項目 | 内容 |
|------|------|
| **データ永続化** | Named Volume (`mysql_data`) でコンテナ削除後もデータを保持 |
| **初期化SQL** | `init/` 以下のSQLが初回起動時のみ自動実行（ファイル名順） |
| **ヘルスチェック** | `mysqladmin ping` で起動完了を検知 |
| **文字コード** | `utf8mb4` / `utf8mb4_unicode_ci` |
| **タイムゾーン** | `Asia/Tokyo` (JST) |
| **スロークエリログ** | 1秒以上のクエリを記録 |
| **バイナリログ** | ポイントインタイムリカバリ対応、7日保持 |

## セットアップ

`.env.example` をコピーして `.env` を作成し、各値を設定する。

```bash
cp .env.example .env
```

## 起動方法

```bash
# 起動
docker compose up -d

# 接続確認
docker compose exec mysql mysql -u myuser -pmypassword mydb

# 停止（データは保持）
docker compose down

# データごと削除
docker compose down -v
```

> `.env` のパスワードは本番環境では必ず変更してください。
