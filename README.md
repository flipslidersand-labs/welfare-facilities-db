# Welfare Facilities DB

> **Status**: Active (moving to production) — 2026-08-01
> A roadmap addressing 4 critical items (data import, API authentication, credentials, deployment) has been established for the production rollout.
>
> **ステータス**: 本番化進行中 — 2026-08-01 判定
> Critical 4件（データインポート・API認証・認証情報・デプロイ先）のロードマップを策定し本番化へ移行。

A system that visualizes — on a per-year basis — revenue, financial condition, and operational details of social welfare corporations and care service providers in Japan.
Ingests CSV data from government public datasets (WAM NET, Care Service Information Publication, Disability Welfare Information Publication).

社会福祉法人・介護事業者を対象に、法人単位の売上・財務状況と施設単位の規模・定員などを年ごとに可視化するシステム。政府系公開データ（WAM NET、介護サービス情報公表、障害福祉情報公表）から CSV を取り込みます。

## Overview / 概要

- **Corporation Master / 法人マスタ**: Annual basic information for social welfare corporations / 社会福祉法人の基本情報を年次で管理
- **Facility Master / 事業所マスタ**: Care/disability/child welfare facilities (capacity, service type) / 介護・障害・児童福祉施設の情報
- **Corporate Financials / 法人財務年次**: Revenue, ordinary profit, net assets by fiscal year / 売上・経常利益・純資産などを年度別に記録
- **Dashboard / ダッシュボード**: Looker Studio visualization / Looker Studio での可視化対応

## Quick Start / クイックスタート

```bash
bash setup.sh   # recommended / 推奨
```

Or with Docker Compose / Docker Compose で起動:

```bash
docker-compose up
# Frontend: http://localhost:5173
# Backend API: http://localhost:8000
# API docs: http://localhost:8000/docs
```

### Local Development / ローカル開発

```bash
# Backend
cd backend && python -m venv venv && source venv/bin/activate
pip install -r requirements.txt && cp .env.example .env
python scripts/init_db.py && uvicorn app.main:app --reload

# Frontend
cd frontend && npm install && npm run dev
```

## Technology Stack / 技術スタック

| Layer | Tech |
|---|---|
| Backend | FastAPI 0.111.0, SQLAlchemy 2.0, PostgreSQL 16 |
| Frontend | React 18.2, TypeScript 5.3, Vite 5, Recharts 2.10 |
| Container | Docker + Docker Compose |

## API Endpoints / API エンドポイント

### Corporations / 法人
- `GET /api/corporations` — List with filter & pagination / 一覧（フィルタ・ページング）
- `GET /api/corporations/{id}` — Detail with financials / 詳細（財務・事業所込み）
- `GET /api/corporations/{id}/financials` — Financial history / 財務推移
- `GET /api/corporations/{id}/facilities` — Child facilities / 配下事業所

### Facilities / 施設
- `GET /api/facilities` — List with prefecture/type filter / 一覧（都道府県・サービス種別フィルタ）
- `GET /api/facilities/{id}` — Detail / 詳細

### Analytics / 分析
- `GET /api/analytics/ranking?fiscal_year=2022` — Revenue ranking / 売上ランキング
- `GET /api/analytics/regional?fiscal_year=2022` — Regional breakdown / 地域別集計

## Data Import / データ取込み

```bash
# Step 1: Corporate financial data / 法人財務データ
python scripts/import_corporation_csv.py --master corporations.csv
python scripts/import_corporation_csv.py --financials financials.csv

# Step 2: Facility data / 施設データ
python scripts/import_facility_csv.py facilities.csv 介護

# Step 3: Corporation-facility matching / マッチング
python scripts/match_corporation_facility.py 0.8
```

## Development Roadmap / 開発ロードマップ

### Phase 1 — Complete / 実装済み
- ✅ DB schema (3 tables) / DB スキーマ
- ✅ CSV import scripts / CSV インポートスクリプト
- ✅ FastAPI routers / FastAPI ルーター
- ✅ React frontend (Dashboard, List, Detail) / React フロントエンド

### Phase 2 — Next / 次
- ⏳ Looker Studio dashboard / Looker Studio ダッシュボード
- ⏳ Multi-year auto-update batch / 複数年度データ自動更新バッチ

### Phase 3 — Production / 本運用
- ⏳ Scheduled updates / 月次・年次スケジュール更新
- ⏳ Read-only permissions / 権限管理

## License

MIT
