# 花粉症・限界突破エクスキュース（言い訳）ジェネレーター

花粉症を理由に言い訳を生成するジョークジェネレーターです。症状や状況を入力すると、AI がユニークな言い訳を生成します。
生成した言い訳は X（旧 Twitter）でシェアしたり、追加の指示を与えてさらに盛ることもできます。

入力項目は **症状（必須）**、相手、状況、花粉症レベル（1〜5）、ニュアンス（文体）。  
症状以外は任意で、未入力の場合は AI がランダムに選んで生成します。現在地の花粉データを利用することもできます。

**デモ: https://excuse-by-pollen.vercel.app/**
> バックエンドは現在停止中のため、AI による言い訳生成は動作しない場合があります。ローカルでの実行は[こちら](#ローカルでの実行docker-推奨)を参照してください。

## スクリーンショット

<!-- スクリーンショットまたは GIF をここに追加 -->

## 技術スタック

![技術スタック図](https://github.com/user-attachments/assets/f3cd9613-7981-43f5-b72d-d919fe1c611a)

### フロントエンド

- Next.js / React / TypeScript
- Tailwind CSS
- Font Awesome
- @vercel/og（OGP 画像生成）
- Node.js / Bun
- Vercel（デプロイ先）

### バックエンド

- Python / FastAPI
- Google Pollen API（花粉データ取得）
- Gemini 3 Flash（言い訳生成 AI）
- Upstash Redis（シェア用データ保存）
- Render.com（デプロイ先）

### データベース

- Redis / Upstash

シェア機能でID紐付きのデータを保存するため、高速な NoSQL である Redis を採用しています。

## 機能

| 機能 | 説明 |
|---|---|
| 言い訳を生成する | 症状・相手・状況・レベル・ニュアンスをもとに AI が言い訳を生成 |
| もっと盛る | 生成済みの言い訳に追加指示を与えて再生成（例：「もっと症状を盛って」） |
| 説得力スコア | 生成した言い訳の説得力を AI が数値化して表示 |
| 読み上げ機能 | ブラウザの SpeechSynthesis API で言い訳を音声再生 |
| バッジ表示 | 現在地の都市名・花粉指数・花粉の種類をバッジで表示 |
| X シェア | OGP 画像付きで生成した言い訳を X に投稿 |
| 位置情報の利用 | 緯度・経度をバックエンドに送信し、市区町村名と花粉データを取得 |

## ローカルでの実行（Docker 推奨）

### 1. リポジトリをクローン

```bash
git clone --recursive https://github.com/hduehgw0/excuse-by-pollen.git
cd excuse-by-pollen
```

### 2. バックエンドの環境変数を設定

```bash
cp back/.env.sample back/.env
```

`back/.env` を開き、以下のキーを実際の値に書き換えてください。

| 変数名 | 内容 | 取得先 |
|---|---|---|
| `GEMINI_API_KEY` | Gemini API キー | Google AI Studio |
| `POLLEN_API_KEY` | Google Pollen API キー | Google Cloud Console |
| `UPSTASH_REDIS_REST_URL` | Upstash Redis の REST URL | Upstash コンソール |
| `UPSTASH_REDIS_REST_TOKEN` | Upstash Redis のトークン | Upstash コンソール |

### 3. Docker で起動

```bash
# ビルド＆起動（初回）
docker compose up --build

# 2回目以降
docker compose up

# バックグラウンドで起動
docker compose up -d

# 終了
docker compose down
```

起動後、[http://localhost:3000](http://localhost:3000) でアクセスできます。

> フロントエンド・バックエンドそれぞれを個別に起動する場合は、各サブモジュールの README を参照してください。
> - [excuse-by-pollen-front](https://github.com/hduehgw0/excuse-by-pollen-front)
> - [excuse-by-pollen-back](https://github.com/hduehgw0/excuse-by-pollen-back)
