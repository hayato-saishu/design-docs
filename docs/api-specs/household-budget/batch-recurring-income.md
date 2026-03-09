# 定期収入自動反映バッチ シーケンス図

## 概要

| 項目 | 内容 |
|---|---|
| バッチ名 | 定期収入自動反映バッチ |
| 実行タイミング | 毎日 00:05 |
| 処理内容 | `recurring_incomes` から当日が `day_of_month` の有効な設定を取得し、当月分の収入明細を自動登録 |
| 重複防止 | `transactions.recurring_income_id` と当月の組み合わせで既存チェック |

---

## 処理フロー

```mermaid
sequenceDiagram
    participant Scheduler as スケジューラー<br/>（毎日 00:05）
    participant BE as バックエンド
    participant DB as データベース

    Scheduler->>BE: バッチ起動

    BE->>DB: SELECT * FROM recurring_incomes<br/>WHERE day_of_month = DAY(TODAY())<br/>AND is_active = true
    DB-->>BE: 対象の定期収入設定一覧

    loop 各定期収入設定
        BE->>DB: SELECT COUNT(*) FROM transactions<br/>WHERE recurring_income_id = ?<br/>AND YEAR(date) = YEAR(TODAY())<br/>AND MONTH(date) = MONTH(TODAY())
        DB-->>BE: 当月登録済み件数

        alt 未登録（COUNT = 0）
            BE->>DB: INSERT INTO transactions<br/>(user_id, type, amount, category_id, date,<br/>memo, recurring_income_id)<br/>VALUES (?, 'income', ?, ?, TODAY(), ?, ?)
            DB-->>BE: 登録完了
            BE->>BE: 成功ログ出力<br/>[INFO] recurring_income_id={id} を登録しました
        else 登録済み（重複スキップ）
            BE->>BE: スキップログ出力<br/>[INFO] recurring_income_id={id} は当月登録済みのためスキップ
        end
    end

    BE->>BE: 処理完了ログ出力<br/>[INFO] バッチ終了 登録件数={n} スキップ件数={m}
```
