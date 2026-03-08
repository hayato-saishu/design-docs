# ER図

## テーブル設計

```mermaid
erDiagram
    USERS {
        uuid id PK
        string email UK "メールアドレス"
        string password_hash "パスワードハッシュ"
        string name "表示名"
        string role "ロール: admin / user"
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

    POSTS {
        uuid id PK
        uuid user_id FK
        string title "タイトル"
        text content "本文"
        string status "draft / published / archived"
        timestamp published_at
        timestamp created_at
        timestamp updated_at
    }

    TAGS {
        uuid id PK
        string name UK "タグ名"
        timestamp created_at
    }

    POST_TAGS {
        uuid post_id FK
        uuid tag_id FK
    }

    USERS ||--o{ SESSIONS : "持つ"
    USERS ||--o{ POSTS : "投稿する"
    POSTS ||--o{ POST_TAGS : "持つ"
    TAGS ||--o{ POST_TAGS : "付けられる"
```

## テーブル説明

| テーブル名 | 説明 |
|---|---|
| `users` | ユーザー情報 |
| `sessions` | 認証セッション（リフレッシュトークン管理） |
| `posts` | 投稿コンテンツ |
| `tags` | タグマスタ |
| `post_tags` | 投稿とタグの中間テーブル |

## インデックス設計

| テーブル | カラム | 種別 | 用途 |
|---|---|---|---|
| `users` | `email` | UNIQUE | ログイン検索 |
| `sessions` | `refresh_token` | UNIQUE | トークン検証 |
| `sessions` | `user_id` | INDEX | ユーザー別セッション取得 |
| `posts` | `user_id` | INDEX | ユーザー別投稿取得 |
| `posts` | `status, published_at` | INDEX | 公開記事一覧取得 |
