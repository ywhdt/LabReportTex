# 自行修改模板

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/13-customization.md)

模板已经把“每份报告都要填写的内容”和“课程要求变化时才需要调整的配置”分开放置。通常可以安全修改 `config/metadata.tex`、`sections/`、图片和参考文献；页面布局、基本信息表、页眉、字体与可选模块则集中在 `config/`。

## 页面尺寸与边距

[`report.tex`](../../template/report.tex) 在 `\documentclass` 之前声明 `\DocumentMetadata`，随后使用 `ltjsarticle` 文档类；在项目支持的 TeX Live 2026 环境中，当前默认编译结果是 A4 页面。调整页面时不要交换这两条命令的顺序。四边边距则由 [`config/preamble.tex`](../../template/config/preamble.tex) 中的 `geometry` 明确设置：

```tex
\geometry{top=25mm,bottom=30mm,left=25mm,right=25mm,footskip=12mm}
\setlength{\headheight}{30pt}
```

这组设置对应 [`LAYOUT-PAGE-01`](../../examples/FEATURES.md)，可在[示例 PDF 第 1–21 页](../../examples/compiled/rc-low-pass-filter.pdf)观察整体效果。课程只要求改变页边距时，优先修改这一行，而不要在各章节中分别加入负间距或缩放命令。例如要把左右边距改为 30 mm，可以写成：

```tex
\geometry{top=25mm,bottom=30mm,left=30mm,right=30mm,footskip=12mm}
```

如果课程还指定了纸张尺寸，应同时核对文档类的纸张选项、打印设置和最终 PDF 的页面尺寸，不能只改变页边距数值。模板默认以 A4 为目标；改成其他纸张尺寸后，至少需要检查基本信息表、页眉、宽表格、并排子图和 PGFPlots 图形。

`\headheight` 为多列页眉预留高度。页眉内容变多、字号变大或换行增加后，如果日志出现 `\headheight is too small`，应根据实际页眉高度增大这个值，而不是忽略警告。修改上下边距时还要确认正文不会与页眉、居中的页码或页面边缘重叠。

## 基本信息表、页眉与页脚

模板没有单独占满一页的标题页。`report.tex` 中的 `\maketitle` 调用由 [`config/commands.tex`](../../template/config/commands.tex) 重新定义，用来在第一页正文之前生成“レポートの基本情報”表；随后正文在同一页继续排版。该效果对应 [`TITLEBLOCK-01`](../../examples/FEATURES.md)，示例见 PDF 第 1 页。

基本信息表使用 `tabularx`，页眉使用 `fancyhdr`。两者都由 `commands.tex` 定义，但所需宏包在 `config/preamble.tex` 中先行加载。因此，应保持 `report.tex` 中下面的读取顺序：

```tex
\input{config/preamble}
\input{config/commands}
\input{config/metadata}
```

`commands.tex` 必须在 `metadata.tex` 之前读取，否则 `\course`、`\group` 等项目命令尚未定义；`preamble.tex` 又必须在 `commands.tex` 之前读取，否则 `tabularx`、`fancyhdr` 等接口不可用。每份报告的实际内容仍应填写在 `metadata.tex`，不要把姓名、学号或日期直接写死在排版定义中。

如果只在自己的报告中增加一个基本信息字段，需要在 `commands.tex` 定义接收值的命令并提供空默认值，在 `metadata.tex` 填写该命令，再在 `\maketitle` 或页眉中安排显示位置。只在表格中直接写固定文字，不会形成可以逐份报告填写的字段。新增命令不要使用模板内部保留的 `labreport@` 或含 `@` 名称。

基本信息表的字段位置在 `\renewcommand{\maketitle}{...}` 内调整。页眉各列位于 `\fancyhead[C]{...}` 内，页脚当前只有居中的页码：

```tex
\fancyfoot[C]{\thepage}
```

页眉采用四个固定宽度列和一个自动占用剩余空间的 `X` 列。改变列数或宽度时，应保证总宽度不超过 `\textwidth`，并用短值和接近真实上限的长值各编译一次。页眉与页码的完整效果对应 [`LAYOUT-HEADER-01`](../../examples/FEATURES.md)，各字段的对应关系见 [`META-HEADER-01`](../../examples/FEATURES.md)。

如果课程确实要求独立标题页，应把它当作新增文档结构来设计，并明确它是否显示页眉、是否计入页码以及正文从哪一页开始；不要把当前基本信息表误认为一个已经存在的标题页接口。

## 字体与字号

日文字体、欧文字体和数学字体由不同机制管理。当前日文字体来自 `config/preamble.tex` 中的 `luatexja-preset`：

```tex
\usepackage[deluxe]{luatexja-preset}
```

更换预设时只替换这一行，不要重复加载宏包。可用预设、字体文件检查和中文排版不在支持范围内等边界，见[日文排版与字体](11-languages-and-fonts.md)。混合排版效果对应 [`CORE-JAPANESE-01`](../../examples/FEATURES.md)。

模板没有在文档类选项中显式固定全局字号，当前 `ltjsarticle` 默认正文为 10 pt；基本信息表标题使用 `\Large`，页眉使用 `\small`，代码清单在 `config/listings.tex` 中使用 `\fontsize{8}{10}\selectfont`。因此，“改变正文字号”“改变页眉字号”和“改变代码字号”是三项不同的修改。只需要局部强调时，优先在有限范围内使用 `\small`、`\large` 等相对字号，不要在正文中反复设置绝对字号。

如果课程要求改变全局字号，需要修改文档类选项，并重新检查页数、浮动体位置、表格换行、图题、基本信息表和页眉。日文字体预设不会自动同步替换欧文和数学字体；若三者都有明确指定，应分别配置并检查 PDF 的字体嵌入结果。

## 图表配色与样式

核心导言区默认加载 `xcolor`，但示例 PDF 中的多色说明文字只是示例的局部标注方式，不是要求普通报告保留的模板环境。该展示对应 [`COLOR-ANNOTATION-01`](../../examples/FEATURES.md)。

不同图表的长期设置位于各自配置文件中：

- [`config/listings.tex`](../../template/config/listings.tex)：代码字号、边框、行号以及关键字、注释、字符串的颜色。
- [`config/plots.tex`](../../template/config/plots.tex)：`plotLine` 颜色，以及 `report axis`、`report plot`、`report legend`、`report dual right axis` 等 PGFPlots 样式。
- [`config/diagrams.tex`](../../template/config/diagrams.tex)：TikZ、CircuitikZ 和 `mdframed` 的加载与兼容处理；具体图形的颜色通常在正文中的绘图选项里指定。

代码和绘图模块默认关闭。先按照[图片、表格、代码与绘图](08-figures-tables-and-code.md)启用对应模块，再修改它的样式文件。修改 `config/plots.tex` 中的共通样式后，应同时检查单纵轴、双纵轴和无图例图形；示例中的对应入口是 [`PLOT-STYLE-AXIS-01`](../../examples/FEATURES.md)、[`PLOT-DUAL-01`](../../examples/FEATURES.md) 与 [`PLOT-BASE-01`](../../examples/FEATURES.md)。

颜色不应成为区分数据系列的唯一手段。提交前最好同时检查灰度打印、线型、点型、图例和文字对比度。模板不会自动验证课程指定色、色觉可辨识性或打印机效果，这些都需要在最终 PDF 中人工确认。

## 拆分新的配置文件

只有当一组设置会被多处使用，或已经使现有配置难以阅读时，才值得拆出新文件。包加载命令必须位于 `\begin{document}` 之前。若新增的是可选功能，可以仿照现有模块创建 `config/course-style.tex`，再从 `config/preamble.tex` 读取：

```tex
% Optional course-specific styles.
% \input{config/course-style}
```

这种文件适合放置该功能所需的 `\usepackage`、颜色和样式定义。启用后必须把文件与报告一起保存；Release 模板不会自动生成用户新增的配置。

如果新文件用于覆盖基本信息表、页眉或项目命令，它应在 `config/commands.tex` 之后、`config/metadata.tex` 之前读取：

```tex
\input{config/preamble}
\input{config/commands}
\input{config/course-overrides}
\input{config/metadata}
```

如果新文件定义了 `metadata.tex` 将要调用的新字段命令，也必须保持这个顺序。反过来，正文专用内容不应放进导言区；它更适合留在 `sections/` 或单独的正文文件中。

不要拆开 [`biblatex-jareport.sty`](../../template/biblatex-jareport.sty) 与 [`biblatex-jareport-japanese.lbx`](../../template/biblatex-jareport-japanese.lbx)，也不要把 `\usepackage` 放入章节文件。`config/diagrams.tex` 中还有为 TeX Live 2026 的列表行为保留的 `mdframed` 兼容处理，启用装置图模块时应保留整份文件，而不是只复制三条宏包加载命令。

## 保持升级兼容

模板不会自动合并用户改动，也没有自动升级命令。Release ZIP 中的 `VERSION` 只标记下载包版本；开发树的 `template/` 本身没有该文件。升级前应保留当前可编译目录，并把自己的修改分成三类：

1. `metadata.tex`、章节、图片和 `.bib` 数据，可以按新目录中的相同位置迁移。
2. 页面边距、字体预设和模块样式等配置，应逐项与新版本比较后重新应用。
3. `commands.tex`、`report.tex`、本地参考文献样式和 `cleveref` 包装属于高耦合部分，不应直接用旧文件整体覆盖新版本。

尤其不要覆盖 [`config/preamble.tex`](../../template/config/preamble.tex) 中 `showonlyrefs` 与 `cleveref` 的兼容包装，也不要改变 `hyperref`、`cleveref` 的加载顺序；相关注意事项见[公式与交叉引用](09-math-and-cross-references.md)。

迁移完成后，从能够看到 `report.tex` 的目录运行完整编译，并检查日志、PDF 页面、引用、字体、图表和参考文献。

---

[← 上一篇：源码、说明与 PDF 对照](12-source-doc-pdf-cross-reference.md) | [下一篇：常见问题与故障排除 →](14-troubleshooting.md)
