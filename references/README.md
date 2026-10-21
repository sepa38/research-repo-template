# 参考文献

研究で読む論文・本・資料をここにまとめる。読むたびに書誌情報を [references.bib](references.bib) に足しておけば、卒論・修論の参考文献リストを最後にまとめて探し直さなくて済む。

| 置くもの | 内容 |
| --- | --- |
| [references.bib](references.bib) | 書誌情報（BibTeX）。参考文献リストはここから作る |
| この README | 資料ごとのメモ（何がどこに書いてあるか）と、読んだかどうか |
| `<キー>.pdf` | 資料の PDF。git では追跡しない（[.gitignore](../.gitignore)） |

## 資料を足す

1. **キーを決める。** `<第一著者の姓><年><タイトルの最初の単語>` を小文字で（例: `hohenberg1964inhomogeneous`）。Web ページなど著者がいないものは、サイト名などで決める。
2. **BibTeX を [references.bib](references.bib) に足す。** 自分で打たずに取ってくる。
   - DOI がある論文は、DOI から取れる。キーは 1 で決めたものに書き換える。

     ```sh
     curl -sLH "Accept: application/x-bibtex" https://doi.org/10.1103/PhysRev.136.B864
     ```

   - Google Scholar の検索結果の「引用」→「BibTeX」からも取れる。
   - Web ページ、講習会のスライド、先輩の卒論などは `@misc` にして、`url` と参照日（`note = {2026-10-02 参照}` など）を書く。
3. **この README の「一覧」に足す。** キー、タイトル、読んで何がどこに書いてあったか（節・ページ）をメモする。
4. **PDF があれば `<キー>.pdf` の名前でこのディレクトリに置く。**

## 引用する

- ノートやスライドでは、キーとページで書いておく（例: `[hohenberg1964inhomogeneous, p.B865]`）。卒論ではそのまま `\cite{hohenberg1964inhomogeneous}` に置き換えられる。
- LaTeX で書くときは、原稿から `references.bib` を参照する（例: `thesis/` に原稿を置くなら `\bibliography{../references/references}`）。
- Word で書くときは、[Zotero](https://www.zotero.org/) などの文献管理ソフトに `references.bib` を読み込めば、参考文献リストを作れる。
- スライドの出典（`> 出典: ...`）も、`references.bib` の書誌情報から写す。

## 一覧

### 指定された資料

TODO: 指導教員や先輩から読むように言われた資料を書く（論文、教科書、研究室の Wiki、先輩の卒論など）。

- [ ] `キー` **タイトル**（著者, 年）
  - リンク
  - 何がどこに書いてあるか

### 自分で見つけた資料

- [ ] `キー` **タイトル**（著者, 年）
  - 何がどこに書いてあるか
