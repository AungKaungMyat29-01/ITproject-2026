# TODOアプリ V1 設計書

## 1. 作るものとファイルの役割

### 1.1 ファイル構成（授業テキストに合わせた配置）
既存のルート直下構成（`index.ts`, `package.json`, `tsconfig.json`）を維持し、TODO機能用のファイルを機能別フォルダへ配置する。

```text
プロジェクト直下/
├── index.ts                      # 既存エントリ（express.json追加・todoRouter登録・既存/と/api/db-check維持）
├── db.ts                         # DB接続モジュール（※現在未作成のため要確認・新規作成予定）
├── package.json                  # 依存関係（DBドライバ追加は要確認）
├── tsconfig.json                 # TSコンパイル設定（include対象にdb.ts・src/**/*の追加が必要）
├── public/
│   └── todo/
│       └── todo.js               # ブラウザ側Fetch通信（TodoApiクラス）とDOM操作
├── views/
│   └── todo/
│       └── index.ejs             # TODO画面テンプレート（/todo）
├── src/
│   └── apps/
│       └── todo/
│           ├── todoRouter.ts     # /api/v1/todos のルーティング定義
│           ├── todoController.ts # リクエスト検証・HTTPステータスとJSON応答制御
│           └── todoModel.ts      # SQL実行・DBレコードとAPI JSONの形式変換
├── docs/
│   └── todo/
│       ├── 01_requirements.md    # 要件定義書
│       ├── 02_design.md          # 本設計書
│       ├── 03_implementation_plan.md # 実装手順書
│       └── 04_test_results.md    # テスト結果書
└── sql/
    ├── README.md                 # SQL実行履歴記録
    └── todo/
        └── 020_create_todo_tasks.sql # todo_tasksテーブル作成DDL