# 図、表、コード、描画

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/08-figures-tables-and-code.md)

テンプレートは、外部画像、フロート、サブ図、一般的な表に必要な中核パッケージを初期状態で読み込んでいます。TikZ／CircuitikZ／`mdframed`、コードリスト、PGFPlots は比較的規模が大きいため、三つのオプションモジュールに分かれています。実際に使うモジュールだけを有効にすると、設定を簡潔に保ち、エラーの原因も絞りやすくなります。

## モジュールを有効にする必要があるか確認する

外部画像と表を使うだけなら、追加モジュールを有効にする必要はありません。装置図、コード、データグラフを使う場合は、[`config/preamble.tex`](../../template/config/preamble.tex) を開き、対応する行のコメントを外します。

```tex
\input{config/diagrams} % TikZ、CircuitikZ、mdframed
\input{config/listings} % listings
\input{config/plots}    % PGFPlots
```

三つの行は個別に有効にでき、同時に使う必要はありません。サンプルでまとめて有効にする方法は [`MODULES-01`](../../examples/FEATURES.md)、個別の有効化箇所は [`MODULE-DIAGRAMS-01`](../../examples/FEATURES.md)、[`MODULE-LISTINGS-01`](../../examples/FEATURES.md)、[`MODULE-PLOTS-01`](../../examples/FEATURES.md) に対応します。対応する `\input` 行は[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)にあり、表示結果は順にサンプル PDF 第 8～9 ページ、第 10・16～17 ページ、第 13～15 ページです。

## 外部画像

PNG、JPG、PDF の画像を `figures/` に置きます。ルートファイルでは `\graphicspath{{figures/}}` を設定済みなので、本文では通常ファイル名だけを指定します。

```tex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.82\linewidth]{oscilloscope-waveform.png}
  \caption{遮断周波数付近で観測した波形}
  \label{fig:oscilloscope-waveform}
\end{figure}
```

`width=0.82\linewidth` は、現在の本文幅の 82% に画像幅を設定します。特定のコンピュータにある絶対パスではなく、本文幅を基準に大きさを決める方法を勧めます。`\caption` の後に `\label` を置けば、`\cref{fig:oscilloscope-waveform}` で正しい図番号を参照できます。

完全な用例は [`GRAPHICSPATH-01`](../../examples/FEATURES.md) と [`FIG-IMAGE-01`](../../examples/FEATURES.md) に対応します。パス設定は[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)、画像の挿入は[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、元画像は[oscilloscope-waveform.png](../../examples/source/rc-low-pass-filter/figures/oscilloscope-waveform.png)、表示結果は [PDF 第 9 ページ](../../examples/compiled/rc-low-pass-filter.pdf)です。`\graphicspath` は検索場所を設定するだけで、画像をコピーしません。レポートを提出または移動するときも、実際のファイルを残してください。

### フロート位置とサブ図

通常は `[htbp]` を優先し、現在位置、ページ上部、ページ下部、フロート専用ページの中から LaTeX に適切な場所を選ばせます。テンプレートが読み込む `float` は、現在位置に固定する `[H]` も提供します。`[H]` を多用するとページ末尾に大きな空白が残ることがあるため、PDF の改ページを確認してください。この動作は [`FLOAT-H-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex)にあり、表示結果は PDF 第 7～9・11～15・17 ページです。

二つの図を横に並べ、個別の番号を付ける場合は、読み込み済みの `subcaption` を使えます。

```tex
\begin{figure}[H]
  \centering
  \begin{subfigure}[t]{0.47\linewidth}
    \centering
    \includegraphics[width=\linewidth]{input-waveform.png}
    \caption{入力波形}
    \label{fig:input-waveform}
  \end{subfigure}
  \hfill
  \begin{subfigure}[t]{0.47\linewidth}
    \centering
    \includegraphics[width=\linewidth]{output-waveform.png}
    \caption{出力波形}
    \label{fig:output-waveform}
  \end{subfigure}
  \caption{入力波形と出力波形}
  \label{fig:waveforms}
\end{figure}
```

二つのファイル名は、自分で `figures/` に置いた画像の名前へ変更してください。この構成は、サンプルにある同じ `subfigure` の使い方をもとにしています。回路図と装置図を並べた完全な用例は [`FIG-SUBFIGURE-01`](../../examples/FEATURES.md)、ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、表示結果は PDF 第 8 ページです。サブ図の範囲参照 `REF-SUBFIG-RANGE-01` は次のページで説明します。

テンプレートは `subcaption` による `subfigure` 環境を使い、サブ図番号を `(a)`、`(b)` 形式にそろえています。古い `subfigure` パッケージの環境ではありません。二つのサブ図の幅と中間の間隔の合計は、親の `\linewidth` を超えないようにしてください。また、各サブ図と親図には異なるラベルが必要です。

## 表

短い数値表には標準の `tabular` を使えます。説明が長い場合は、`tabularx` の `X` 列で残り幅を自動配分する方法が便利です。次の基本構造を直接変更できます。

```tex
\begin{table}[htbp]
  \centering
  \caption{測定条件}
  \label{tab:conditions}
  \begin{tabularx}{\linewidth}{lX}
    \toprule
    項目 & 条件 \\
    \midrule
    入力波形 & 正弦波、振幅 $1.00\,\mathrm{V}$ \\
    周波数 & $100\,\mathrm{Hz}$ から $10\,\mathrm{kHz}$ \\
    \bottomrule
  \end{tabularx}
\end{table}
```

- `X` 列は表の残り幅を使い、内容を自動的に折り返します。[`TABLE-TABULARX-01`](../../examples/FEATURES.md) に対応し、サンプル PDF 第 6～7 ページで確認できます。
- `p{0.20\linewidth}` は幅を固定し、折り返し可能な列を作ります。[`TABLE-PCOLUMN-01`](../../examples/FEATURES.md) に対応し、表示結果は PDF 第 7 ページです。
- `booktabs` の `\toprule`、`\midrule`、`\bottomrule` を使うと、表の横罫線を整理できます。[`TABLE-BOOKTABS-01`](../../examples/FEATURES.md) に対応し、表示結果は PDF 第 7 ページです。通常は多数の縦罫線を追加する必要はありません。
- 列数が決まっていて内容が短い場合は、標準の `tabular` の方が簡単です。数値比較の用例は [`TABLE-REPEAT-01`](../../examples/FEATURES.md)、ソースは[サンプル 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、表示結果は PDF 第 12 ページです。

`X` 列または `p` 列を左揃えにするため、サンプルでは `>{\raggedright\arraybackslash}X` を使っています。`\arraybackslash` は段落の揃え方を変えた後に、表の行末にある `\\` の働きを戻します。`\raggedright` だけをコピーして省略しないでください。表が広すぎる場合は、サンプルと同様に局所的に `\tabcolsep` を小さくする方法もあります。これは列間の空白だけを変え、内容そのものを自動圧縮するものではありません。

同じ分類名を複数行にまたがって表示する場合は、読み込み済みの `multirow` を使います。簡潔な三線表にする場合は、前述の `booktabs` と組み合わせます。

```tex
\begin{tabular}{lll}
  \toprule
  領域 & 周波数 & 利得 \\
  \midrule
  \multirow{2}{*}{通過域}
    & $100\,\mathrm{Hz}$ & $0.00\,\mathrm{dB}$ \\
    & $500\,\mathrm{Hz}$ & $-0.35\,\mathrm{dB}$ \\
  \bottomrule
\end{tabular}
```

完全な複数行グループは [`TABLE-MULTIROW-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、表示結果は PDF 第 11 ページです。`\multirow{2}{*}{通過域}` の `2` は、実際にまたがるデータ行数と一致させてください。パッケージが同じ文字列を見つけて自動的にまとめるわけではありません。表が広い場合は、まず列の内容を短くするか、`X` 列や `p` 列を使います。表全体の文字を小さくする場合は、数値と単位を読めるかも確認してください。

## TikZ、CircuitikZ、`mdframed`

`\input{config/diagrams}` のコメントを外すと、TikZ、CircuitikZ、`mdframed` を読み込み、`positioning`、`calc`、`arrows.meta`、`circuits.logic.US` の四つの TikZ ライブラリを有効にします。モジュールの設定ファイルは [`config/diagrams.tex`](../../template/config/diagrams.tex) です。

次はサンプルにある RC 回路の最小構成で、`figure` 内に置けます。

```tex
\begin{circuitikz}[american voltages]
  \draw
    (0,0) node[left]{$V_{\mathrm{in}}$}
    to[R,l=$R$] (2.4,0)
    -- (3.3,0) node[right]{$V_{\mathrm{out}}$}
    (2.4,0) to[C,l=$C$] (2.4,-2)
    -- (0,-2) -- (0,0);
\end{circuitikz}
```

この用例は [`DIAG-CIRCUIT-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、表示結果は PDF 第 8 ページです。

一般的な装置接続図では、`tikzpicture` のノード、相対位置、矢印を使います。サンプルは `right=4mm of generator` で装置を並べ、`$(A)!0.5!(B)$` で中点を計算し、`mdframed` で装置図全体を囲みます。安定 ID は [`DIAG-APPARATUS-01`](../../examples/FEATURES.md)、[`DIAG-TIKZ-01`](../../examples/FEATURES.md)、[`DIAG-CALC-01`](../../examples/FEATURES.md)、[`DIAG-FRAME-01`](../../examples/FEATURES.md) です。すべて[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)にあり、表示結果は PDF 第 8 ページです。`circuits.logic.US` の AND ゲートは [`DIAG-LOGIC-01`](../../examples/FEATURES.md) に対応し、PDF 第 8 ページで確認できます。

サンプルの `device/.style` は、その `tikzpicture` 内だけで定義され、テンプレートの公開スタイルではありません。`diagrams.tex` は描画ツールを読み込むだけで、「装置図環境」は追加しません。色付きの説明と灰色のコード欄は、サンプル専用の `config/sample-guide.tex` で作成しており、テンプレートの機能ではありません。

`mdframed` は次のように直接使えます。

```tex
\begin{mdframed}[
  linecolor=black!60,
  linewidth=0.7pt,
  roundcorner=3pt
]
  ここに装置構成や注意事項を記述する。
\end{mdframed}
```

モジュール内部では、TeX Live 2026 のリスト実装と `mdframed` が内部で使う旧式リストとの互換性を保つ処理も行っています。利用者向けの新しい環境は追加されません。通常どおり `mdframed` を使い、互換処理で使われる内部コマンドをコピーしたり、直接呼び出したりしないでください。

## コードリスト

`\input{config/listings}` のコメントを外すと、`lstlisting` と `\lstinline` を使えます。モジュール全体の設定は、等幅 8 pt の文字と 10 pt の行送り、単線の枠、左側の行番号、`columns=fullflexible`、自動折り返し、キーワード／コメント／文字列の色分け、下側のキャプション、文字列中の空白記号を非表示にする指定を含みます。コードリスト名は「リスト」、コードリスト一覧名は「リスト一覧」に統一されます。最小のコードブロックは次のとおりです。

```tex
\begin{lstlisting}[
  language=Python,
  caption={測定値の読み込み},
  label={lst:read-data}
]
import csv

with open("data.csv", encoding="utf-8") as stream:
    rows = list(csv.DictReader(stream))
\end{lstlisting}
```

`language=Python` は構文強調を決めます。`language=bash` に変えると Bash を表示できます。`caption` と `label` を付けたコードリストは、`\cref{lst:read-data}` で参照できます。短いファイル名は本文で `\lstinline|data.csv|` と書けます。

完全な Python コードブロックは [`CODE-BLOCK-01`](../../examples/FEATURES.md)、構文強調、自動折り返し、相互参照は [`CODE-HIGHLIGHT-01`](../../examples/FEATURES.md)、[`CODE-WRAP-01`](../../examples/FEATURES.md)、[`CODE-REF-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex)、表示結果は PDF 第 16～17 ページです。二つ目の Python と Bash の用例は `CODE-BLOCK-02` と `CODE-LANGUAGE-02` で、どちらも第 17 ページにあります。行内コードは [`CODE-INLINE-01`](../../examples/FEATURES.md)、ソースは[サンプル 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、表示結果は PDF 第 10 ページです。

`listings` が認識しない言語を `language=...` に指定するとエラーになります。その場合は、まずこのオプションを外して言語固有の強調がないコードブロックにし、必要であれば自分のテンプレートへ言語定義を追加してください。`listings` は組版だけを行い、プログラムを実行または検証しません。自動折り返しも表示だけを変え、元のコードは変更しません。サンプルで動作確認しているのは、ASCII 文字のみを含むコードです。Unicode 文字を含むコメントでは、同じ表示結果にならない場合があります。`\lstinline|...|` では、区切りに選んだ `|` を内容中に再び使えません。

## PGFPlots のデータグラフ

`\input{config/plots}` のコメントを外すと PGFPlots を使えます。モジュールは `compat=1.18` に固定し、五つの公開スタイルを提供します。

- `report axis base`：軸線、寸法、グリッド、目盛、ラベルの基本書式です。凡例は含みません。
- `report legend`：凡例の位置、枠、文字、サンプル線の書式です。
- `report axis`：`report axis base` と `report legend` を組み合わせ、一般的な凡例付きグラフに使います。
- `report plot`：データ系列の線幅、色、マーカーサイズを統一しますが、マーカーの種類は自動選択しません。
- `report dual right axis`：背景を透明にし、右側目盛を使い、横軸を隠す第 2 縦軸です。

定義は [`config/plots.tex`](../../template/config/plots.tex) にあります。一般的な 1 軸グラフは次の構造から始められます。CSV の最初の行には `frequency_hz` と `gain_measured` の二つの列名が必要です。

```tex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}
    \begin{axis}[
      report axis,
      xmode=log,
      xlabel={周波数 / Hz},
      ylabel={利得 / dB}
    ]
      \addplot[report plot,mark=*]
        table[x=frequency_hz,y=gain_measured,col sep=comma]
        {data/frequency-response.csv};
      \addlegendentry{測定値}
    \end{axis}
  \end{tikzpicture}
  \caption{利得の周波数特性}
  \label{fig:gain-response}
\end{figure}
```

サンプルのデータファイルは [frequency-response.csv](../../examples/source/rc-low-pass-filter/data/frequency-response.csv) です。CSV の読み込み、1 軸、共通の軸／系列／凡例、対数横軸は、それぞれ [`PLOT-DATAFILE-01`](../../examples/FEATURES.md)、[`PLOT-SINGLE-01`](../../examples/FEATURES.md)、[`PLOT-STYLE-AXIS-01`](../../examples/FEATURES.md)、[`PLOT-STYLE-PLOT-01`](../../examples/FEATURES.md)、[`PLOT-STYLE-LEGEND-01`](../../examples/FEATURES.md)、[`PLOT-LOG-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、表示結果は PDF 第 13 ページで、同じデータを第 13～15 ページでも引き続き使っています。

Release テンプレートには初期状態で `data/` ディレクトリがありません。このパスを使う場合は、自分でディレクトリを作成し、CSV も一緒に保存してください。ヘッダーは `x=`、`y=` の後の名前と一致させ、カンマ区切りのファイルでは `col sep=comma` を残します。`report axis` が決めるのは凡例の外観だけです。`\addlegendentry` がなければ凡例は自動生成されません。スタイルより後ろに書いた軸オプションは、スタイルの既定値を上書きします。

左右 2 軸グラフでは、一つの `tikzpicture` に、寸法と横軸範囲がまったく同じ二つの `axis` を重ねます。

```tex
\begin{tikzpicture}
  \begin{axis}[
    report axis,
    width=0.78\linewidth,
    height=0.48\linewidth,
    xmin=100, xmax=10000,
    xlabel={周波数 / Hz},
    ylabel={利得 / dB}
  ]
    \addplot[report plot,mark=*]
      coordinates {(100,0) (1000,-1.4) (10000,-15.9)};
    \addlegendentry{利得}
  \end{axis}
  \begin{axis}[
    report dual right axis,
    width=0.78\linewidth,
    height=0.48\linewidth,
    xmin=100, xmax=10000,
    ylabel={位相差 / deg}
  ]
    \addplot[blue!60!black,mark=square*]
      coordinates {(100,-4) (1000,-33) (10000,-81)};
    \addlegendentry{位相差}
  \end{axis}
\end{tikzpicture}
```

二つの軸で `width`、`height`、`xmin`、`xmax`、対数／線形の設定を一致させないと、目盛位置は重なりません。左右 2 軸グラフの実例は [`PLOT-DUAL-01`](../../examples/FEATURES.md) と [`PLOT-STYLE-RIGHT-01`](../../examples/FEATURES.md)、ソースは[サンプル 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、表示結果は PDF 第 14～15 ページです。凡例が不要な残差グラフでは `report axis base` を直接使います。これは [`PLOT-BASE-01`](../../examples/FEATURES.md) に対応し、表示結果は PDF 第 15 ページです。

PGFPlots には多くの軸オプションがありますが、このプロジェクトがテンプレートの公開インターフェースとするのは上記の五つのスタイルだけです。座標範囲、目盛、ラベル、線種、データ列の変更は通常の使い方です。全体のスタイルを変える場合は `config/plots.tex` を調整し、1 軸、左右 2 軸、凡例なしの三種類をあらためて確認してください。

## ソース・説明・PDF の対応関係

このページで扱う機能 ID、ソース位置、PDF ページ番号は、すべて[サンプル機能一覧](../../examples/FEATURES.md)に基づきます。サンプル全体を確認するときは、次の対応表から該当箇所をたどれます。

| 機能 | 安定 ID | ソース | PDF ページ |
| --- | --- | --- | --- |
| 外部画像、画像ディレクトリ | `GRAPHICSPATH-01`、`FIG-IMAGE-01` | [report.tex](../../examples/source/rc-low-pass-filter/report.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 9 |
| サブ図とフロート位置 | `FIG-SUBFIGURE-01`、`FLOAT-H-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex) | 7～9、11～15、17 |
| 表 | `TABLE-TABULARX-01`、`TABLE-PCOLUMN-01`、`TABLE-BOOKTABS-01`、`TABLE-MULTIROW-01`、`TABLE-REPEAT-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 6～7、11～12 |
| 回路図と装置図 | `DIAG-CIRCUIT-01`、`DIAG-APPARATUS-01`、`DIAG-TIKZ-01`、`DIAG-CALC-01`、`DIAG-FRAME-01`、`DIAG-LOGIC-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| コードリスト | `CODE-INLINE-01`、`CODE-BLOCK-01`、`CODE-HIGHLIGHT-01`、`CODE-WRAP-01`、`CODE-REF-01`、`CODE-BLOCK-02`、`CODE-LANGUAGE-02` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex) | 10、16～17 |
| PGFPlots | `PLOT-DATAFILE-01`、`PLOT-SINGLE-01`、`PLOT-STYLE-AXIS-01`、`PLOT-STYLE-PLOT-01`、`PLOT-STYLE-LEGEND-01`、`PLOT-LOG-01`、`PLOT-DUAL-01`、`PLOT-STYLE-RIGHT-01`、`PLOT-BASE-01` | [04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 13～15 |

---

[← 前へ：独自コマンドと環境](07-commands-and-environments.md) | [次へ：数式と相互参照 →](09-math-and-cross-references.md)
