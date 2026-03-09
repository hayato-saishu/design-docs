# 明細 API シーケンス図

## 1. 明細登録

**`POST /api/transactions`**

```mermaid
sequenceDiagram
    participant FE as フロントエンド
    participant BE as バックエンド
    participant DB as データベース

    FE->>BE: POST /api/transactions
    note over FE,BE: Request Header<br/>Authorization: Bearer {accessToken}<br/>Request Body<br/>{ type: "income"|"expense", amount: number,<br/>  categoryId: string, date: string, memo: string }

    BE->>BE: JWTトークン検証・userId取得

    alt トークン有効
        BE->>DB: INSERT INTO transactions<br/>(user_id, type, amount, category_id, date, memo)
        DB-->>BE: 保存完了（生成されたtransactionId）
        BE-->>FE: 201 Created
        note over BE,FE: Response Body<br/>{ id: string, type: string, amount: number,<br/>  categoryId: string, date: string, memo: string, createdAt: string }
    else トークン無効
        BE-->>FE: 401 Unauthorized
        note over BE,FE: Response Body<br/>{ error: "認証が必要です" }
    end
```

---

## 2. 明細一覧取得（月ごと）

**`GET /api/transactions?year=YYYY&month=MM`**

```mermaid
sequenceDiagram
    participant FE as フロントエンド
    participant BE as バックエンド
    participant DB as データベース

    FE->>BE: GET /api/transactions?year=YYYY&month=MM
    note over FE,BE: Request Header<br/>Authorization: Bearer {accessToken}<br/>Query Params<br/>year: number（例: 2026）, month: number（例: 3）

    BE->>BE: JWTトークン検証・userId取得
    BE->>DB: SELECT * FROM transactions<br/>WHERE user_id = ? AND YEAR(date) = ? AND MONTH(date) = ?<br/>ORDER BY date DESC
    DB-->>BE: 明細一覧（収入・支出）

    BE->>BE: totalIncome / totalExpense / balance を集計

    BE-->>FE: 200 OK
    note over BE,FE: Response Body<br/>{ transactions: [...],<br/>  summary: { totalIncome: number, totalExpense: number, balance: number } }
```

---

## 3. 明細一覧取得（カテゴリごと）

**`GET /api/transactions?year=YYYY&month=MM&category_id=xxx`**

```mermaid
sequenceDiagram
    participant FE as フロントエンド
    participant BE as バックエンド
    participant DB as データベース

    FE->>BE: GET /api/transactions?year=YYYY&month=MM&category_id=xxx
    note over FE,BE: Request Header<br/>Authorization: Bearer {accessToken}<br/>Query Params<br/>year: number, month: number, category_id: string

    BE->>BE: JWTトークン検証・userId取得
    BE->>DB: SELECT * FROM transactions<br/>WHERE user_id = ? AND YEAR(date) = ? AND MONTH(date) = ?<br/>AND category_id = ?<br/>ORDER BY date DESC
    DB-->>BE: 明細一覧（指定カテゴリ）

    BE->>BE: カテゴリ別合計を集計

    BE-->>FE: 200 OK
    note over BE,FE: Response Body<br/>{ transactions: [...],<br/>  summary: { categoryId: string, categoryName: string, total: number } }
```
