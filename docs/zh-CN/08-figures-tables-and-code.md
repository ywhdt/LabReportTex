# 图片、表格、代码与绘图

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/08-figures-tables-and-code.md)

模板默认已经加载外部图片、浮动体、子图和常用表格所需的核心宏包。TikZ／CircuitikZ／`mdframed`、代码清单和 PGFPlots 较重，因此拆成三个可选模块。只启用实际会使用的模块，可以让第一份报告保持简单，也能缩小错误来源。

## 先确认是否需要启用模块

外部图片和表格不需要额外操作。要使用装置图、代码或数据图，请打开 [`config/preamble.tex`](../../template/config/preamble.tex)，取消对应行的注释：

```tex
\input{config/diagrams} % TikZ、CircuitikZ、mdframed
\input{config/listings} % listings
\input{config/plots}    % PGFPlots
```

三行可以分别启用，不要求同时打开。示例的集中启用方式对应 [`MODULES-01`](../../examples/FEATURES.md)，三个单独入口分别为 [`MODULE-DIAGRAMS-01`](../../examples/FEATURES.md)、[`MODULE-LISTINGS-01`](../../examples/FEATURES.md) 和 [`MODULE-PLOTS-01`](../../examples/FEATURES.md)；入口位于[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，效果分别见示例 PDF 第 8–9 页、第 10 与 16–17 页、第 13–15 页。

## 外部图片

把 PNG、JPG 或 PDF 图片放入 `figures/`。根文件已设置 `\graphicspath{{figures/}}`，正文通常只写文件名：

```tex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.82\linewidth]{oscilloscope-waveform.png}
  \caption{遮断周波数付近で観測した波形}
  \label{fig:oscilloscope-waveform}
\end{figure}
```

`width=0.82\linewidth` 表示图片宽度为当前正文宽度的 82%。建议用相对正文宽度控制大小，不要依赖某台电脑上的绝对路径。先写 `\caption`、再写 `\label`，后续即可用 `\cref{fig:oscilloscope-waveform}` 引用正确的图号。

完整用例对应 [`GRAPHICSPATH-01`](../../examples/FEATURES.md) 和 [`FIG-IMAGE-01`](../../examples/FEATURES.md)。路径设置见[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，图片插入见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，原始图片为[oscilloscope-waveform.png](../../examples/source/rc-low-pass-filter/figures/oscilloscope-waveform.png)，效果见 [PDF 第 9 页](../../examples/compiled/rc-low-pass-filter.pdf)。`\graphicspath` 只设置搜索位置，不会复制图片；提交或移动报告时仍需保留实际文件。

### 浮动位置与子图

普通情况下优先使用 `[htbp]`，让 LaTeX 在当前位置、页顶、页底或浮动页中选择合适位置。模板加载的 `float` 还提供 `[H]`，用于确实需要固定在当前位置的图表；大量使用 `[H]` 可能留下较大的页尾空白，因此应在 PDF 中检查分页。该行为对应 [`FLOAT-H-01`](../../examples/FEATURES.md)，源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)与[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex)，效果分布在 PDF 第 7–9、11–15、17 页。

需要把两张图并排并分别编号时，可以使用已经加载的 `subcaption`：

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

这里的两个文件名需要换成自己放入 `figures/` 的图片。子图写法来自示例中同样的 `subfigure` 结构；完整的回路图与装置图并排用例对应 [`FIG-SUBFIGURE-01`](../../examples/FEATURES.md)，源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，效果见 PDF 第 8 页。子图范围引用另有 `REF-SUBFIG-RANGE-01`，在下一篇说明。

模板通过 `subcaption` 提供 `subfigure` 环境，并统一把子图编号设为 `(a)`、`(b)` 形式；这里不是旧 `subfigure` 宏包提供的环境。两个子图的宽度与中间间距之和不能超过父图的 `\linewidth`，各子图和父图也必须使用不同标签。

## 表格

简单数值表可以使用标准 `tabular`；说明文字较长时，优先使用 `tabularx` 的 `X` 列自动分配剩余宽度。下面是一个可以直接修改的基础结构：

```tex
\begin{table}[htbp]
  \centering
  \caption{測定条件}
  \label{tab:conditions}
  \begin{tabularx}{\linewidth}{lX}
    \toprule
    項目 & 条件 \\
    \midrule
    入力波形 & 正弦波、振幅 \qty{1.00}{\volt} \\
    周波数 & \qty{100}{\hertz} から \qty{10}{\kilo\hertz} \\
    \bottomrule
  \end{tabularx}
\end{table}
```

- `X` 列使用表格中剩余的宽度并自动换行，对应 [`TABLE-TABULARX-01`](../../examples/FEATURES.md)，示例效果在 PDF 第 6–7 页。
- `p{0.20\linewidth}` 可以建立固定宽度且可换行的列，对应 [`TABLE-PCOLUMN-01`](../../examples/FEATURES.md)，效果在 PDF 第 7 页。
- `\toprule`、`\midrule`、`\bottomrule` 来自 `booktabs`，用于层次清楚的横线，对应 [`TABLE-BOOKTABS-01`](../../examples/FEATURES.md)，效果在 PDF 第 7 页。通常不需要再加入大量竖线。
- 列数固定、内容都很短时，标准 `tabular` 更直接。数值比较用例对应 [`TABLE-REPEAT-01`](../../examples/FEATURES.md)，源码见[示例 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)，效果在 PDF 第 12 页。

需要让 `X` 或 `p` 列左对齐时，示例采用 `>{\raggedright\arraybackslash}X`。`\arraybackslash` 在改变段落对齐后恢复表格行末 `\\` 的作用，不应只复制 `\raggedright` 而省略它。表格过宽时，还可以像示例那样在局部缩小 `\tabcolsep`；这只改变列间留白，不会自动压缩内容。

需要把同一分类跨多行显示时，可以使用已经加载的 `multirow`；若想简洁地绘制三线表，可以与上面的 `booktabs` 配合使用：

```tex
\begin{tabular}{lll}
  \toprule
  領域 & 周波数 & 利得 \\
  \midrule
  \multirow{2}{*}{通過域}
    & \qty{100}{\hertz} & \qty{0.00}{\decibel} \\
    & \qty{500}{\hertz} & \qty{-0.35}{\decibel} \\
  \bottomrule
\end{tabular}
```

完整多行分组对应 [`TABLE-MULTIROW-01`](../../examples/FEATURES.md)，源码见[示例 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)，效果见 PDF 第 11 页。`\multirow{2}{*}{通過域}` 中的 `2` 必须与实际跨越的数据行数一致；宏包不会按照相同文字自动分组。表格变宽时，先缩短列内容或改用 `X`、`p` 列；直接缩小整张表的字号应作为后续选择，并检查数字和单位是否仍可读。

## TikZ、CircuitikZ 与 `mdframed`

取消 `\input{config/diagrams}` 的注释后，会加载 TikZ、CircuitikZ 和 `mdframed`，并启用 `positioning`、`calc`、`arrows.meta`、`circuits.logic.US` 四个 TikZ 库。模块入口见 [`config/diagrams.tex`](../../template/config/diagrams.tex)。

下面是示例中 RC 回路的最小结构，可以放在 `figure` 中：

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

该用例对应 [`DIAG-CIRCUIT-01`](../../examples/FEATURES.md)，源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，效果见 PDF 第 8 页。

普通装置连接图使用 `tikzpicture` 中的节点、相对位置和箭头。示例以 `right=4mm of generator` 排列设备，用 `$(A)!0.5!(B)$` 计算中点，并用 `mdframed` 给整幅装置图加框。稳定 ID 分别为 [`DIAG-APPARATUS-01`](../../examples/FEATURES.md)、[`DIAG-TIKZ-01`](../../examples/FEATURES.md)、[`DIAG-CALC-01`](../../examples/FEATURES.md) 和 [`DIAG-FRAME-01`](../../examples/FEATURES.md)，全部位于[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，效果见 PDF 第 8 页。`circuits.logic.US` 的 AND 门用例为 [`DIAG-LOGIC-01`](../../examples/FEATURES.md)，效果见 PDF 第 8–9 页。

示例中的 `device/.style` 只在该 `tikzpicture` 内定义，并不是模板公开样式。`diagrams.tex` 只加载绘图工具，没有新增“装置图环境”。彩色说明与灰色源码框来自示例专用的 `config/sample-guide.tex`，也不属于模板功能。

`mdframed` 可以直接这样使用：

```tex
\begin{mdframed}[
  linecolor=black!60,
  linewidth=0.7pt,
  roundcorner=3pt
]
  ここに装置構成や注意事項を記述する。
\end{mdframed}
```

模块内部还有一段 TeX Live 2026 列表实现与 `mdframed` 旧式内部列表的兼容补丁。它不会新增用户环境；正常使用 `mdframed` 即可，不要复制或调用补丁里的内部命令。

## 代码清单

取消 `\input{config/listings}` 的注释后，可以使用 `lstlisting` 和 `\lstinline`。模块的全局设置包括等宽 8 pt 字体与 10 pt 行距、单线边框、左侧行号、`columns=fullflexible`、自动折行、关键字／注释／字符串配色、位于下方的题注，以及不显示字符串中的空格标记。清单名称统一为“リスト”，清单目录名称为“リスト一覧”。一个最小代码块如下：

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

`language=Python` 决定语法高亮；换成 `language=bash` 可以显示 Bash。带 `caption` 和 `label` 的清单可以用 `\cref{lst:read-data}` 引用。短文件名可在正文中写成 `\lstinline|data.csv|`。

完整 Python 代码块对应 [`CODE-BLOCK-01`](../../examples/FEATURES.md)，语法高亮、自动换行和交叉引用分别对应 [`CODE-HIGHLIGHT-01`](../../examples/FEATURES.md)、[`CODE-WRAP-01`](../../examples/FEATURES.md)、[`CODE-REF-01`](../../examples/FEATURES.md)，源码见[示例 05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex)，效果见 PDF 第 16–17 页。第二段 Python 和 Bash 用例分别为 `CODE-BLOCK-02`、`CODE-LANGUAGE-02`，也位于第 17 页。行内代码对应 [`CODE-INLINE-01`](../../examples/FEATURES.md)，源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，效果见 PDF 第 10 页。

如果某种语言不在 `listings` 的已知语言中，`language=...` 会报错；此时先删除该选项得到无语言高亮的代码块，再决定是否为自己的模板增加语言定义。`listings` 只负责排版，不执行或验证程序；自动折行也只改变显示，不会修改原始代码。示例验证的是当前 ASCII 代码内容，不应据此假定任意 Unicode 源码注释都会获得相同效果。使用 `\lstinline|...|` 时，选作边界的 `|` 不能再出现在内容中。

## PGFPlots 数据图

取消 `\input{config/plots}` 的注释后，可以使用 PGFPlots。模块固定 `compat=1.18`，并提供五个公开样式：

- `report axis base`：轴线、尺寸、网格、刻度和标签的基础格式，不带图例。
- `report legend`：图例的位置、边框、字体和示意线格式。
- `report axis`：组合 `report axis base` 与 `report legend`，适合普通带图例的图。
- `report plot`：统一数据系列的线宽、颜色和标记大小，但不自动选择标记类型。
- `report dual right axis`：透明背景、右侧刻度且隐藏横轴的第二纵轴。

这些定义位于 [`config/plots.tex`](../../template/config/plots.tex)。普通单纵轴图可以从下面的结构开始；CSV 第一行需要有 `frequency_hz` 和 `gain_measured` 两个列名：

```tex
\begin{figure}[H]
  \centering
  \begin{tikzpicture}
    \begin{axis}[
      report axis,
      xmode=log,
      xlabel={周波数 / \unit{\hertz}},
      ylabel={利得 / \unit{\decibel}}
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

示例中的数据文件是 [frequency-response.csv](../../examples/source/rc-low-pass-filter/data/frequency-response.csv)。读取 CSV、单纵轴、共通轴／系列／图例和对数横轴分别对应 [`PLOT-DATAFILE-01`](../../examples/FEATURES.md)、[`PLOT-SINGLE-01`](../../examples/FEATURES.md)、[`PLOT-STYLE-AXIS-01`](../../examples/FEATURES.md)、[`PLOT-STYLE-PLOT-01`](../../examples/FEATURES.md)、[`PLOT-STYLE-LEGEND-01`](../../examples/FEATURES.md) 与 [`PLOT-LOG-01`](../../examples/FEATURES.md)。源码见[示例 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)，效果见 PDF 第 13 页，并在第 13–15 页继续展示同一数据。

Release 模板默认没有 `data/` 目录；采用这个路径时需要自行创建目录并把 CSV 一同保存。表头必须与 `x=`、`y=` 后的名称一致，读取逗号分隔文件时保留 `col sep=comma`。`report axis` 只规定图例外观，没有 `\addlegendentry` 就不会自动生成图例；样式后的轴选项会覆盖样式默认值。

双纵轴图是在同一个 `tikzpicture` 中重叠两个尺寸和横轴范围完全相同的 `axis`：

```tex
\begin{tikzpicture}
  \begin{axis}[
    report axis,
    width=0.78\linewidth,
    height=0.48\linewidth,
    xmin=100, xmax=10000,
    xlabel={周波数 / \unit{\hertz}},
    ylabel={利得 / \unit{\decibel}}
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
    ylabel={位相差 / \unit{\degree}}
  ]
    \addplot[blue!60!black,mark=square*]
      coordinates {(100,-4) (1000,-33) (10000,-81)};
    \addlegendentry{位相差}
  \end{axis}
\end{tikzpicture}
```

两个轴的 `width`、`height`、`xmin`、`xmax` 以及对数／线性设置必须一致，否则刻度不会重合。完整双纵轴用例对应 [`PLOT-DUAL-01`](../../examples/FEATURES.md) 与 [`PLOT-STYLE-RIGHT-01`](../../examples/FEATURES.md)，源码见[示例 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)，效果见 PDF 第 14–15 页。无需图例的残差图直接使用 `report axis base`，对应 [`PLOT-BASE-01`](../../examples/FEATURES.md)，效果见 PDF 第 15 页。

PGFPlots 的轴选项很多，本项目只把上面五个样式作为模板公开接口。修改坐标范围、刻度、标签、线型和数据列属于正常用法；若要改动全局风格，应在 `config/plots.tex` 中调整并重新检查单纵轴、双纵轴和无图例三类图。

## 三方对照入口

本篇提到的功能 ID、源码位置与 PDF 页码均来自[示例功能索引](../../examples/FEATURES.md)。阅读完整示例时可以按下面的范围定位：

| 功能 | 稳定 ID | 源码 | PDF 页 |
| --- | --- | --- | --- |
| 外部图片、图片目录 | `GRAPHICSPATH-01`、`FIG-IMAGE-01` | [report.tex](../../examples/source/rc-low-pass-filter/report.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 9 |
| 子图与浮动位置 | `FIG-SUBFIGURE-01`、`FLOAT-H-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex) | 7–9、11–15、17 |
| 表格 | `TABLE-TABULARX-01`、`TABLE-PCOLUMN-01`、`TABLE-BOOKTABS-01`、`TABLE-MULTIROW-01`、`TABLE-REPEAT-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 6–7、11–12 |
| 回路与装置图 | `DIAG-CIRCUIT-01`、`DIAG-APPARATUS-01`、`DIAG-TIKZ-01`、`DIAG-CALC-01`、`DIAG-FRAME-01`、`DIAG-LOGIC-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 8–9 |
| 代码清单 | `CODE-INLINE-01`、`CODE-BLOCK-01`、`CODE-HIGHLIGHT-01`、`CODE-WRAP-01`、`CODE-REF-01`、`CODE-BLOCK-02`、`CODE-LANGUAGE-02` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex) | 10、16–17 |
| PGFPlots | `PLOT-DATAFILE-01`、`PLOT-SINGLE-01`、`PLOT-STYLE-AXIS-01`、`PLOT-STYLE-PLOT-01`、`PLOT-STYLE-LEGEND-01`、`PLOT-LOG-01`、`PLOT-DUAL-01`、`PLOT-STYLE-RIGHT-01`、`PLOT-BASE-01` | [04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 13–15 |

---

[← 上一篇：自定义命令与环境](07-commands-and-environments.md) | [下一篇：公式与交叉引用 →](09-math-and-cross-references.md)
