# テンプレートのカスタマイズ

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/13-customization.md)

テンプレートでは、「レポートごとに記入する内容」と「授業の要件が変わったときだけ調整する設定」を別の場所に分けています。通常は `config/metadata.tex`、`sections/`、画像、参考文献を安全に変更できます。ページレイアウト、基本情報表、ヘッダー、フォント、オプションモジュールは `config/` にまとまっています。

## 用紙サイズと余白

[`report.tex`](../../template/report.tex) は `\documentclass` より前に `\DocumentMetadata` を宣言し、その後で `ltjsarticle` 文書クラスを使います。このプロジェクトがサポートする TeX Live 2026 環境では、現在の標準コンパイル結果は A4 用紙です。ページ設定を調整するときも、この二つの命令の順序を入れ替えないでください。四辺の余白は、[`config/preamble.tex`](../../template/config/preamble.tex) の `geometry` で明示的に設定しています。

```tex
\geometry{top=25mm,bottom=30mm,left=25mm,right=25mm,footskip=12mm}
\setlength{\headheight}{30pt}
```

この設定は [`LAYOUT-PAGE-01`](../../examples/FEATURES.md) に対応し、[サンプル PDF 第 1～21 ページ](../../examples/compiled/rc-low-pass-filter.pdf)でページ全体への効果を確認できます。授業の要件が余白の変更だけであれば、各セクションに負のスペースや拡大縮小コマンドを追加せず、まずこの行を変更してください。たとえば、左右の余白を 30 mm にする場合は次のようにします。

```tex
\geometry{top=25mm,bottom=30mm,left=30mm,right=30mm,footskip=12mm}
```

授業で用紙サイズも指定されている場合は、文書クラスの用紙オプション、印刷設定、最終 PDF のページサイズをあわせて確認します。余白の数字だけを変えても、用紙サイズは変更できません。テンプレートは A4 を標準としているため、別の用紙サイズへ変更した後は、少なくとも基本情報表、ヘッダー、幅の広い表、横並びのサブ図、PGFPlots のグラフを確認してください。

`\headheight` は、複数列のヘッダーに必要な高さを確保します。ヘッダーの内容を増やしたり、文字を大きくしたり、改行を追加したりした後に、ログへ `\headheight is too small` と出た場合は、実際のヘッダーの高さに合わせて値を増やしてください。この警告を無視しないでください。上下の余白を変えた場合は、本文がヘッダー、中央のページ番号、用紙の端と重なっていないことも確認します。

## 基本情報表、ヘッダー、フッター

テンプレートには、1 ページ全体を使う独立したタイトルページはありません。`report.tex` で呼び出す `\maketitle` は [`config/commands.tex`](../../template/config/commands.tex) で再定義され、1 ページ目の本文より前に「レポートの基本情報」表を作ります。本文はそのまま同じページに続きます。この表示は [`TITLEBLOCK-01`](../../examples/FEATURES.md) に対応し、サンプル PDF 第 1 ページで確認できます。

基本情報表は `tabularx`、ヘッダーは `fancyhdr` を使います。どちらも `commands.tex` で定義されていますが、必要なパッケージは `config/preamble.tex` が先に読み込みます。そのため、`report.tex` では次の順序を維持してください。

```tex
\input{config/preamble}
\input{config/commands}
\input{config/metadata}
```

`commands.tex` は `metadata.tex` より先に読み込む必要があります。逆にすると、`\course`、`\group` などの情報コマンドがまだ定義されていません。また、`preamble.tex` は `commands.tex` より先でなければ、`tabularx` や `fancyhdr` などのインターフェースを利用できません。各レポートの値は、引き続き `metadata.tex` に記入します。氏名、学籍番号、日付を組版定義へ直接書き込まないでください。

自分のレポートだけに基本情報のフィールドを追加する場合は、`commands.tex` に値を受け取るコマンドと空の既定値を定義し、`metadata.tex` でそのコマンドに値を記入してから、`\maketitle` またはヘッダーに表示位置を用意します。表の中へ固定文字列を直接書くだけでは、レポートごとに記入できるフィールドにはなりません。新しいコマンドには、テンプレート内部用の `labreport@` または `@` を含む名前を使わないでください。

基本情報表のフィールド位置は、`\renewcommand{\maketitle}{...}` の中で調整できます。ヘッダーの各列は `\fancyhead[C]{...}` の中にあり、現在のフッターは中央のページ番号だけです。

```tex
\fancyfoot[C]{\thepage}
```

ヘッダーには、幅を固定した 4 列と、残りの幅を使う `X` 列があります。列数や幅を変える場合は、合計が `\textwidth` を超えないようにし、短い値と実際の上限に近い長い値の両方でコンパイルしてください。ヘッダーとページ番号全体の表示は [`LAYOUT-HEADER-01`](../../examples/FEATURES.md)、各フィールドの対応関係は [`META-HEADER-01`](../../examples/FEATURES.md) で確認できます。

授業で独立したタイトルページが本当に必要な場合は、新しい文書構造として設計してください。ヘッダーを表示するか、ページ番号に含めるか、本文を何ページ目から始めるかも明確にする必要があります。現在の基本情報表を、すでに用意されたタイトルページ用のインターフェースだと考えないでください。

## フォントと文字サイズ

日本語フォント、欧文フォント、数学フォントは別々の仕組みで管理されます。現在の日本語フォントは、`config/preamble.tex` の `luatexja-preset` で設定しています。

```tex
\usepackage[deluxe]{luatexja-preset}
```

プリセットを変える場合は、この 1 行を置き換え、パッケージを重ねて読み込まないでください。利用できるプリセット、フォントファイルの確認、中国語組版がサポート対象外であることなど、設定の境界は[日本語組版とフォント](11-languages-and-fonts.md)で説明しています。混植の表示は [`CORE-JAPANESE-01`](../../examples/FEATURES.md) に対応します。

テンプレートは、文書クラスのオプションで全体の文字サイズを明示的に固定していません。現在の `ltjsarticle` の標準本文は 10 pt です。基本情報表の見出しは `\Large`、ヘッダーは `\small`、コードリストは `config/listings.tex` で `\fontsize{8}{10}\selectfont` を使います。このため、「本文の文字サイズ」「ヘッダーの文字サイズ」「コードの文字サイズ」を変える作業は、それぞれ別の変更です。局所的に強調するだけなら、本文で絶対的な文字サイズを何度も指定せず、限られた範囲に `\small`、`\large` などの相対サイズを使ってください。

授業で全体の文字サイズが指定されている場合は、文書クラスのオプションを変更し、ページ数、フロート位置、表の折り返し、図表のキャプション、基本情報表、ヘッダーを再確認します。日本語フォントのプリセットは、欧文フォントと数学フォントを自動的に同期して置き換えません。三種類すべてに明確な指定がある場合は、個別に設定し、PDF に埋め込まれたフォントを確認してください。

## 図表の配色とスタイル

中核プリアンブルは初期状態で `xcolor` を読み込みます。ただし、サンプル PDF の色付き説明はサンプル内で局所的に使う注釈であり、一般のレポートに残すことを求めるテンプレート環境ではありません。この表示は [`COLOR-ANNOTATION-01`](../../examples/FEATURES.md) に対応します。

図表に長期的に適用する設定は、用途ごとの設定ファイルにあります。

- [`config/listings.tex`](../../template/config/listings.tex)：コードの文字サイズ、枠線、行番号、キーワード、コメント、文字列の色を設定します。
- [`config/plots.tex`](../../template/config/plots.tex)：`plotLine` の色と、`report axis`、`report plot`、`report legend`、`report dual right axis` などの PGFPlots スタイルを設定します。
- [`config/diagrams.tex`](../../template/config/diagrams.tex)：TikZ、CircuitikZ、`mdframed` の読み込みと互換処理を行います。個々の図形の色は、通常、本文中の描画オプションで指定します。

コードモジュールと描画モジュールは初期状態で無効です。[図、表、コード、描画](08-figures-tables-and-code.md)の手順で必要なモジュールを有効にしてから、そのスタイルファイルを変更してください。`config/plots.tex` の共通スタイルを変えた場合は、単一縦軸、二重縦軸、凡例なしのグラフをまとめて確認します。サンプルの対応項目は [`PLOT-STYLE-AXIS-01`](../../examples/FEATURES.md)、[`PLOT-DUAL-01`](../../examples/FEATURES.md)、[`PLOT-BASE-01`](../../examples/FEATURES.md) です。

データ系列を色だけで区別しないでください。提出前に、グレースケール印刷、線種、点の形、凡例、文字とのコントラストも確認することを勧めます。テンプレートは授業指定の色、色覚による識別のしやすさ、プリンタの出力を自動的には検証しません。最終 PDF を目視で確認してください。

## 設定ファイルを追加で分割する

複数の場所で使う一連の設定がある場合、または既存の設定ファイルが読みにくくなった場合にだけ、新しいファイルへ分割します。パッケージの読み込み命令は、必ず `\begin{document}` より前に置きます。オプション機能を追加する場合は、既存モジュールにならって `config/course-style.tex` を作り、`config/preamble.tex` から読み込めます。

```tex
% Optional course-specific styles.
% \input{config/course-style}
```

このファイルには、その機能に必要な `\usepackage`、色、スタイル定義を置けます。有効にした後は、レポートと一緒にファイルを保存してください。Release テンプレートが、利用者の追加した設定を自動生成することはありません。

新しいファイルで基本情報表、ヘッダー、情報コマンドを上書きする場合は、`config/commands.tex` の後、`config/metadata.tex` の前に読み込みます。

```tex
\input{config/preamble}
\input{config/commands}
\input{config/course-overrides}
\input{config/metadata}
```

新しいファイルが `metadata.tex` から呼び出す新しいフィールドコマンドを定義する場合も、この順序を保ちます。反対に、本文だけで使う内容はプリアンブルへ置かず、`sections/` または別の本文ファイルに置いてください。

[`biblatex-jareport.sty`](../../template/biblatex-jareport.sty) と [`biblatex-jareport-japanese.lbx`](../../template/biblatex-jareport-japanese.lbx) を分けたり、`\usepackage` をセクションファイルへ置いたりしないでください。`config/diagrams.tex` には、TeX Live 2026 のリスト動作に合わせた `mdframed` の互換処理もあります。装置図モジュールを有効にするときは、三つのパッケージ読み込み行だけをコピーせず、ファイル全体を残してください。

## 更新時の互換性を保つ

テンプレートは利用者の変更を自動的にマージせず、自動更新コマンドもありません。Release ZIP 内の `VERSION` は、ダウンロードしたパッケージのバージョンを示すだけです。開発ツリーの `template/` 自体には、このファイルはありません。更新する前に現在のコンパイル可能なディレクトリを保存し、自分の変更を次の三種類に分けてください。

1. `metadata.tex`、各セクション、画像、`.bib` データは、新しいディレクトリの同じ位置へ移せます。
2. ページ余白、フォントプリセット、モジュールのスタイルなどの設定は、新しいバージョンと一項目ずつ比較してから適用し直します。
3. `commands.tex`、`report.tex`、ローカルの参考文献スタイル、`cleveref` のラッパーは結合度が高いため、古いファイル全体で新しいファイルを上書きしないでください。

特に、[`config/preamble.tex`](../../template/config/preamble.tex) にある `showonlyrefs` と `cleveref` の互換ラッパーを上書きせず、`hyperref` と `cleveref` の読み込み順序も変更しないでください。関連する注意点は[数式と相互参照](09-math-and-cross-references.md)で説明しています。

移行後は、`report.tex` が見えるディレクトリで完全なコンパイルを実行し、ログ、PDF の各ページ、引用、フォント、図表、参考文献を確認します。

---

[← 前へ：ソース・説明・PDF の対応関係](12-source-doc-pdf-cross-reference.md) | [次へ：よくある質問とトラブルシューティング →](14-troubleshooting.md)
