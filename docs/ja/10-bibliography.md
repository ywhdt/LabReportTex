# 参考文献

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/10-bibliography.md)

テンプレートでは、本文中の引用と文末の参考文献を BibLaTeX で管理し、`.bib` データを Biber で読み込みます。通常は文献データを記入して本文から引用し、一般のレポートと同じ Latexmk コマンドを実行すれば十分です。

## 最小限の引用手順

参考文献データベースは、すでに [`report.tex`](../../template/report.tex) で登録されています。

```tex
\addbibresource{bibliography/references.bib}
```

まず、[`bibliography/references.bib`](../../template/bibliography/references.bib) に日本語文献を 1 件追加します。次はオンライン資料の例です。

```bibtex
@online{probe-guide,
  author       = {{電子計測資料センター}},
  title        = {受動プローブの補償と入力容量},
  organization = {電子計測資料センター},
  date         = {2025-11-18},
  url          = {https://example.com/measurement/passive-probe},
  urldate      = {2026-08-10},
  langid       = {japanese}
}
```

次に、本文から同じキーを引用します。

```tex
プローブの入力容量については公開資料を参照した\cite{probe-guide}。
```

最後に、`report.tex` があるディレクトリで次を実行します。

```console
latexmk -lualatex report.tex
```

必要であれば Latexmk が Biber を呼び出し、引用と参考文献一覧が安定するまで LuaLaTeX を繰り返し実行します。`lualatex` を 1 回だけ実行したり、Biber を単独で 1 回実行したりして、上のコマンドの代わりにしないでください。データベースの読み込み箇所は [`BIB-RESOURCE-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)、参考文献の結果は[サンプル PDF 第 20～21 ページ](../../examples/compiled/rc-low-pass-filter.pdf)で確認できます。

`.bib` に項目を追加するだけでは表示されません。本文で `\cite{...}` を使って引用する必要があります。本文には引用番号を出さず、一覧だけに含める場合は `\nocite{...}` を使えます。`\nocite{*}` は現在のデータベースにあるすべての項目を収録するため、慎重に使ってください。引用キーは大文字と小文字を区別し、`.bib` の項目種別に続く波括弧内のキーと完全に一致させます。

サンプルでは、タイトル情報の前に置いた 1 文の注意書きから、現在のプロジェクトリポジトリを最初のオンライン文献として引用しています。これにより、本文中の引用から文末の項目までの流れを一通り確認できます。この用例は [`BIB-REPOSITORY-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 00-sample-notice.tex](../../examples/source/rc-low-pass-filter/sections/00-sample-notice.tex)と[サンプル references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)、結果は PDF 第 1・20 ページです。

## `biblatex-jareport` の適用範囲

中核プリアンブルでは、次の設定を使います。

```tex
\usepackage[
  sorting=none,
  bibstyle=ieee,
  citestyle=numeric-comp
]{biblatex-jareport}
```

実際のソースは 1 行で記述されています。ここでは三つのオプションを読みやすくするために改行しています。それぞれの役割は次のとおりです。

- [`biblatex-jareport.sty`](../../template/biblatex-jareport.sty) は、このプロジェクトが保守するローカルのラッパーです。オプションを BibLaTeX へ渡し、基本スタイルの driver を保存したうえで、厳密に日本語と指定された 11 種類の項目にプロジェクト独自の出力マクロを選びます。
- [`biblatex-jareport-japanese.lbx`](../../template/biblatex-jareport-japanese.lbx) はローカルの日本語言語ファイルです。英語の biblatex 文字列と追加設定を継承し、bibliography／references の二つの見出しだけを「参考文献」にします。完全な日本語ローカライズファイルではありません。
- `bibstyle=ieee` は日本語以外の項目に使う基本の参考文献スタイル、`citestyle=numeric-comp` は連続番号を圧縮する数字引用形式です。`sorting=none` は引用された順序を保ちます。

二つのローカルファイルは、プロジェクトの配布 ZIP にテンプレートと一緒に含まれており、CTAN から別途インストールする依存ファイルではありません。標準のディレクトリ構成では `report.tex` と同じ階層にあります。テンプレートを移動するときに `.sty` または `.lbx` を除くと、ファイルや言語マッピングが見つからないというエラーになります。

このプロジェクトの組版規則は、**SIST 02-2007 を参考にしているだけ**であり、完全準拠を表明または保証するものではありません。

## `langid={japanese}` は厳密に記入する

11 種類のラップされた driver は、すべて同じ厳密な条件で分岐します。フィールドの値が正確に `japanese` の場合にだけ、日本語書式が選ばれます。

```bibtex
langid = {japanese}
```

`langid` を省略した場合、別の言語値を指定した場合、または違う綴りを使った場合、日本語とは自動判定されません。その項目は選択中の基本スタイル、現在は IEEE の driver に戻ります。逆に、英語文献へ誤って `langid={japanese}` を付けると、プロジェクトの日本語書式に入ります。タイトルの文字や著者名から言語を自動判定する機能はありません。

この分岐は [`BIB-JAPANESE-01`](../../examples/FEATURES.md) に対応します。説明のソースは[サンプル 07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)、日本語項目のデータは[サンプル references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)、結果は PDF 第 20～21 ページです。

## 11 種類の日本語 driver と実際のフィールド

プロジェクトは、`article`、`book`、`inbook`、`incollection`、`inproceedings`、`misc`、`online`、`proceedings`、`report`、`thesis`、`unpublished` に日本語分岐を用意しています。下表は、各分岐が実際に出力を試みる主なフィールドと、よくある用途を示したものです。フィールドがない場合、日本語分岐はその項目を省略することも、表に示す別のフィールドへフォールバックすることもあります。Biber が一部のデータ問題を報告する場合はありますが、このプロジェクトの日本語分岐は、授業、学会、SIST の完全性要件を満たしているかどうかを判定しません。

| 種別 | よくある用途 | 日本語分岐が実際に使う主なフィールド |
| --- | --- | --- |
| `article` | 学術誌の論文 | `author`（ない場合は `editor` を試す）、`title`、`journaltitle`、`year`、`volume`、`number`、`pages`、`eid`、`doi`、`url`、`urldate` |
| `book` | 独立した書籍 | `author`／`editor`、`title`、`edition`、`location`、`publisher`、`year`、`pagetotal`（ない場合は `pages` を試す）、`series`、`number`、`isbn`、`url`、`urldate` |
| `inbook` | 書籍の一章または一部分 | `author`、`title`、`booktitle`（必要なら `maintitle` または `title` へフォールバック）、`editor`、`edition`、`location`、`publisher`、`year`、`pages`、`series`、`number`、`isbn`、`url`、`urldate` |
| `incollection` | 編集された論文集の独立した論文 | 日本語の `inbook` と同じプロジェクト書式を使います。代表的なフィールドは `author`、`title`、`booktitle`、`editor`、出版情報、`pages`、`url`、`urldate` です。 |
| `inproceedings` | 会議論文または講演論文 | `author`、`title`、`booktitle`／`eventtitle`、`editor`、`venue`、`eventdate`、`organization`、`publisher`、`year`、`pages`、`series`、`number`、`url`、`urldate` |
| `proceedings` | 会議録全体 | `editor`（ない場合は `author` を試す）、`title`、`venue`、`eventdate`、`organization`、`publisher`、`year`、`pagetotal`、`url`、`urldate` |
| `report` | 技術報告書、機関報告書 | `author`、`title`、`location`、`institution`（必要なら `organization` へフォールバック）、`year`、`number`、`pagetotal`、`url`、`urldate` |
| `thesis` | 学士論文、修士論文、博士論文 | `author`、`title`、`location`、`institution`（必要なら `organization` へフォールバック）、`year`、`pagetotal`、`type`、`url`、`urldate`。`type` がない場合は「学位論文」と表示します。 |
| `online` | Web ページ、オンライン解説、オンラインデータベース | `author`／`editor`、`title`、`organization`（必要なら `publisher` へフォールバック）、`date`、`url`、`urldate` |
| `misc` | 技術資料など、他の種別に分類しにくい資料 | `author`、`title`、`howpublished`、`year`、`note`、`url`、`urldate` |
| `unpublished` | 未刊行原稿、内部資料 | `author`、`title`、`journaltitle`、`year`、`note`、`url`、`urldate` |

`inbook` と `incollection` の日本語分岐は、同じプロジェクトマクロを共用します。ただし、日本語以外の項目は、それぞれ基本スタイル本来の driver に戻ります。`inbook` と `incollection` には `booktitle`、`inproceedings` には `booktitle` または `eventtitle` を記入してください。収録先タイトルがない場合に `title` へ戻る処理は、現在の実装にある最後のフォールバックにすぎません。同じタイトルが 2 回出力される可能性があるため、推奨される記入方法ではありません。

`proceedings` の専用処理を除き、各日本語分岐は `author` がない場合に `editor` を試します。後で収録先の編集者も別に出力する `inbook`、`incollection`、`inproceedings` では、著者がないと同じ編集者が重複して出力される場合があります。会議関係の種別では、`organization` と `publisher` が異なる場合は両方を順に出力し、同じ場合は一度だけ出力します。`organization` がない場合も、`publisher` を一度だけ出力します。対象の書式に合わせてフィールドを記入し、必ずコンパイル結果を確認してください。

表にある `book`、`inbook`、`incollection`、`inproceedings` の `number` は、`series` も存在する場合にだけ表示されます。`urldate` も単独では出力されず、同じ項目に `url` がある場合にだけ表示されます。`online` は完全な `date` を使い、会議関係の種別は `eventdate` も使いますが、その他の日本語 driver の多くは年だけを出力します。総ページ数には `pagetotal`、論文の掲載ページには `pages` を使います。これらは現在の実装の動作であり、すべての biblatex スタイルに共通する規則ではありません。

11 種類の driver の一覧は [`BIB-DRIVERS-01`](../../examples/FEATURES.md) に対応します。各種別のサンプルデータと PDF ページは次のとおりです。

| 種別 | 安定機能 ID | サンプル PDF ページ |
| --- | --- | --- |
| `book` | `BIB-JA-BOOK-01` | 20 |
| `article` | `BIB-JA-ARTICLE-01` | 20 |
| `inbook` | `BIB-JA-INBOOK-01` | 20 |
| `incollection` | `BIB-JA-INCOLLECTION-01` | 20 |
| `inproceedings` | `BIB-JA-INPROCEEDINGS-01` | 20 |
| `proceedings` | `BIB-JA-PROCEEDINGS-01` | 21 |
| `report` | `BIB-JA-REPORT-01` | 20 |
| `thesis` | `BIB-JA-THESIS-01` | 20 |
| `online` | `BIB-JA-ONLINE-01` | 20 |
| `misc` | `BIB-JA-MISC-01` | 20 |
| `unpublished` | `BIB-JA-UNPUBLISHED-01` | 21 |

すべてのソースは[サンプル references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)にあり、組版結果は[サンプル PDF](../../examples/compiled/rc-low-pass-filter.pdf)で確認できます。

## 日本語以外の項目と IEEE へのフォールバック

上記の種別で `langid` が `japanese` ではない場合、ラッパーは読み込み時に保存した基本 driver を呼び出します。上記 11 種類のラップ対象に含まれない項目種別は、最初から基本スタイルをそのまま使います。現在の `bibstyle=ieee` では、サンプルの英語 `article` は日本語の句読点やフィールド順ではなく、IEEE の参考文献形式で出力されます。

英語へのフォールバックは [`BIB-EN-FALLBACK-01`](../../examples/FEATURES.md) に対応します。説明は[サンプル 07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)、データは[サンプル references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)、結果は PDF 第 21 ページです。ここでいう「フォールバック」は、選択した基本スタイルを保つという意味に限られます。テンプレートが言語ごとにローカライズ済みの driver を用意しているという意味ではありません。

## 条件付き出力と引用番号

[`config/preamble.tex`](../../template/config/preamble.tex) は `\AtDataInput` を使います。biblatex が Biber の生成した `.bbl` から少なくとも 1 件の項目を読み込むと、内部状態を真にします。[`sections/07-references.tex`](../../template/sections/07-references.tex) は、その状態の場合にだけ次を実行します。

```tex
\printbibliography[title={参考文献}]
```

このため、空のテンプレートには内容のない「参考文献」見出しが表示されません。引用していない項目を `.bib` に追加しただけでも、参考文献一覧は表示されません。本文の `\cite` または `\nocite` によって、該当項目が現在の参考文献処理に含まれます。この動作は [`BIB-CONDITION-01`](../../examples/FEATURES.md) と [`BIB-PRINT-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)、結果は PDF 第 20～21 ページです。

引用形式は `citestyle=numeric-comp` です。複数の連続した文献番号は範囲へ圧縮され、この動作は [`BIB-COMPRESS-01`](../../examples/FEATURES.md) に対応します。実際の連続引用は[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)で確認できます。`sorting=none` により、番号は最初に引用された順に並びます。URL と参照日の用例は [`BIB-URL-01`](../../examples/FEATURES.md) に対応し、サンプル 07-references.tex の説明と PDF 第 20 ページで確認できます。

## よくあるエラー

### 本文中の引用が未解決になる

PDF に未解決の印が出る場合や、ログに未定義の引用が報告される場合は、次の順に確認します。

1. `\cite{key}` の `key` が `.bib` の項目キーと完全に一致しているか。
2. `.bib` にカンマの不足、波括弧の対応不備、重複キーなどの構文エラーがないか。
3. `report.tex` があるディレクトリで `latexmk -lualatex report.tex` を実行したか。
4. `biber --version` でバージョンが正常に表示されるか。

修正後は Latexmk の完全なコマンドをもう一度実行してください。PDF を削除するだけ、または LuaLaTeX を 1 回だけ実行するだけでは不十分です。

### 日本語文献が IEEE 形式で表示される

項目に `langid={japanese}` が正確に含まれているか確認してください。タイトルと著者が日本語でも、日本語 driver は選ばれません。別の綴りも自動的には正規化されません。

### データはあるのに参考文献見出しが出ない

本文に `\cite{...}` または `\nocite{...}` があるか確認してください。`.bib` にデータがあっても、`\cite` または `\nocite` で現在の参考文献処理へ含めていなければ、条件付き出力は無効のままです。

### 特定のフィールドが表示されない

まず項目種別が正しいか確認し、次に上の driver フィールド表と照合してください。`.bib` にフィールドがあっても、現在の driver が必ず出力するとは限りません。たとえば日本語の `online` は `organization`、`date`、`url`、`urldate` を使いますが、任意の追加フィールドまで表示するわけではありません。

### スタイルまたは言語ファイルが見つからない

`biblatex-jareport.sty`、`biblatex-jareport-japanese.lbx`、`report.tex` が同じ階層にあり、主ファイルだけを移動していないか確認してください。日本語を含む参考文献データは UTF-8 で保存し、従来の BibTeX ではなく Biber で処理します。

---

[← 前へ：数式と相互参照](09-math-and-cross-references.md) | [次へ：日本語組版とフォント →](11-languages-and-fonts.md)
