# 研究リポジトリのテンプレート

卒業研究・修士研究を 1 つの git リポジトリで管理するためのひな形。計算・実験のデータと解析、学習ノート、参考文献、発表スライド、予定と進捗を、決まった場所に決まった書き方で置いていく。

分野は問わない。第一原理計算の卒業研究で使っていた構成から、分野によらない部分を取り出して作った。計算でも、実験・測定でも、シミュレーションでも同じ構成で使える。

## このテンプレートについて

> 自分のリポジトリを作ったら、この節は消す。

### 何が楽になるか

- **スライドを Markdown で書ける。** [Marp](https://marp.app/) のテンプレートとテーマが入っていて、VS Code で書けばそのままスライドになる。図は計算・実験のディレクトリの図を直接参照するので、解析をやり直せばスライドの図も新しくなる（[slides](slides/README.md)）。
- **`git log` が研究日誌になる。** コミットメッセージを `exp(xrd-a): 試料 A の XRD を測定した` のように「何を変えたか」で分けて書くので、履歴を見れば、いつ何をしたかがわかる（[コミットメッセージ](#コミットメッセージ)）。
- **卒論の参考文献リストがすぐ作れる。** 読んだ資料の書誌情報を BibTeX で [references/references.bib](references/references.bib) に貯めていくので、卒論・修論を書くときに探し直さなくて済む。LaTeX ならそのまま参照でき、Word なら Zotero などに読み込める（[references](references/README.md)）。
- **計算・実験の記録の書き方が決まっている。** 1 つの計算・実験ごとにディレクトリを作り、目的・条件・結果を README に書く。生データ、解析のスクリプト、図が同じ場所にあるので、どの図がどのデータから作られたかをあとでたどれる（[experiments](experiments/README.md)）。
- **予定と実績が 1 か所にある。** 進捗報告までにやることと、やったことの記録を [schedule.md](schedule.md) にまとめる。
- **[Claude Code](https://claude.com/claude-code) などの AI にルールを渡せる。** [CLAUDE.md](CLAUDE.md) に置き場所や規約が書いてあるので、AI に作業させても同じ書き方になる。

### 使い始める

1. GitHub でこのリポジトリの「Use this template」→「Create a new repository」から自分のリポジトリを作る。研究室の資料や未発表の結果を入れることになるので、**Private** にする（下の「[公開について](#公開について)」）。
2. 自分の研究に合わせて書き換える。書き換える箇所には `TODO` と書いてあるので、`grep -rn TODO .` で探せる。
   - この README: タイトルと最初の 2 段落を自分の研究テーマにし、この「このテンプレートについて」の節を消す
   - [CLAUDE.md](CLAUDE.md): 研究テーマと、計算・測定をどこでどう行うか
   - [schedule.md](schedule.md): 最初の予定
   - [references/README.md](references/README.md): 指導教員や先輩から指定された資料
3. 分野に合わせて決める（どれも後から変えられる）。
   - [experiments/README.md](experiments/README.md) の中の構成（例: 計算なら `in/`・`out/`、実験なら `data/`）
   - [.gitignore](.gitignore) で追跡しない大きいデータのパターン
   - 計算が中心なら、`experiments/` を `calc/` に、コミットの type の `exp` を `calc` に変えてもよい。変えたら README と CLAUDE.md も合わせる
4. VS Code でリポジトリのフォルダを開き、おすすめの拡張機能 Marp for VS Code を入れる。解析に Python を使うなら環境も作る（下の「[Python の環境](#python-の環境)」）。

## 予定と進捗

[schedule.md](schedule.md) に書く。

## ディレクトリ構成

```text
.
├── experiments/       計算・実験ごとのデータ・解析・図と記録
│   └── template/      新しく作るときにコピーするひな形
├── notes/             学習ノート
├── references/        参考文献（BibTeX と読んだメモ。PDF は git で追跡しない）
├── slides/            発表スライド（Marp）
│   ├── template/      新しく作るときにコピーするひな形
│   └── theme/         スライドの見た目（CSS）
├── schedule.md        予定と進捗
├── requirements.txt   解析に使う Python のパッケージ
├── CLAUDE.md          AI（Claude Code）向けの説明
├── .marprc.yml        Marp CLI の設定（テーマの登録）
└── .vscode/           VS Code の設定（テーマの登録、おすすめの拡張機能）
```

卒論・修論の原稿などは、必要になったらディレクトリを追加する（例: `thesis/`）。

## 計算・実験の管理

詳しくは [experiments/README.md](experiments/README.md)。

- 計算・実験ごとに `experiments/<名前>/` を作る（`cp -r experiments/template experiments/<名前>`）。関連するものはまとめてよい（例: 練習のものは `experiments/tutorial/<名前>/`）。
- 生データは `data/`、図は `fig/`、解析や図を作るスクリプトは `tools/` に置く。生データは書き換えず、補正や換算はスクリプトで行う。
- 大きいファイルやバイナリは [.gitignore](.gitignore) で追跡しないようにし、置き場所を README に書く。
- 各 `README.md` に、日付・目的・条件・結果（うまくいったか、主な数値、図）を書く。

## Python の環境

解析に Python を使うときは、リポジトリの中に仮想環境を作る（`.venv/` は git で追跡しない）。使うパッケージは [requirements.txt](requirements.txt) に書いておく。

```sh
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
```

## コミットメッセージ

`<type>(<scope>): <要約>` の形で書く。要約は日本語で短く。scope は省略してよい。

| type | 使うとき | 例 |
| --- | --- | --- |
| `exp` | 計算・実験の入力・データ・記録 | `exp(xrd-a): 試料 A の XRD を測定した` |
| `analysis` | 解析スクリプト・図 | `analysis(xrd-a): ピーク位置を求めるスクリプトを追加` |
| `notes` | 学習ノート | `notes: 先行研究のまとめを書いた` |
| `refs` | 参考文献 | `refs: 先行研究の論文を 3 件追加` |
| `slides` | 発表資料 | `slides(1009): 進捗報告のスライドを作成` |
| `feedback` | 発表へのフィードバック | `feedback(1009): 10/09 の進捗報告へのフィードバックを追加` |
| `thesis` | 卒論・修論の原稿 | `thesis(ch2): 方法の章を追加` |
| `fix` | 結果や記述に関わる誤りの修正 | `fix(xrd-a): 換算の誤りを直して再解析` |
| `docs` | README など、リポジトリの説明 | `docs: 予定を更新` |
| `chore` | 設定・構成の変更 | `chore: .gitignore を更新` |

- feat のような「変更の種類」ではなく、「何を変えたか」で type を選ぶ。
- scope には、`exp` と `analysis` なら `experiments/` から下のディレクトリのパス（`xrd-a`、`tutorial/sample1` など）、`slides` と `feedback` なら発表日、`thesis` なら章を書く。
- `exp` では、うまくいったかどうかや主な数値を本文に書いておく。
- `fix` だけは「誤りの修正」という種類で分ける。どの結果がいつ直ったかを追えるようにするため。本文には影響を受ける結果を書く。

こう書いておくと、履歴がそのまま研究の記録として読める。

```sh
git log --format='%ad %s' --date=short    # 日付つきの研究日誌
git log --oneline --grep '^fix'           # 誤りを直した記録
git log --oneline -- experiments/xrd-a    # 1 つの計算・実験の履歴
```

```text
2026-10-14 exp(xrd-a): 試料 A の XRD を測定した
2026-10-14 docs: 予定を更新
2026-10-10 feedback(1009): 10/09 の進捗報告へのフィードバックを追加
2026-10-08 slides(1009): 進捗報告のスライドを作成
2026-10-07 refs: 先行研究の論文を 3 件追加
```

## 公開について

このテンプレートから作ったリポジトリには、研究室の内部資料（Wiki、先輩の卒論など）の内容や未発表の結果が入るので、GitHub などに置くときは private にする。参考文献の PDF とスライドの書き出しは [.gitignore](.gitignore) で追跡しないようにしてあるが、ノートや README に書いた内容は追跡される。
