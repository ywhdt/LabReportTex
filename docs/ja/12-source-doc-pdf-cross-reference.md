# ソース・説明・PDF の対応関係

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/12-source-doc-pdf-cross-reference.md)

テンプレートでは、同じ機能を三つの場所から確認できます。コピーできるサンプルソース、機能一覧の [`examples/FEATURES.md`](../../examples/FEATURES.md)、そしてコンパイル済みの[サンプル PDF](../../examples/compiled/rc-low-pass-filter.pdf)です。安定した機能 ID が、この三つを結び付けます。

## サンプルソースのマーカー

現在のサンプルソースは [`examples/source/rc-low-pass-filter/`](../../examples/source/rc-low-pass-filter/) にあります。21 ページで構成された、架空の RC ローパスフィルタ実験レポートです。マーカーは、その機能を最もよく表す入口または用例の近くに置かれています。たとえば、次のように記述されています。

```tex
% [DOC:LIST-ITEMIZE-01] Itemized experimental objectives.
\begin{itemize}[leftmargin=2.8em,nosep]
  \item RC 低域通過フィルタの振幅特性と位相特性を測定する。
\end{itemize}
```

通常のコンパイル結果では、関連する表示の近くに色付きの日本語説明を置き、灰色のコード欄に実際に使ったソースを示します。説明はできるだけ簡単な `です・ます` 体にしています。さらに詳しく知りたい場合は、機能 ID からこの説明文書と完全なソースを確認してください。

色付きの説明とコード欄は、[`config/sample-guide.tex`](../../examples/source/rc-low-pass-filter/config/sample-guide.tex) の `\SampleGuideNote` と `\SampleGuideSource` で作成します。サンプル PDF 専用であり、テンプレートの機能ではありません。サンプル `report.tex` の `\showsampleguidetrue` を `\showsampleguidefalse` に変更すると、説明を隠して黒い実験本文だけをコンパイルできます。

## PDF ページと表示結果の一覧

[`examples/FEATURES.md`](../../examples/FEATURES.md) は、すべてのマーカーを七つの機能グループに分けています。下表は各グループの入口です。ここでは代表的な ID だけを示しているため、完全な一覧、各ソースへのリンク、正確なページ番号は機能一覧で確認してください。

| 機能グループ | 代表的な機能 ID | 主なサンプルソース | PDF の範囲 |
| --- | --- | --- | --- |
| 文書の入口とメタデータ | `NOTICE-01`、`DOCMETA-01`、`MODULE-CORE-01`、`META-TITLE-01`、`LAYOUT-HEADER-01`、`CORE-JAPANESE-01`、`COLOR-ANNOTATION-01`、`GUIDE-SOURCE-01`、`GUIDE-TOGGLE-01` | [report.tex](../../examples/source/rc-low-pass-filter/report.tex)、[metadata.tex](../../examples/source/rc-low-pass-filter/config/metadata.tex)、[sample-guide.tex](../../examples/source/rc-low-pass-filter/config/sample-guide.tex)、[00-sample-notice.tex](../../examples/source/rc-low-pass-filter/sections/00-sample-notice.tex) | 1～21 |
| 構成とリスト | `STRUCT-SECTION-01`、`LIST-ITEMIZE-01`、`LIST-ENUMERATE-01`、`LIST-DESCRIPTION-01`、`LIST-BASIC-01` | [01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)、[02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 2～3、6、9～10、19 |
| 数式と相互参照 | `MATH-EQUATION-01`、`MATH-ALIGN-01`、`REF-SECTION-01`、`REF-RANGE-01`、`REF-MULTI-01`、`LINK-HYPERREF-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 3～5、8～9、15 |
| 表、画像、装置図 | `TABLE-TABULARX-01`、`TABLE-MULTIROW-01`、`FIG-IMAGE-01`、`FIG-SUBFIGURE-01`、`DIAG-CIRCUIT-01`、`FLOAT-H-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 6～15 |
| データと PGFPlots | `PLOT-DATAFILE-01`、`PLOT-SINGLE-01`、`PLOT-DUAL-01`、`PLOT-BASE-01` | [04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、[frequency-response.csv](../../examples/source/rc-low-pass-filter/data/frequency-response.csv) | 13～15 |
| コードリスト | `CODE-INLINE-01`、`CODE-BLOCK-01`、`CODE-HIGHLIGHT-01`、`CODE-WRAP-01`、`CODE-REF-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex) | 10、16～17 |
| 参考文献 | `BIB-RESOURCE-01`、`BIB-JAPANESE-01`、`BIB-DRIVERS-01`、`BIB-EN-FALLBACK-01`、`BIB-COMPRESS-01` | [07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)、[references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib) | 20～21。引用は本文にもあります。 |

---

[← 前へ：日本語組版とフォント](11-languages-and-fonts.md) | [次へ：テンプレートのカスタマイズ →](13-customization.md)
