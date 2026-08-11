# 本文とセクション構成

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/06-document-structure.md)

## メインファイル `report.tex`

[`report.tex`](../../template/report.tex) は、テンプレートの設定、レポート情報、本文を組み立てるメインファイルです。

次のコードでは本文の構成に関係する行だけを示しています。完全なファイルには、ほかに `\DocumentMetadata`、文書クラス、参考文献データベース、画像パスの設定があります。

```tex
\input{config/preamble}
\input{config/commands}
\input{config/metadata}

\begin{document}

\maketitle

\input{sections/01-purpose}
\input{sections/02-theory}
\input{sections/03-method}
\input{sections/04-results}
\input{sections/05-discussion}
\input{sections/06-summary}
\input{sections/07-references}

\end{document}
```

## 既定のセクション

既定の構成は、一般的な日本語の実験レポートに対応しています。

| ファイル | 既定の見出し | 主な目的 |
| --- | --- | --- |
| [`01-purpose.tex`](../../template/sections/01-purpose.tex) | `目的` | 実験で確認する対象と目標を示します。 |
| [`02-theory.tex`](../../template/sections/02-theory.tex) | `原理` | 実験に関わる式、モデル、定義を整理します。 |
| [`03-method.tex`](../../template/sections/03-method.tex) | `実験方法` | 装置、条件、操作手順を記録します。 |
| [`04-results.tex`](../../template/sections/04-results.tex) | `実験結果` | 測定結果、表、図を配置します。 |
| [`05-discussion.tex`](../../template/sections/05-discussion.tex) | `考察` | 結果、誤差、現象を分析します。 |
| [`06-summary.tex`](../../template/sections/06-summary.tex) | `まとめ` | 実験で得た結果を簡潔にまとめます。 |
| [`07-references.tex`](../../template/sections/07-references.tex) | 条件付きで生成される `参考文献` | 引用した文献がある場合に文献一覧を出力します。 |

これらの名称は出発点となる構成です。必要に応じて、`\section{...}` の見出しを変えたり、セクションを追加、削除、並べ替えたりできます。

最初の 6 つの本文ファイルには、直接 `\section` が入っています。`07-references.tex` には `\section` 命令が直接書かれていません。条件が満たされたとき、`\printbibliography` が「参考文献」の見出しを生成します。

## セクションの本文を書く

最初の 6 つの本文ファイルにはすでに `\section` があるため、その次の行から本文を書けます。

```tex
\section{目的}

本実験では，RC低域通過フィルタの周波数特性を測定し，
遮断周波数付近における利得と位相差の変化を確認する。
```

本文中では `\subsection` も使用できます。相互参照が必要な場合は、セクション、数式、図、表に `\label` を付け、テンプレートで設定済みの `\cref` を使います。詳細は[数式と相互参照](09-math-and-cross-references.md)を参照してください。

## セクションを追加する

たとえば、まとめの後に付録を加える場合は、`sections/appendix.tex` を作成します。

```tex
\section{付録}

ここに追加の測定結果を示す。
```

次に、`report.tex` で出力位置を決めます。

```tex
\input{sections/06-summary}
\input{sections/appendix}
\input{sections/07-references}
```

## セクションを削除・並べ替える

一時的に不要なセクションがあるときは、対応する `\input` 行を削除するか、コメントアウトできます。

```tex
% \input{sections/06-summary}
```

本文ファイルは残しておけば、後から簡単に戻せます。セクションの順序を変えるには、`report.tex` 内の `\input` 行を移動するだけです。

## 目次と改ページ

このテンプレートは比較的短い実験レポートを想定しています。初期状態では目次を作らず、セクション間にも `\clearpage` や `\newpage` などの強制改ページコマンドを入れていません。`\maketitle` は独立した表紙を作らず、現在のページに基本情報表だけを出力します。本文はそのまま同じページから続き、後続のセクションも残りのスペースに自然につながります。スペースが不足した場合は、LaTeX が通常どおり改ページします。

必要であれば、対応するコマンドを自分で追加できます。

## 参考文献

`sections/07-references.tex` には条件判定があります。有効な文献データが Biber によって読み込まれたときにだけ `\printbibliography` が実行されます。通常は `\cite` または `\nocite` によって読み込まれるため、未引用の項目を `.bib` ファイルへ追加するだけでは文献一覧は表示されず、空テンプレートにも内容のない「参考文献」の見出しは出ません。

参考文献を使うときは、次をすべて行います。

1. `bibliography/references.bib` に項目を追加します。
2. 本文で `\cite{...}` を使ってその項目を引用します。本文では引用せず一覧には載せたい場合は、`\nocite{...}` を使えます。
3. `latexmk -lualatex report.tex` を実行し、Biber の呼び出しと必要な回数の再コンパイルを Latexmk に任せます。

参考文献データとプロジェクトのスタイルは、[参考文献](10-bibliography.md)で詳しく説明します。

## 完全なサンプルとの対応

[サンプルの report.tex](../../examples/source/rc-low-pass-filter/report.tex) も同じセクション分割を使っています。[サンプル機能一覧](../../examples/FEATURES.md)の `STRUCT-SECTION-01`、`TITLEBLOCK-01`、`BIB-CONDITION-01` からソースと PDF のページ番号を見つけ、[コンパイル済みのサンプル PDF](../../examples/compiled/rc-low-pass-filter.pdf)と照らし合わせてください。

---

[← 前へ：レポートの基本情報](05-metadata.md) | [次へ：独自コマンドと環境 →](07-commands-and-environments.md)
