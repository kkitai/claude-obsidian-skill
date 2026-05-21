# claude-obsidian-template

Claude Code と Obsidian を連携させるためのスキル管理リポジトリです。

## スキル一覧

### `/obsidian`

開発プロジェクト用の Obsidian vault 連携環境をセットアップします。

**できること:**
- Obsidian vault の作成（デフォルト: `~/Documents/Obsidian/<project-name>`）
- Claude Code 連携に必要な `.claude/` フォルダ構造のセットアップ
- カスタムスラッシュコマンドの配置（`/daily`, `/inbox-review`, `/research`, `/mtg`）
- PARA メソッドに基づくフォルダ構造の作成

**使い方:**
```
/obsidian
```

## インストール方法

```bash
# スキルをインストール
claude skills install obsidian/
```

## 参考

- [Claude Code + Obsidian 連携ガイド](https://note.com/antrefreepreneur/n/nabdcbc1aa26b)
