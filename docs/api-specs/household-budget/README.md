# 家計簿アプリ API仕様 シーケンス図

## API一覧

| ファイル | エンドポイント | 説明 |
|---|---|---|
| [auth.md](./auth.md) | `POST /api/auth/login` | ログイン |
| [auth.md](./auth.md) | `POST /api/auth/refresh` | トークン更新 |
| [auth.md](./auth.md) | `POST /api/auth/logout` | ログアウト |
| [users.md](./users.md) | `POST /api/users` | ユーザー登録 |
| [transactions.md](./transactions.md) | `POST /api/transactions` | 明細登録（収入・支出） |
| [transactions.md](./transactions.md) | `GET /api/transactions` | 明細一覧取得 |
| [recurring-incomes.md](./recurring-incomes.md) | `POST /api/recurring-incomes` | 定期収入設定登録 |
| [recurring-incomes.md](./recurring-incomes.md) | `PUT /api/recurring-incomes/:id` | 定期収入設定更新・無効化 |

## バッチ一覧

| ファイル | バッチ名 | 実行タイミング | 説明 |
|---|---|---|---|
| [batch-recurring-income.md](./batch-recurring-income.md) | 定期収入自動反映バッチ | 毎日 00:05 | 当日が反映日の定期収入設定から収入明細を自動生成 |

---

> 新しいAPIやバッチを追加する場合は、このREADMEのテーブルにも追記してください。
