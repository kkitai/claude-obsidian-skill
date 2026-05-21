# Vault 構造テンプレート

このファイルは各設定ファイルの具体的な内容を定義する。
`<project-name>` と `<project-path>` は実際の値に置き換えること。

---

## .claude/settings.local.json

```json
{
  "env": {},
  "notes": {
    "project": "<project-name>",
    "project_path": "<project-path>",
    "tech_stack": {
      "language": "TODO: 使用言語を記入 (例: TypeScript, Python)",
      "framework": "TODO: フレームワークを記入 (例: Next.js, FastAPI)",
      "database": "TODO: DBを記入 (例: PostgreSQL, SQLite)",
      "infra": "TODO: インフラを記入 (例: Vercel, AWS)"
    },
    "preferences": {
      "language": "ja",
      "date_format": "YYYY-MM-DD",
      "note_style": "zettelkasten"
    }
  }
}
```

---

## .claude/commands/daily.md

```markdown
今日（$ARGUMENTS または今日の日付）の日次レビューを実施してください。

以下の手順で進めてください：

1. 00_Inbox/ フォルダの未処理メモを確認する
2. 01_Projects/<project-name>/ の進行中タスクを確認する
3. 以下の形式でデイリーノートを作成または更新する：

ファイルパス: 01_Projects/<project-name>/daily/YYYY-MM-DD.md

## YYYY-MM-DD のデイリーログ

### 今日のタスク
- [ ] 

### 完了したこと


### 明日への持ち越し
- [ ] 

### メモ・気づき


```

---

## .claude/commands/inbox-review.md

```markdown
00_Inbox/ フォルダの未整理メモを分類・整理してください。

手順：
1. 00_Inbox/ 内の全ファイルを一覧表示する
2. 各ファイルの内容を読み込む
3. 以下の基準で分類先を提案する：
   - プロジェクト関連 → 01_Projects/<project-name>/
   - 継続的なテーマ → 02_Areas/
   - 参考情報・リサーチ → 03_Resources/
   - 古い/不要なもの → 04_Archive/

4. 分類案をリスト形式で提示する：

| ファイル名 | 現在地 | 移動先候補 | 理由 |
|-----------|--------|----------|------|

5. ユーザーの確認を得てから実際に移動する
```

---

## .claude/commands/research.md

```markdown
「$ARGUMENTS」についてこのVault内をクロス検索し、関連ノートをまとめてください。

手順：
1. Vault内のすべての.mdファイルを対象に「$ARGUMENTS」に関連するキーワードで検索する
2. 関連度が高い順にノートをリストアップする
3. 各ノートの関連箇所を引用する
4. 関連ノート同士のつながりを図示する（テキストで）
5. まだ書かれていない視点・リンクを提案する

出力形式：
## 「$ARGUMENTS」に関するリサーチまとめ

### 関連ノート
1. [ノート名](パス) — 関連する内容の要約

### キーインサイト


### 未探索の視点
```

---

## .claude/commands/mtg.md

```markdown
以下の会議メモから、構造化されたログを作成してください。

$ARGUMENTS（または 00_Inbox/ の最新ファイル）を読み込み、以下の形式で整理してください：

保存先: 01_Projects/<project-name>/meetings/YYYY-MM-DD_<会議名>.md

## 会議ログ: <会議名>

- 日時: YYYY-MM-DD HH:MM
- 参加者: 
- 目的: 

### アジェンダ
1. 

### 議事録


### 決定事項
- [ ] 

### アクションアイテム
| タスク | 担当者 | 期限 |
|--------|--------|------|

### 次回までの宿題
```

---

## .claude/rules/folder-structure.md

```markdown
# Vault フォルダ構造ルール

このVaultは以下のフォルダ構造で管理されています。
ファイルを作成・移動する際は必ずこのルールに従ってください。

## フォルダの役割

### 00_Inbox/
- 目的: 未整理メモの一時置き場
- ルール: ここに置くのは一時的。定期的に /inbox-review で整理する
- ファイル名: 自由（日付プレフィックス推奨: YYYYMMDD_タイトル.md）

### 01_Projects/
- 目的: アクティブなプロジェクトのノート
- 構造: 01_Projects/<プロジェクト名>/ 以下にサブフォルダで管理
- サブフォルダ例: daily/, meetings/, tasks/, docs/

### 02_Areas/
- 目的: 継続的に管理・更新する関心領域
- 例: 技術学習, 健康, キャリア, 読書記録
- ルール: プロジェクトではなく「継続中のテーマ」を置く

### 03_Resources/
- 目的: 参考資料・リサーチ・外部情報のまとめ
- 例: 技術ドキュメントメモ, 記事の要約, 書籍ノート

### 04_Archive/
- 目的: 完了・不要になったノートの保管庫
- ルール: 削除する前にここへ移動する

## ファイル命名規則
- 日本語ファイル名OK
- スペースはハイフン（-）に変換
- 日付はYYYY-MM-DD形式
- タイトルは内容が一目でわかる具体的な名前にする

## リンクの使い方
- [[ノート名]] で内部リンクを作成する
- 関連するノートは積極的にリンクする
- タグは #タグ名 形式で記述する
```

---

## README.md

```markdown
# <project-name> Obsidian Vault

このVaultは「<project-name>」プロジェクトの知識管理とClaude Code連携のために作成されました。

## プロジェクト情報
- プロジェクトパス: <project-path>
- 作成日: YYYY-MM-DD

## フォルダ構造
- `00_Inbox/` — 未整理メモの一時置き場
- `01_Projects/` — プロジェクト別ノート
- `02_Areas/` — 継続的な関心領域
- `03_Resources/` — 参考資料・リサーチ
- `04_Archive/` — アーカイブ

## Claude Code 連携コマンド

このVaultフォルダで `claude` を起動すると以下のコマンドが使えます：

| コマンド | 機能 |
|---------|------|
| `/daily` | 今日のタスク・ログ整理 |
| `/inbox-review` | 未整理メモの自動分類提案 |
| `/research <キーワード>` | Vault内クロス検索 |
| `/mtg` | 会議ログの構造化 |

## 設定
個人の技術スタックや設定は `.claude/settings.local.json` を編集してください。
```
