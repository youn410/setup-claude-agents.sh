# setup-claude-agents.sh

# example
```
❯   team-lead エージェントを起動して、以下のタスクを処理してほしい。

  ## タスク: ToDo CLI ツールの作成

  Python で以下の機能を持つ CLI ツールを作成する。

  ### 機能
  - `todo add <text>`   : タスクを追加する
  - `todo list`         : タスク一覧を表示する（ID・内容・完了状態）
  - `todo done <id>`    : タスクを完了済みにする
  - `todo delete <id>`  : タスクを削除する

  ### 仕様
  - データは `~/.todo.json` に JSON 形式で永続化する
  - ID は追加順の連番とする
  - list は未完了と完了済みを区別して表示する
  - 存在しない ID を指定した場合はエラーメッセージを出す
  - 実行スクリプトはsrc配下、テストスクリプトはtest配下に作成する

⏺ team-lead として各エージェントを順番に起動します。まず designer から始めます。

⏺ designer(Design Todo CLI tool)
  ⎿  Done (2 tool uses · 9.1k tokens · 38s)
  (ctrl+o to expand)

⏺ 設計完了。次に implementer を起動します。

⏺ implementer(Implement Todo CLI tool)
  ⎿  Done (22 tool uses · 21.2k tokens · 1m 57s)
  (ctrl+o to expand)

⏺ 実装・テスト完了 (49テスト通過)。次に code-simplifier を起動します。

⏺ code-simplifier(Simplify Todo CLI code)
  ⎿  Done (22 tool uses · 25.5k tokens · 1m 27s)
  (ctrl+o to expand)

⏺ 簡潔化完了。次に code-reviewer を起動します。

⏺ code-reviewer(Review Todo CLI code)
  ⎿  Done (7 tool uses · 19.1k tokens · 45s)
  (ctrl+o to expand)

⏺ CHANGES_REQUESTED。implementer に修正を依頼します。

⏺ implementer(Fix reviewer issues in Todo CLI)
  ⎿  Done (21 tool uses · 28.6k tokens · 1m 30s)
  (ctrl+o to expand)

⏺ 修正完了 (50テスト通過)。再度 code-reviewer を起動します。

⏺ code-reviewer(Re-review fixed Todo CLI code)
  ⎿  Done (7 tool uses · 19.0k tokens · 45s)
  (ctrl+o to expand)

⏺ 再度 implementer に修正を依頼します。

⏺ implementer(Fix remaining reviewer issues)
  ⎿  Done (15 tool uses · 19.4k tokens · 48s)
  (ctrl+o to expand)

⏺ 修正完了 (52テスト通過)。再度 code-reviewer を起動します。

⏺ code-reviewer(Final code review Todo CLI)
  ⎿  Done (7 tool uses · 18.0k tokens · 23s)
  (ctrl+o to expand)

⏺ code-reviewer APPROVED。integration-tester と security-reviewer を並行起動します。

⏺ 2 agents finished (ctrl+o to expand)
   ├─ integration-tester (Integration test Todo CLI) · 10 tool uses · 14.7k tokens
   │  ⎿  Done
   └─ security-reviewer (Security review Todo CLI) · 3 tool uses · 7.5k tokens
      ⎿  Done

⏺ integration-tester SKIPPED、security-reviewer APPROVED。ui-tester を起動します。

⏺ ui-tester(UI test Todo CLI)
  ⎿  Done (4 tool uses · 12.3k tokens · 22s)
  (ctrl+o to expand)

⏺ 全ゲート通過。git-syncer を起動します。

⏺ git-syncer(Git commit Todo CLI)
  ⎿  Done (0 tool uses · 7.9k tokens · 6s)
  (ctrl+o to expand)

⏺ 全工程完了です。

  完了サマリー

  ┌──────────────┬────────────────────┬──────────────────────────┐
  │   ステップ   │    エージェント    │           結果           │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ 設計         │ designer           │ DESIGNED                 │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ 実装         │ implementer        │ PASSED (49テスト)        │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ 簡潔化       │ code-simplifier    │ PASSED                   │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ レビュー     │ code-reviewer      │ APPROVED (2回差し戻し後) │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ 統合テスト   │ integration-tester │ SKIPPED                  │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ セキュリティ │ security-reviewer  │ APPROVED                 │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ UIテスト     │ ui-tester          │ SKIPPED                  │
  ├──────────────┼────────────────────┼──────────────────────────┤
  │ Git          │ git-syncer         │ SKIPPED (.git なし)      │
  └──────────────┴────────────────────┴──────────────────────────┘

  作成ファイル

  todo-cli/
  ├── docs/design/todo-cli.md
  ├── src/
  │   ├── task.py      # Task データクラス・ドメインロジック
  │   ├── storage.py   # ~/.todo.json の読み書き (DataCorruptedError, アトミック書き込み)
  │   └── todo.py      # CLI エントリポイント
  └── tests/
      ├── conftest.py
      ├── test_task.py
      ├── test_storage.py
      └── test_cli.py   (52テスト)

  使い方

  python3 src/todo.py add "買い物に行く"
  python3 src/todo.py list
  python3 src/todo.py done 1
  python3 src/todo.py delete 1

  git で管理したい場合は git init を実行してください。
```
