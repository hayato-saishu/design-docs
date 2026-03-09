# ER図

## テーブル設計

```mermaid
erDiagram
    USERS {
        uuid id PK
        string email UK "メールアドレス"
        string password_hash "パスワードハッシュ"
        string name "表示名"
        boolean is_active "有効フラグ"
        timestamp created_at
        timestamp updated_at
    }

    SESSIONS {
        uuid id PK
        uuid user_id FK
        string refresh_token UK "リフレッシュトークン"
        timestamp expires_at "有効期限"
        timestamp created_at
    }

    CATEGORIES {
        uuid id PK
        uuid user_id FK "NULL=システム共通"
        string name "カテゴリ名"
        string type "income / expense"
        string color "表示色（HEX）"
        timestamp created_at
    }

    TRANSACTIONS {
        uuid id PK
        uuid user_id FK
        uuid category_id FK
        string type "income / expense"
        integer amount "金額（円）"
        string memo "メモ"
        date date "明細日付"
        uuid recurring_income_id FK "NULL=手動登録"
        timestamp created_at
        timestamp updated_at
    }

    RECURRING_INCOMES {
        uuid id PK
        uuid user_id FK
        uuid category_id FK
        integer amount "金額（円）"
        string memo "メモ"
        integer day_of_month "毎月の反映日（1〜28）"
        boolean is_active "有効フラグ"
        timestamp created_at
        timestamp updated_at
    }

    USERS ||--o{ SESSIONS : "持つ"
    USERS ||--o{ CATEGORIES : "作成する"
    USERS ||--o{ TRANSACTIONS : "登録する"
    USERS ||--o{ RECURRING_INCOMES : "設定する"
    CATEGORIES ||--o{ TRANSACTIONS : "分類される"
    CATEGORIES ||--o{ RECURRING_INCOMES : "分類される"
    RECURRING_INCOMES ||--o{ TRANSACTIONS : "自動生成する"
```

## テーブル説明

| テーブル名 | 説明 |
|---|---|
| `users` | ユーザー情報 |
| `sessions` | 認証セッション（リフレッシュトークン管理） |
| `categories` | カテゴリマスタ（収入・支出。user_id=NULLはシステム共通） |
| `transactions` | 収支明細（手動登録・定期収入自動生成の両方） |
| `recurring_incomes` | 毎月の定期収入設定 |

## インデックス設計

| テーブル | カラム | 種別 | 用途 |
|---|---|---|---|
| `users` | `email` | UNIQUE | ログイン検索 |
| `sessions` | `refresh_token` | UNIQUE | トークン検証 |
| `sessions` | `user_id` | INDEX | ユーザー別セッション取得 |
| `transactions` | `user_id, date` | INDEX | 月別明細取得 |
| `transactions` | `user_id, category_id` | INDEX | カテゴリ別明細取得 |
| `transactions` | `recurring_income_id` | INDEX | 定期収入の重複チェック |
| `recurring_incomes` | `user_id, is_active` | INDEX | バッチ処理での対象取得 |
| `recurring_incomes` | `day_of_month` | INDEX | 反映日でのバッチ絞り込み |