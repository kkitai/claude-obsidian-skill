---
name: obsidian
description: 開発プロジェクト用のObsidian vault連携をセットアップするスキル。「/obsidian」と入力されたとき、または「obsidianと連携したい」「obsidian設定」「vault設定」「obsidianをセットアップして」などのリクエストが来たとき必ず使用すること。カレントディレクトリの開発プロジェクト向けにObsidian vaultを作成し、Claude Codeとの連携に必要な .claude/ フォルダ構造（settings.local.json, commands/, rules/）をセットアップします。プロジェクトの技術スタック・日次タスク管理・会議ログ・リサーチなどを一元管理できる環境を構築します。
---

# Obsidian 連携セットアップ

このスキルはカレントディレクトリの開発プロジェクトに対して、Obsidian vaultとClaude Codeの連携環境を構築します。

## セットアップの流れ

### Step 1: プロジェクト情報の収集

まず以下の情報を確認・収集する：

**プロジェクト名の決定**
```bash
# カレントディレクトリがgit repositoryか確認
git rev-parse --is-inside-work-tree 2>/dev/null
```

- git repositoryの場合 → `basename $(git rev-parse --show-toplevel)` でプロジェクト名を取得
- git repositoryでない場合 → ユーザーにプロジェクト名の入力を求める：
  ```
  カレントディレクトリはgitリポジトリではありません。
  プロジェクト名を入力してください（例: my-project）:
  ```

**パスの確定**
- プロジェクトパス: カレントディレクトリの絶対パス
- Obsidian vaultパス（デフォルト）: `~/Documents/Obsidian/<project-name>`
- ユーザーが引数でvaultパスを指定していれば、そちらを使用する

### Step 2: 設定内容の確認とユーザーへの提示

以下の内容をユーザーに表示し、実行の可否を問う：

```
以下の設定でObsidian連携をセットアップします：

  プロジェクトパス : <project-path>
  Obsidian Vault  : <vault-path>

  作成されるVault構造：
  <vault-path>/
  ├── .claude/
  │   ├── settings.local.json   # 技術スタック・個人設定
  │   ├── commands/             # カスタムスラッシュコマンド
  │   │   ├── daily.md          # 日次タスク整理
  │   │   ├── inbox-review.md   # 未整理メモ分類
  │   │   ├── research.md       # Vault内クロス検索
  │   │   └── mtg.md            # 会議ログ構造化
  │   └── rules/
  │       └── folder-structure.md  # フォルダ構造ルール
  ├── 00_Inbox/                 # 未整理メモの一時置き場
  ├── 01_Projects/              # プロジェクト別ノート
  │   └── <project-name>/
  ├── 02_Areas/                 # 継続的な関心領域
  ├── 03_Resources/             # 参考資料・リサーチ
  ├── 04_Archive/               # アーカイブ
  └── README.md

続けますか？ (yes/no):
```

- ユーザーが **no** と回答したら → セットアップを中止する
- ユーザーが **yes** と回答したら → Step 3へ進む
- ユーザーがカスタムvaultパスを指定したい場合は受け付け、上記表示を更新して再確認する

### Step 3: Vault ディレクトリ構造の作成

ユーザーが承認したら、以下を作成する：

```bash
# ベースディレクトリ群
mkdir -p <vault-path>/.claude/commands
mkdir -p <vault-path>/.claude/rules
mkdir -p "<vault-path>/00_Inbox"
mkdir -p "<vault-path>/01_Projects/<project-name>"
mkdir -p "<vault-path>/02_Areas"
mkdir -p "<vault-path>/03_Resources"
mkdir -p "<vault-path>/04_Archive"
```

### Step 4: 設定ファイルの作成

以下のファイルを作成する。詳細なテンプレート内容は `references/vault_structure.md` を参照すること。

**作成するファイル一覧:**
1. `<vault-path>/.claude/settings.local.json` — 技術スタック・個人設定
2. `<vault-path>/.claude/commands/daily.md` — /daily コマンド
3. `<vault-path>/.claude/commands/inbox-review.md` — /inbox-review コマンド
4. `<vault-path>/.claude/commands/research.md` — /research コマンド
5. `<vault-path>/.claude/commands/mtg.md` — /mtg コマンド
6. `<vault-path>/.claude/rules/folder-structure.md` — フォルダ構造ルール
7. `<vault-path>/README.md` — Vault の説明

各ファイルの具体的な内容は `references/vault_structure.md` に定義されている。プロジェクト名やパスは実際の値に置き換えること。

### Step 5: 完了メッセージの表示

```
✓ Obsidian Vault のセットアップが完了しました！

  Vault パス: <vault-path>

次のステップ:
1. Obsidian でこのフォルダを Vault として開く
2. .claude/settings.local.json に技術スタックや個人情報を追記する
3. Vault フォルダで claude を起動してコマンドを試す:
   - /daily        → 今日のタスク整理
   - /inbox-review → 未整理メモの分類提案
   - /research     → ノートのクロス検索
   - /mtg          → 会議ログの構造化
```

## エラーハンドリング

- vaultパスが既に存在する場合: 上書き確認をユーザーに行う
- ディレクトリ作成権限がない場合: エラーメッセージを表示して別のパスを提案する
- プロジェクト名に特殊文字が含まれる場合: ファイルシステムに安全な名前に変換する（スペース→ハイフン など）
