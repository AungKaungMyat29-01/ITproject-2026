# TODOアプリ V1 実装手順書

本手順書は `docs/todo/01_requirements.md` および `docs/todo/02_design.md` を根拠とし、既存の `index.ts`・`package.json`・`tsconfig.json` の構成を壊さずに最小単位で実装・検証を進めるための手順を定義する。

---

## 0. 事前確認：各機能の完了定義（Done条件）

- **REQ-01（一覧）**: データ0件なら `GET /api/v1/todos` の `data` が空配列 `[]` になり、画面に「TODOがありません」と表示され、1件以上ある場合は `id` 昇順で表示されること。
- **REQ-02（登録）**: `POST /api/v1/todos` に送った `title` の前後空白を除いて1〜100文字でDB保存され、`201` で採番済みTODOが返り、空白のみ・101文字以上は `422` となること。
- **REQ-03（部分更新）**: `PATCH /api/v1/todos/:id` で指定したIDの `isCompleted` だけが更新されて `200` が返り、リロード後も維持され、存在しないIDは `404` となること。
- **REQ-04（削除）**: `DELETE /api/v1/todos/:id` で指定したIDのデータがDBから削除されて `204`（本文なし）が返り、画面から消え、存在しないIDは `404` となること。

---

## 工程1：基盤整備（DBドライバ導入・`db.ts`・`tsconfig.json`・`/api/db-check`）

※現在 `db.ts` と `/api/db-check` が未作成のため、TODO実装前に既存構成へ最小限のDB接続基盤を追加する。

- **対象ファイル**: `package.json`, `tsconfig.json`, `db.ts`（新規）, `index.ts`
- **作業内容**:
  1. `pg` および `@types/pg`（または第2回講義指定のDBドライバ）をインストールする。
  2. `tsconfig.json` の `include` に `"db.ts"`, `"src/**/*"` を追加する。
  3. プロジェクト直下に `db.ts` を作成し、環境変数 `DATABASE_URL` でNeon DBへ接続する処理を記述する。
  4. `index.ts` に `app.use(express.json());` と `GET /api/db-check` を追加する（既存の `GET /` は変更しない）。
- **確認コマンド**:
  ```bash
  npm install pg && npm install -D @types/pg
  npm run build
  npm run dev
  curl -i http://localhost:3000/api/db-check