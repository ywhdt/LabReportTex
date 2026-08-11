# 独自コマンドと環境

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/07-commands-and-environments.md)

このページでは、テンプレートが直接定義するコマンド、利用できるリスト環境、`config/` が提供する公開インターフェースを説明します。図やグラフ、相互参照の具体的な書き方は、[図、表、コード、描画](08-figures-tables-and-code.md)と[数式と相互参照](09-math-and-cross-references.md)で扱います。

## レポート情報コマンドの定義場所

各レポートの値は [`config/metadata.tex`](../../template/config/metadata.tex) に記入し、コマンドの定義と表示方法は [`config/commands.tex`](../../template/config/commands.tex) に置かれています。役割の分担は次のとおりです。

- `\title` と `\author` は文書クラスにもともとあるコマンドです。テンプレートでは、1 ページ目の基本情報表を作り、内容を PDF の Title 属性と Author 属性へ保存するために使います。
- `\course`、`\group`、`\theme`、`\studentid`、`\experimentpartner`、`\staff`、`\experimentdate`、`\place`、`\experimenttime`、`\reportfirstdue` は、テンプレートが定義する公開コマンドです。
- `\maketitle` は本文から呼び出す公開コマンドですが、文書クラス本来のタイトル形式ではなく、「レポートの基本情報」を示す表を作るように再定義されています。

通常の利用者は、`metadata.tex` の波括弧内にある値だけを変更してください。各項目の表示場所、`\group{A}` が自動的に `A班` になること、PDF メタデータについては、[レポートの基本情報](05-metadata.md)でまとめています。

これらは引数を一つ取るコマンドで、内容、長さ、書式を検査しません。同じコマンドを複数回記入すると、最後に呼び出した値で上書きされます。標準の `\date` はテンプレートの基本情報表に含まれていないため、記入しても自動表示されません。`\maketitle` は幅 `0.95\linewidth` の基本情報表だけを出力し、独立した表紙や強制改ページは作りません。ヘッダーには固定幅の `tabularx` で科目、テーマ、学籍番号、班、作成者を順に配置し、フッター中央にはページ番号を表示します。内容が長い場合は、改行とヘッダーの高さをコンパイル後に確認してください。

サンプルでは、コマンド定義の読み込み箇所を [`MODULE-COMMANDS-01`](../../examples/FEATURES.md) として示しています。ソースは[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)、表示結果は [PDF 第 1～21 ページ](../../examples/compiled/rc-low-pass-filter.pdf)で確認できます。1 ページ目の基本情報表は [`TITLEBLOCK-01`](../../examples/FEATURES.md) と `META-TABLE-01`、ヘッダーは `META-HEADER-01` と `LAYOUT-HEADER-01` に対応します。各項目のソースは[サンプル metadata.tex](../../examples/source/rc-low-pass-filter/config/metadata.tex)です。[サンプル機能一覧](../../examples/FEATURES.md)には、`META-TITLE-01`、`META-AUTHOR-01`、`META-COURSE-01`、`META-GROUP-01`、`META-THEME-01`、`META-STUDENT-01`、`META-PARTNER-01`、`META-STAFF-01`、`META-DATE-01`、`META-PLACE-01`、`META-TIME-01`、`META-DUE-01` もあり、各ソースと PDF 第 1 ページを対応させています。

## TeX Live 2026 のリスト記法

テンプレートの [`report.tex`](../../template/report.tex) は、先頭で次の宣言を使います。

```tex
\DocumentMetadata{lang=ja-JP,tagging=off}
```

この宣言は、文書言語を日本語に設定して Tagged PDF を無効にするだけでなく、TeX Live 2026 の新しいリストキーも有効にします。テンプレートは `enumitem` を直接読み込んでいません。リストのオプションには、現在のテンプレートとサンプルでコンパイルを確認済みの書き方を使い、`enumitem` に依存する構文全体をそのまま持ち込まないでください。

`\DocumentMetadata` は `\documentclass` より前に置く必要があります。文書クラスより後ろへ移動した構成は、テンプレートの検証対象外です。

### 順序のない `itemize`

実験の目的や条件に順序がない場合は、`itemize` を使えます。

```tex
\begin{itemize}[leftmargin=2.8em,nosep]
  \item RC 回路の振幅特性を測定する。
  \item 理論値と測定値を比較する。
\end{itemize}
```

`leftmargin=2.8em` は左余白を明示し、`nosep` はリストの前後と項目間にある余分な縦方向の空きを抑えます。項目間に少し間隔を残す場合は、`nosep` の代わりに `itemsep=0.25em` を指定できます。完全な用例は[サンプル 01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)にあり、機能 ID は [`LIST-ITEMIZE-01`](../../examples/FEATURES.md)、[`LIST-KEY-LEFTMARGIN-01`](../../examples/FEATURES.md)、[`LIST-KEY-NOSEP-01`](../../examples/FEATURES.md)、表示結果は [PDF 第 2～3 ページ](../../examples/compiled/rc-low-pass-filter.pdf)です。同じファイルで項目間隔を広げた別の書き方は `LIST-ITEMIZE-02`、測定条件とまとめのリストはそれぞれ `LIST-ITEMIZE-03`（PDF 第 9 ページ）と `LIST-ITEMIZE-04`（PDF 第 19 ページ）に対応します。

### 順序のある `enumerate`

手順に明確な順序がある場合は `enumerate` を使います。次の `label` は番号を `(1)` 形式にします。

```tex
\begin{enumerate}[label=(\arabic*),leftmargin=3em,itemsep=0.25em]
  \item 回路を構成する。
  \item 入力波形を設定する。
  \item 測定値を記録する。
\end{enumerate}
```

通常の `1.` 形式にする場合は、`label=\arabic*.` と書きます。二つの形式は [`LIST-ENUMERATE-01`](../../examples/FEATURES.md) と [`LIST-ENUMERATE-02`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)と[サンプル 05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex)、表示結果は PDF 第 10 ページと第 19 ページです。

### 用語を説明する `description`

記号、フィールド、誤差要因のように「名前と説明」を対応させる場合は、`description` を使います。

```tex
\begin{description}[leftmargin=8em,labelwidth=6.5em,nosep]
  \item[$R$] 抵抗値
  \item[$C$] 静電容量
  \item[$f_c$] 遮断周波数
\end{description}
```

`labelwidth` はラベル部分の幅、`leftmargin` は説明本文の開始位置を調整します。ラベルが長い場合は、この二つを明示的に大きくし、PDF で重なりがないかを確認してください。記号と周波数領域の用例は [`LIST-DESCRIPTION-01`](../../examples/FEATURES.md) と [`LIST-DESCRIPTION-02`](../../examples/FEATURES.md) です。ソースは[サンプル 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、表示結果は PDF 第 6 ページです。CSV フィールドと誤差要因は、それぞれ `LIST-DESCRIPTION-03`（PDF 第 10 ページ）と `LIST-DESCRIPTION-04`（PDF 第 19 ページ）に対応し、[サンプル機能一覧](../../examples/FEATURES.md)から位置を確認できます。

### 基本の `list`

ラベルと長さをすべて自分で決める場合は、LaTeX の基本 `list` 環境も使えます。上記の角括弧によるキーではなく、第 2 引数の中で長さを設定します。

```tex
\begin{list}{--}{%
  \setlength{\leftmargin}{2.8em}%
  \setlength{\itemsep}{0.15em}%
  \setlength{\parsep}{0pt}%
}
  \item 信号発生器
  \item オシロスコープ
\end{list}
```

この用例は [`LIST-BASIC-01`](../../examples/FEATURES.md) です。ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、表示結果は PDF 第 6 ページです。

新しいリストキーを使う際は、次の点に注意してください。

- `2.8em`、`3em`、`8em` のように明示した長さを使い、別のリスト実装に由来する `leftmargin=*` などの特殊値は使いません。
- `diagrams` モジュールでは、TeX Live 2026 のリスト実装と `mdframed` が内部で使う旧式リストとの間で、局所的な互換処理を行います。利用者は通常どおり `mdframed` を使い、互換処理で使われる内部コマンドを直接呼び出さないでください。

リストキーを有効にする `\DocumentMetadata` の例は [`DOCMETA-01`](../../examples/FEATURES.md) です。ソースは[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)、説明は PDF 第 1 ページにあります。

## `config/` の公開インターフェースと内部実装

クリーンなテンプレートでは、設定を次の層に分けています。

| ファイル | 利用者向けの公開インターフェース | 公開インターフェースとして呼び出さない内容 |
| --- | --- | --- |
| [`config/metadata.tex`](../../template/config/metadata.tex) | 各レポート情報コマンドの引数 | 内部の組版ロジックはありません |
| [`config/commands.tex`](../../template/config/commands.tex) | メタデータ用コマンドと本文中の `\maketitle` | `\@course`、`\@group` など `@` を含む保存用コマンド、基本情報表とヘッダーの実装詳細 |
| [`config/preamble.tex`](../../template/config/preamble.tex) | 中核パッケージ、ページレイアウト、相互参照の動作、三つのオプションモジュールを読み込む設定 | `\labreport@cref` などの互換ラッパー、参考文献状態フラグ、接続表現の実装 |
| [`config/diagrams.tex`](../../template/config/diagrams.tex) | TikZ、CircuitikZ、`mdframed` と読み込み済み TikZ ライブラリの有効化 | `\labreport@legacyitem` と `\mdf@trivlist` のパッチ |
| [`config/listings.tex`](../../template/config/listings.tex) | `listings` モジュールと全体の `\lstset` スタイル | テンプレート独自のコマンドや環境は追加していません |
| [`config/plots.tex`](../../template/config/plots.tex) | `report axis base`、`report legend`、`report axis`、`report plot`、`report dual right axis` | PGFPlots 自体の内部実装 |

中核プリアンブルの読み込み箇所は [`MODULE-CORE-01`](../../examples/FEATURES.md) です。ソースは[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)で、PDF 第 1～21 ページ全体に影響します。ページレイアウトとヘッダー／フッターは、それぞれ [`LAYOUT-PAGE-01`](../../examples/FEATURES.md) と [`LAYOUT-HEADER-01`](../../examples/FEATURES.md) に対応し、同じ `report.tex` にマーカーがあります。

## オプションモジュール

TikZ／CircuitikZ／`mdframed`、`listings`、PGFPlots は、次の三つの `\input` 行でそれぞれ有効にします。

```tex
\input{config/diagrams}
\input{config/listings}
\input{config/plots}
```

クリーンなテンプレートの [`config/preamble.tex`](../../template/config/preamble.tex) には、コメントアウトされた `\input` 行があらかじめあります。使うものだけコメントを外してください。三つのモジュールは [`MODULE-DIAGRAMS-01`](../../examples/FEATURES.md)、[`MODULE-LISTINGS-01`](../../examples/FEATURES.md)、[`MODULE-PLOTS-01`](../../examples/FEATURES.md) に対応します。各モジュールの読み込み箇所は[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)、表示結果は順に PDF 第 8～9 ページ、第 10・16～17 ページ、第 13～15 ページです。

三つのモジュールをまとめて有効にする例は [`MODULES-01`](../../examples/FEATURES.md) に対応します。サンプル PDF 第 1 ページの説明と、後続ページにある各モジュールの実例を見比べられます。

このプロジェクトは、中国語向けの環境を含め、独自の本文環境を定義していません。授業専用のコマンドや別のパッケージを追加する場合は、自分用の派生テンプレートとして扱い、`@` や `labreport@` の接頭辞を避け、完全にコンパイルして既存コマンドを上書きしていないか確認してください。より体系的な変更方法は、[テンプレートのカスタマイズ](13-customization.md)で説明します。

---

[← 前へ：本文とセクション構成](06-document-structure.md) | [次へ：図、表、コード、描画 →](08-figures-tables-and-code.md)
