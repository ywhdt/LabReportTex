# 数式と相互参照

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/09-math-and-cross-references.md)

テンプレートは `mathtools`、`amssymb`、`hyperref`、`cleveref` を初期状態で読み込んでいます。一般的な数式、複数行の導出、数学記号、リンク付き相互参照のために、別のモジュールを有効にする必要はありません。このページでは、テンプレートが実際に採用しているインターフェースと番号付けの方針だけを説明します。

## 一つの数式と複数行の導出

一つの独立した数式には `equation` を使い、環境内に `\label` を置きます。

```tex
\begin{equation}
  Z_C=\frac{1}{j\omega C}
  \label{eq:capacitor}
\end{equation}
```

この用例は [`MATH-EQUATION-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は [PDF 第 3 ページ](../../examples/compiled/rc-low-pass-filter.pdf)です。

複数行の等号位置をそろえる場合は `align` を使い、個別に参照する各行へ `\label` を付けます。

```tex
\begin{align}
  H(j\omega)
    &\coloneqq\frac{V_{\mathrm{out}}}{V_{\mathrm{in}}}
      \label{eq:definition}\\
    &=\frac{1}{1+j\omega RC}.
      \label{eq:transfer}
\end{align}
```

`&` は揃える位置を示し、行末の `\\` で次の行を始めます。完全な 4 行の導出は [`MATH-ALIGN-01`](../../examples/FEATURES.md) に対応します。ソースは同じ[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は PDF 第 4 ページです。振幅、利得、位相を 3 行で示す数式は [`MATH-FREQUENCY-01`](../../examples/FEATURES.md) で、同じく第 4 ページにあります。

## `mathtools` と `amssymb` で実際に使う機能

テンプレートは `mathtools` を通して一般的な AMS 数式機能を利用し、サンプルでは次を実際に使っています。

- `\coloneqq` は「ここで定義する」ことを表します。[`MATH-COLONEQQ-01`](../../examples/FEATURES.md) に対応し、ソースは[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は PDF 第 4 ページです。
- `dcases` は、各行をディスプレイ数式と同じ大きさで組む場合分けに使います。[`MATH-DCASES-01`](../../examples/FEATURES.md) に対応し、同じソースファイルと PDF 第 4 ページで確認できます。
- `\lvert ... \rvert`、`\frac`、`\sqrt` などは、振幅、比、平方根に使います。組み合わせた例は `MATH-FREQUENCY-01` にまとまっています。

場合分けの数式は次のように書けます。

```tex
\begin{equation}
  G(f)\simeq
  \begin{dcases}
    0, & f\ll f_c,\\
    -20\log_{10}\!\left(\frac{f}{f_c}\right), & f\gg f_c.
  \end{dcases}
  \label{eq:asymptotic-gain}
\end{equation}
```

サンプルでは、`amssymb` の `\mathbb` を使い、`s\in\mathbb{C}` や `f\in\mathbb{R}_{>0}` と記述しています。この用例は [`MATH-AMSSYMB-01`](../../examples/FEATURES.md)、ソースは[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は PDF 第 3 ページです。

## ラベルと `\eqref`

ラベル名は PDF に表示されませんが、一意にし、対象の種類が分かる名前にすると管理しやすくなります。プロジェクトのサンプルは次の接頭辞を使います。

- `eq:`：数式。例は `eq:transfer` です。
- `fig:`：図とサブ図。例は `fig:gain-response` です。
- `tab:`：表。例は `tab:equipment` です。
- `sec:`、`subsec:`：セクション。例は `sec:theory` です。
- `lst:`：コードリスト。例は `lst:analysis` です。

接頭辞は LaTeX の必須規則ではなく、Cleveref が出力する対象の種類も決めません。種類は `\label` が置かれた数式、図、表、セクション、コードリストのカウンターで決まります。接頭辞は、ファイルをまたぐ重複を減らし、保守しやすくするためのものです。数式、図表、セクションをコピーしたときは、ラベルも必ず変更してください。同じ `\label` があると参照先が不定になります。図表のラベルは `\caption` の後に置き、`align` では参照する行ごとにラベルを付けます。

日本語の「式」を自動出力せず、数式番号だけを参照する場合は `\eqref` を使えます。

```tex
式\eqref{eq:capacitor}からインピーダンスを求める。
```

`\eqref{eq:capacitor}` は括弧付きの番号を出力します。テンプレートの `showonlyrefs` はこの参照を認識するため、対象の数式には番号が付きます。対象の種類をテンプレートに判定させ、「式」「図」「表」などの名称も出力する場合は、次の `\cref` を使ってください。

## `showonlyrefs`：参照した数式だけに番号を付ける

[`config/preamble.tex`](../../template/config/preamble.tex) では、次を有効にしています。

```tex
\mathtoolsset{showonlyrefs=true}
```

これは AMS 数式環境の番号だけに影響し、セクション、図、表、コードリストには影響しません。`equation` や `align` を使うだけでは、必ず番号が表示されるわけではありません。数式が `\eqref` またはテンプレートが対応する `cleveref` コマンドから参照された場合に番号が付き、未参照の数式は番号なしになります。

サンプルには、`\label` を持ちながら本文から参照されない時定数の式を意図的に置いています。[`MATH-SHOWONLYREFS-01`](../../examples/FEATURES.md) に対応し、ソースは[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)です。PDF 第 5 ページではこの式に番号がなく、近くにある参照済みの式には番号が付いています。

この方針では、導出に `equation` や `align` を使って構造を明確にしながら、本文で実際に扱う式だけを番号付きにできます。

## `\cref` と `\Cref`

テンプレートは `cleveref` に日本語の対象名と区切り方を設定しています。通常は次のように書きます。

```tex
\cref{eq:transfer}に伝達関数を示す。
\cref{fig:gain-response}に測定結果を示す。
\cref{tab:equipment}に使用機器を示す。
```

`\cref` はラベルの種類を判断し、「式」「図」「表」「節」「リスト」などの名前を自動で付けます。テンプレートは `noabbrev` も指定し、省略形を使いません。この動作は [`REF-NAME-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は PDF 第 4 ページです。セクション参照は [`REF-SECTION-01`](../../examples/FEATURES.md)、ソースは[サンプル 01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)、表示結果は PDF 第 3 ページです。

参照表記を文頭に置く場合は、大文字形式の `\Cref` を使えます。

```tex
\Cref{eq:transfer}の絶対値を求める。
```

日本語の対象名には英語のような大文字・小文字の変化がありませんが、`\Cref` は文頭形式のインターフェースとして残しています。他の言語や対象の種類でも、同じインターフェースを使えます。この用例は [`REF-CAPITAL-01`](../../examples/FEATURES.md)、表示結果は PDF 第 4 ページです。

テンプレートは `cleveref` に `nameinlink` を指定しているため、通常参照では対象名と番号の全体がクリック可能なリンクになります。`hyperref` の `hidelinks` はリンクの枠を隠しますが、リンク自体は残ります。この動作は [`LINK-HYPERREF-01`](../../examples/FEATURES.md)、ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、表示結果は PDF 第 9 ページです。

## 範囲、スター付き、複数ラベルの参照

連続する対象には `\crefrange{開始ラベル}{終了ラベル}` を使い、文頭形式には `\Crefrange` を使います。

```tex
\crefrange{eq:definition}{eq:transfer}
\Crefrange{eq:cutoff-min}{eq:cutoff-max}
```

通常の範囲と文頭範囲は [`REF-RANGE-01`](../../examples/FEATURES.md) と [`REF-RANGE-CAPITAL-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は PDF 第 4～5 ページです。サブ図の範囲も同じインターフェースを使い、[`REF-SUBFIG-RANGE-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、表示結果は PDF 第 8 ページです。

参照文字列だけを出力し、ハイパーリンクを作らない場合は、コマンド名の後にスターを付けます。

```tex
\cref*{eq:transfer}
\Cref*{eq:transfer}
\crefrange*{eq:definition}{eq:transfer}
\Crefrange*{eq:definition}{eq:transfer}
```

一つの対象を参照するスター付き形式は [`REF-STAR-01`](../../examples/FEATURES.md)、範囲のスター付き形式は [`REF-RANGE-STAR-01`](../../examples/FEATURES.md) に対応します。ソースはいずれも[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は PDF 第 5 ページです。スターが変えるのはリンクの有無だけで、`showonlyrefs` による「参照済み」の判定は変わりません。これらのコマンドへ直接渡した数式ラベルには、引き続き番号が表示されます。

一つの `\cref` に複数のラベルをカンマ区切りで渡すこともできます。

```tex
\cref{tab:frequency-response,tab:repeatability,fig:gain-residual}
```

テンプレートは `cleveref` に `sort&compress` を指定し、同じ種類の番号を整理しながら、異なる種類をそれぞれの日本語名でまとめます。表と図を混在させた参照は [`REF-MULTI-01`](../../examples/FEATURES.md)、ソースは[サンプル 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、表示結果は PDF 第 15 ページです。連続した範囲の始点と終点が明確な場合は `\crefrange` を使い、不連続または種類が異なる対象には複数ラベルの `\cref` を使ってください。

## テンプレート独自の処理

`mathtools` の `showonlyrefs` は `\eqref` を直接認識できますが、元の `cleveref` コマンドは、すべてのスター付き形式と範囲形式を数式参照として自動登録しません。両者を組み合わせて動作させるため、テンプレートの [`config/preamble.tex`](../../template/config/preamble.tex) は次の二段階で処理します。

1. `\NewCommandCopy` で、`\cref`、`\Cref`、`\crefrange`、`\Crefrange` の元の実装を `labreport@` を含む内部コマンドとして保存します。
2. `\RenewDocumentCommand` で四つの公開コマンドをラップし、最初にラベルを `\noeqref` へ渡してから、元の通常形式またはスター付き形式を呼び出します。

レポート本文では、引き続き公開インターフェースである `\cref` 系のコマンドを使ってください。`\noeqref` や `\labreport@...` の名前を直接使わないでください。

`\noeqref` という名前は誤解しやすいものです。このラッパー内では、Mathtools にラベルを「参照済み」として登録するために使い、番号を消すものではありません。範囲ラッパーが登録するのは、明示的に渡した開始と終了のラベルだけです。その間にあるすべてのラベルまで自動登録することは保証しません。標準の `\ref` や、ここでラップしていない別の Cleveref コマンドについても、同じ `showonlyrefs` 互換動作は保証されません。数式の参照には、このページで示したコマンド群または標準の `\eqref` を優先してください。

プリアンブルを変更するときは、次の点に注意してください。

- テンプレート設定の後でもう一度 `cleveref` を読み込んだり、新しい `\renewcommand` で四つのラッパーを上書きしたりしないでください。`\cref` で参照した数式が、再び番号なしになる可能性があります。
- `hyperref` を `cleveref` より先に読み込む順序を保ってください。テンプレートはすでにこの順序なので、セクションファイルで両者を読み直す必要はありません。
- 日本語の名称や表示形式だけを変更する場合は、`\crefname`、`\Crefname`、`\crefformat`、`\crefrangeformat`、接続語の設定を編集し、ラッパー層は置き換えないでください。
- 数式番号の付与方針を変更する、または別の参照パッケージと統合する場合は、`\cref`、`\Cref`、二つのスター付き形式、`\crefrange`、`\Crefrange`、二つのスター付き範囲形式、種類を混在させた複数ラベル参照をすべて確認してください。

## 日本語の対象名と接続方法

テンプレートは figure、table、equation、section／subsection、subfigure、listing／lstlisting を、それぞれ「図」「表」「式」「節」「図」「リスト」と表示します。一つの図、表、数式、コードリストの明示的な形式は、それぞれ `図<number>`、`表<number>`、`式(<number>)`、`リスト<number>` です。

図とサブ図の範囲区切りは明示的に `〜` と設定しています。二つの項目は「および」でつなぎ、三つ以上では中間を「、」、最後を「および」でつなぎます。ソースで範囲形式を明示的に定義しているのは figure と subfigure だけです。その他の対象種別は、Cleveref が現在の名称と接続語を組み合わせるため、すべての種類がテンプレート独自の書式で処理されるとは限りません。

## ソース・説明・PDF の対応関係

すべての安定 ID は[サンプル機能一覧](../../examples/FEATURES.md)にあり、このページで扱うものは次のとおりです。

| 機能 | 安定 ID | ソース | PDF ページ |
| --- | --- | --- | --- |
| 一つの数式、複数行の整列、一般的な数式機能 | `MATH-EQUATION-01`、`MATH-ALIGN-01`、`MATH-FREQUENCY-01`、`MATH-DCASES-01`、`MATH-COLONEQQ-01`、`MATH-AMSSYMB-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 3～4 |
| 参照した数式だけに番号を付ける | `MATH-SHOWONLYREFS-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| 通常形式、文頭形式、対象名 | `REF-SECTION-01`、`REF-NAME-01`、`REF-CAPITAL-01` | [01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)、[02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 3～4 |
| 範囲と文頭範囲 | `REF-RANGE-01`、`REF-RANGE-CAPITAL-01`、`REF-SUBFIG-RANGE-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 4～5、8 |
| スター付き形式 | `REF-STAR-01`、`REF-RANGE-STAR-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| 種類が異なる複数ラベルの参照 | `REF-MULTI-01` | [04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 15 |
| 枠なしハイパーリンク | `LINK-HYPERREF-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 9 |

---

[← 前へ：図、表、コード、描画](08-figures-tables-and-code.md) | [次へ：参考文献 →](10-bibliography.md)
