# 発表資料

スライドは [Marp](https://marp.app/) で作る。Markdown で書いた `slides.md` がそのままスライドになる。発表ごとに `YYYY-MM-DD_<内容>/` を作って置く。日付は発表の日にして、[notes](../notes/README.md) のディレクトリ名とそろえる。

| 日付 | 内容 | ディレクトリ |
| --- | --- | --- |
| （例）2026-10-09 | 進捗報告 | `2026-10-09_progress/` |

Marp で作ると、次のような点が楽になる。

- PowerPoint を開かずに、エディタで本文を書くだけでスライドになる。見た目はテーマ（下の「[見た目を変える](#見た目を変える)」）でそろう。
- 図は `experiments/<名前>/fig/` の図を相対パスで参照するので、計算や解析をやり直して図を作り直せば、スライドの図も新しくなる。
- 数式を LaTeX の記法で書ける。ノートに書いた式をそのまま貼れる。
- テキストなので、git で前の発表との差分が見られる。

## 準備

VS Code に拡張機能 [Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode) を入れる。このリポジトリを VS Code で開くと、おすすめの拡張機能として表示される（[.vscode/extensions.json](../.vscode/extensions.json)）。

テーマ [theme/research.css](theme/research.css) は [.vscode/settings.json](../.vscode/settings.json) で登録してあるので、**リポジトリのフォルダごと VS Code で開く**（`code .`）。`slides.md` だけを開いたり、親のフォルダを開いたりすると、テーマが読まれず `The specified theme "research" is not recognized` と出る。

## 新しいスライドを作る

1. テンプレートをディレクトリごとコピーする。

   ```sh
   cp -r slides/template slides/2026-10-09_progress
   ```

2. VS Code で `slides.md` を開き、プレビュー（エディタ右上のボタン、または `Ctrl+K V`）を見ながら書く。
3. 見本のスライドを自分の内容に書き換え、見本の図 `sample.svg` は消す。
4. 図はコピーせず、`experiments/<名前>/fig/` の図を相対パスで参照する（例: `![h:420](../../experiments/xxx/fig/yyy.png)`）。

各発表のディレクトリに置くもの:

| ファイル | 内容 |
| --- | --- |
| `slides.md` | スライド本体 |
| `note.md` | 下書き（スライドごとに載せる内容と元ネタ）。必要なら |
| `feedback.md` | 発表で受けた指摘・質問と、次回に向けてやること。コミットは `feedback(<日付>): ...` |
| `slides.pdf` など | 書き出したもの。git では追跡しない（[.gitignore](../.gitignore)） |

## 書き方

基本は普通の Markdown で、`---` でスライドを区切り、`#` でスライドのタイトルを書く。このテンプレートで使える書き方は次のとおり。どれも [template/slides.md](template/slides.md) に見本がある。

| 書き方 | 効果 |
| --- | --- |
| `<!-- _class: title -->` | タイトルスライド |
| `<!-- _class: chapter -->` | 章の扉 |
| `<!-- _class: fig -->` | 図を中央に置き、図の下の文をキャプションにする |
| `<!-- _class: small -->` | 本文の文字を小さくする |
| `![bg right:48% fit](図のパス)` | 図を右側、文を左側に並べる |
| `> 文` | スライドの下端に小さく出す（出典・注） |
| `$...$`、`$$...$$` | 数式（LaTeX の記法） |
| `<!-- 文 -->` | 発表者ノート（スライドには出ない） |

4:3 にするときは、`slides.md` の先頭の `size: 16:9` を `size: 4:3` にする。

## 見た目を変える

見た目（文字の大きさ・色・配置）は [theme/research.css](theme/research.css) にまとめてあり、`slides.md` の先頭の `theme: research` で読み込む。Marp の default テーマをもとに、必要なところだけ変えている。

- **全部の発表で変える**: `theme/research.css` を直す。先頭の `--base-size`（本文の文字の大きさ）や `--accent`（見出しの線や強調の色）を変えるのが手軽。
- **その発表だけ変える**: `slides.md` の最後に `<style>` を書く。テーマの設定より後に読まれるので、こちらが優先される。

  ```html
  <style>
  section {
    --base-size: 24px;
  }
  </style>
  ```

- **別のテーマを足す**: `theme/` に CSS を置き、先頭に `/* @theme 名前 */` を書く。VS Code では [.vscode/settings.json](../.vscode/settings.json) の `markdown.marp.themes` にも足す。

## PDF にする

VS Code のエディタ右上の Marp のボタン →「Export Slide Deck...」で、ファイルの種類を PDF か PPTX にして書き出す。書き出しには Chrome・Edge・Firefox のどれかが要る。

[Marp CLI](https://github.com/marp-team/marp-cli) を使うときは、リポジトリの直下で実行する。テーマは [.marprc.yml](../.marprc.yml) で登録してある。

```sh
npx @marp-team/marp-cli slides/2026-10-09_progress/slides.md --pdf
```

### WSL で使っているとき

WSL に Linux 側のブラウザが無いと、PDF を直接書き出せない（`No suitable browser found` で失敗する）。HTML の書き出しはブラウザを使わないので、HTML を経由して Windows の Chrome で PDF にする。

1. 「Export Slide Deck...」で、ファイルの種類を HTML にして `slides.md` と同じディレクトリに `slides.html` として保存する。別の場所に保存すると図のパスがずれる。
2. その HTML を Windows の Chrome で PDF にする。

   ```sh
   cd slides/2026-10-09_progress
   "/mnt/c/Program Files/Google/Chrome/Application/chrome.exe" --headless --no-pdf-header-footer \
     --print-to-pdf="$(wslpath -aw slides.pdf)" "file:$(wslpath -aw slides.html | tr '\\' '/')"
   ```

`slides.html` をブラウザで開けば、そのまま発表にも使える（矢印キーで移動、`F` で全画面）。
