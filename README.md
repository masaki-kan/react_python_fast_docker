# React + FastAPI + Prisma (Docker)

個人開発用のフルスタックプロジェクト

## 技術スタック

| レイヤー | 技術 |
|---------|------|
| Frontend | React + TypeScript (Vite) |
| Backend | Python3 + FastAPI |
| ORM | Prisma (prisma-client-py) |
| DB | PostgreSQL 16 |
| 環境 | Docker Compose |

## 前提条件

- Docker / Docker Compose がインストール済みであること

## セットアップ & 起動

### 1. コンテナをビルド & 起動

```bash
docker compose up --build
```

初回はイメージのビルドに時間がかかります。
全サービスが立ち上がると、以下にアクセスできます：

| サービス | URL |
|---------|-----|
| Frontend | http://localhost:3000 |
| Backend | http://localhost:8001 |
| API Docs (Swagger) | http://localhost:8001/docs |

### 2. DB マイグレーション

コンテナが起動した状態で、別ターミナルから実行：

```bash
# スキーマを DB に反映（開発用）
docker compose exec backend prisma db push
```

本番向けにマイグレーションファイルを管理したい場合：

```bash
# マイグレーションファイルを作成 & 適用
docker compose exec backend prisma migrate dev --name init
```

### 3. Prisma Studio（DB の GUI）

コンテナが起動した状態で、別ターミナルから実行：

```bash
docker compose exec backend prisma studio --port 5555 --hostname 0.0.0.0
```

http://localhost:5555 でテーブルの中身を確認・編集できます。

### 4. 動作確認

```bash
# ヘルスチェック
curl http://localhost:8001/health
# => {"status":"ok"}
```

## よく使うコマンド

```bash
# バックグラウンドで起動
docker compose up -d --build

# ログを確認
docker compose logs -f

# 特定サービスのログ
docker compose logs -f backend

# コンテナを停止
docker compose down

# コンテナ + DB データを削除して完全リセット
docker compose down -v

# Prisma Studio（DB の GUI）を起動
docker compose exec backend prisma studio --port 5555 --hostname 0.0.0.0
```

## ディレクトリ構成

```
.
├── docker-compose.yml
├── frontend/
│   ├── Dockerfile
│   ├── src/
│   │   ├── App.tsx
│   │   └── main.tsx
│   └── package.json
└── backend/
    ├── Dockerfile
    ├── requirements.txt
    ├── .env
    ├── app/
    │   └── main.py
    └── prisma/
        └── schema.prisma
```
# react_python_fast_docker
