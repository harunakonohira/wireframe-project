# ワイヤーフレーム自動生成プロジェクト

Claude CodeとFigma MCPを使って、ヒアリングシートからワイヤーフレームを自動生成するプロジェクトです。

---

## このプロジェクトでできること

1. 事前ヒアリングシートを記入する
2. Claude Codeに指示する
3. Figmaにワイヤーフレームが自動で生成される

**Claude Codeが自動でやってくれること：**
- 事前ヒアリングをもとに本ヒアリングシートを先埋め
- ページ構成と仮テキストの提案
- FigmaへのWF出力

---

## はじめる前に必要なもの

- [ ] VSCode（インストール済み）
- [ ] Claude Code for VS Code（VSCodeの拡張機能）
- [ ] Claudeのアカウント（ProまたはMaxプラン）
- [ ] Figmaのアカウント（プロフェッショナルプラン以上）
- [ ] Figmaデスクトップアプリ（ブラウザ版では動きません！）

> ⚠️ FigmaのAPIトークンは不要です！ログイン認証だけでOKです。

---

## セットアップ手順

### 1. このプロジェクトをダウンロードする

GitHubからクローンする場合：
```bash
git clone https://github.com/リポジトリのURL.git
```

ZIPでダウンロードした場合はそのまま解凍してください。

### 2. VSCodeでプロジェクトを開く

```
ファイル → フォルダを開く → wireframe-projectフォルダを選択
```

### 3. Figma MCPを設定する（3ステップ）

#### ステップ① FigmaデスクトップアプリでDev Modeを有効にする

1. FigmaデスクトップアプリでDesignファイルを新規作成して開く
   （⚠️ ファイルを開いていないと設定が出てきません！）
2. 画面右側のパネルで「Dev」タブに切り替える
3. 「MCP」セクションの「サーバーステータス：有効」を確認する
   （なっていない場合はトグルをオンにする）

#### ステップ② Figmaプラグインをインストールする

VSCodeのターミナルを新しいタブで開いて以下を実行：

```bash
claude plugin install figma@claude-plugins-official
```

「Successfully installed」と表示されたらOK！

#### ステップ③ Claude Codeを起動してFigmaにログイン認証する

VSCodeのターミナルでClaude Codeを起動：

```bash
claude
```

起動したら以下を入力：

```
/mcp
```

一覧が表示されるので `plugin:figma:figma · △ needs authentication` を選択してEnterを押す。

ブラウザが開いてFigmaのログイン画面が出るので「Allow Access」をクリック。

> ✅ APIトークンの入力は不要です！「Allow Access」をクリックするだけでOK！

「Authentication successful」と表示されたら設定完了！

#### 接続確認

もう一度 `/mcp` を入力して `plugin:figma:figma · ✔ connected` になっていればOK！

---

## 使い方

### 新しいクライアントのWFを作るときの手順

**① クライアントフォルダを作る**

Claude Codeに以下のように指示するだけ！

```
クライアントフォルダを作って、名前は「クライアント名」
```

Claudeが自動で `clients/_template/` をコピーして作ってくれます。

**② 事前ヒアリングシートを記入する**

`clients/クライアント名/pre-hearing.md` を開いて、クライアントから聞いた内容を記入する。

**③ Claude Codeに指示する**

```
clients/クライアント名/pre-hearing.md を読んで、
hearing.md の埋められる項目を先に埋めてください。
```

**④ あとはClaudeの指示に従うだけ**

Claudeが以下を順番にやってくれる：

1. hearing.mdの先埋め → 残りの確認項目を教えてくれる
2. 本ヒアリングで残りを埋めたら「hearing埋まったよ」と伝える
3. ページ構成と仮テキストの提案 → 確認・修正
4. OKを出したらFigmaに自動出力

---

## ショートカット指示一覧

| 言葉 | Claudeがやること |
|---|---|
| 「クライアントフォルダ作って、名前は〇〇」 | `_template`をコピーして自動作成 |
| 「〇〇のpre-hearingを読んで」 | STEP1開始・hearing.mdを先埋め |
| 「hearing埋まったよ」 | STEP2→3・仮構成と仮テキストを提案 |
| 「OKです」 | Figma出力開始 |
| 「修正して〇〇に変えて」 | 修正して再提案 |
| 「どこまで進んでる？」 | 現在の進捗を報告 |

---

## フォルダ構成

```
wireframe-project/
├── CLAUDE.md                    ← Claudeへの指示書（編集しない！）
├── README.md                    ← このファイル
├── clients/                     ← クライアントごとのフォルダ
│   └── _template/               ← ひな形（コピーして使う・触らない！）
│       ├── pre-hearing.md       ← 事前ヒアリングシート
│       ├── hearing.md           ← 本ヒアリングシート
│       └── output/              ← WFサマリーの保存先
├── guidelines/                  ← Claudeが読むルール集（編集しない！）
│   ├── wireframe-rules.md
│   └── figma-output-guide.md
└── templates/                   ← ひな形ファイル
    ├── pre-hearing.md
    └── hearing.md
```

---

## よくある質問

**Q. 「Dev」タブやMCPセクションが見当たらない**
A. Figmaデスクトップアプリでデザインファイルを開いた状態でないと表示されません。必ずファイルを開いてから確認してください。

**Q. Figma MCPの認証画面が出てこない**
A. Claude Codeを再起動してから `/mcp` で `plugin:figma:figma` を選択してみてください。

**Q. Figmaへの出力が途中で止まった**
A. FigmaデスクトップアプリのDev Mode MCPが有効になっているか確認してください。サーバーはFigmaが開いている間だけ動作します。

**Q. Claude Codeが動かない**
A. ClaudeのProまたはMaxプランに加入しているか確認してください。無料プランではClaude Codeは使用できません。

**Q. CLAUDE.mdやguidelinesのファイルは編集していい？**
A. 基本的には編集しないでください。WFのルールを変えたい場合は `guidelines/wireframe-rules.md` のみ編集してください。

---

## 注意事項

- `clients/_template/` フォルダは絶対に直接編集しないこと
- `CLAUDE.md` と `guidelines/` 内のファイルは編集しないこと
- FigmaデスクトップアプリはWF生成中は必ず開いたままにしておくこと