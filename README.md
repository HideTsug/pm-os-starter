# PM-OS Starter Kit

プロジェクトマネジメントを **Claude Code と一緒に「OS化」して進める** ためのスターターキット。

「タスクをこなす」のではなく、**プロジェクトの状態・意思決定・文脈を構造化して蓄積し、AI（Claude Code）が外部記憶として読み書きしながら伴走する**運用を最短で立ち上げる。

---

## これは何か

3つの要素でできている。

1. **運用原則（`CLAUDE.md`）** — Claude Code が毎セッション自動で読む。働き方・調査プロトコル・セッションの始め方/終え方を定義。
2. **ワークスペース（`workspace/`）** — プロジェクト・意思決定・会議・関係者を Markdown で蓄積するナレッジベース。`_active-context.md` がセッション間のワーキングメモリ（=現在地ボード）。
3. **判断 Skill（`.claude/skills/`）** — 「やるべきか」「何が根本原因か」「ゴールは検証可能か」を構造的に判断する意思決定プロトコル。Claude Code がこのリポジトリで作業すると自動で使える。

---

## セットアップ（Claude Code のみ・5分）

前提: [Claude Code](https://claude.com/claude-code) が手元で動くこと。Obsidian 等は不要（全部ただの Markdown）。

```bash
# 1. リポジトリを取得
git clone <このリポジトリのURL> pm-os
cd pm-os

# 2. Claude Code を起動
claude
```

起動すると Claude Code がリポジトリ直下の `CLAUDE.md` を自動で読み込む。これで運用原則と Skill が効いた状態になる。

---

## 最初の一歩: プロジェクトを「ダンプ」する

頭の中・既存資料にあるプロジェクト情報を、対話で構造化しながら `workspace/` に流し込む。

`docs/intake-prompt.md` の中身をコピーして、Claude Code に最初のメッセージとして貼るだけ。あとは Claude が質問しながら、プロジェクトノート・意思決定ログ・関係者ノートを作っていく。

```
（claude 起動後、docs/intake-prompt.md の内容を貼る）
```

---

## 日々の進め方

毎回のセッションはこの3手で回る。詳細は `docs/workflow.md`。

1. **開始**: Claude が `workspace/_active-context.md` を読んで現在地を把握する（「現状を教えて」でOK）。
2. **作業**: 相談・調査・タスク分解・意思決定。判断が必要な場面では Skill が自動で発動する。
3. **終了**: 進捗・決定・次アクションを `_active-context.md` と各ノートに反映する（「今日のぶんを記録して」でOK）。

---

## ディレクトリ構成

```
pm-os/
├── CLAUDE.md                    # 運用原則（Claude Codeが自動読込）
├── README.md
├── .claude/skills/             # 判断Skill（自動発動）
│   ├── issue-setting/          # やるべきか? の判断
│   ├── decision-5step/         # 根本原因→計画 の意思決定
│   └── goal-driven-execution/  # 検証可能なゴール定義
├── workspace/                   # ナレッジベース本体
│   ├── _active-context.md      # 現在地ボード（最重要）
│   ├── projects/               # プロジェクトノート
│   ├── decisions/              # 意思決定ログ
│   ├── meetings/               # 会議メモ
│   ├── people/                 # 関係者
│   └── inbox/                  # 未整理メモ
├── templates/                   # ノートのひな形（SSoT）
└── docs/
    ├── intake-prompt.md        # 初回ダンプ用プロンプト
    └── workflow.md             # 日々の進め方
```

---

## 設計思想（なぜこの形か）

- **AIファースト** — 散文的な読みやすさより、検索性・構造化・情報密度を優先。主な読者は人間ではなく Claude。
- **Single Source of Truth** — 同じ情報を複数箇所に書かず、ファイル名で参照する。
- **状態を外に出す** — 頭の中・チャット履歴に状態を溜めず、`workspace/` に書き出す。次のセッションの Claude が続きから動ける。
- **意思決定を残す** — 「何を決めたか」だけでなく「なぜ・どの選択肢を捨てたか・撤退条件」を残す。後から検証できる。
