# 自定义命令与环境

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/07-commands-and-environments.md)

本篇说明模板直接定义的命令、可使用的列表环境，以及 `config/` 中哪些内容属于公开入口。图片、绘图和交叉引用的具体写法分别放在[图片、表格、代码与绘图](08-figures-tables-and-code.md)和[公式与交叉引用](09-math-and-cross-references.md)中。

## 报告信息命令由谁定义

每份报告填写的值放在 [`config/metadata.tex`](../../template/config/metadata.tex)，命令定义和显示方式则在 [`config/commands.tex`](../../template/config/commands.tex)。两者的分工是：

- `\title`、`\author` 是文档类已有的命令，模板使用它们生成第一页信息表，并将内容写入 PDF 的 Title 和 Author 属性。
- `\course`、`\group`、`\theme`、`\studentid`、`\experimentpartner`、`\staff`、`\experimentdate`、`\place`、`\experimenttime`、`\reportfirstdue` 是模板定义的公开命令。
- `\maketitle` 仍是正文中的公开调用入口，但模板把它重新定义为“レポートの基本情報”表，而不是文档类原来的标题样式。

普通使用者只应修改 `metadata.tex` 中花括号里的值。各字段显示位置、`\group{A}` 自动生成 `A班` 和 PDF 元数据已经在[报告基本信息](05-metadata.md)中完整说明。

这些单参数命令不检查内容、长度或格式；同一命令重复填写时，最后一次调用会覆盖前面的值。标准的 `\date` 没有进入模板的信息表，填写它不会自动显示。`\maketitle` 只输出宽度为 `0.95\linewidth` 的基本信息表，不创建独立封面，也不强制换页。页眉用固定宽度的 `tabularx` 依次放置课程、主题、学号、班组和作者，页脚中央显示页码；内容很长时仍需检查换行和页眉高度。

示例把命令入口标为 [`MODULE-COMMANDS-01`](../../examples/FEATURES.md)，源码见[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，效果贯穿 [PDF 第 1–21 页](../../examples/compiled/rc-low-pass-filter.pdf)。第一页信息表对应 [`TITLEBLOCK-01`](../../examples/FEATURES.md) 与 `META-TABLE-01`，页眉对应 `META-HEADER-01` 和 `LAYOUT-HEADER-01`。各字段源码位于[示例 metadata.tex](../../examples/source/rc-low-pass-filter/config/metadata.tex)；功能索引还分别保留了 `META-TITLE-01`、`META-AUTHOR-01`、`META-COURSE-01`、`META-GROUP-01`、`META-THEME-01`、`META-STUDENT-01`、`META-PARTNER-01`、`META-STAFF-01`、`META-DATE-01`、`META-PLACE-01`、`META-TIME-01` 和 `META-DUE-01`，可从[示例功能索引](../../examples/FEATURES.md)跳到源码并在 PDF 第 1 页核对。

## TeX Live 2026 的列表写法

模板在 [`report.tex`](../../template/report.tex) 开头使用：

```tex
\DocumentMetadata{lang=ja-JP,tagging=off}
```

这条声明除了设置日文语言并关闭 Tagged PDF，还启用 TeX Live 2026 的新版列表键。模板没有直接加载 `enumitem`。因此，列表选项应以当前模板和示例已经编译验证的写法为准，不要照搬依赖 `enumitem` 的全部语法。

`\DocumentMetadata` 必须保留在 `\documentclass` 之前。把它移动到文档类之后，不属于模板验证过的结构。

### 无序列表 `itemize`

实验目标或条件没有先后顺序时，可以使用 `itemize`：

```tex
\begin{itemize}[leftmargin=2.8em,nosep]
  \item RC 回路の振幅特性を測定する。
  \item 理論値と測定値を比較する。
\end{itemize}
```

`leftmargin=2.8em` 给出明确的左边距，`nosep` 压缩列表上下和条目之间的额外空白。需要保留一定条目间距时，可以把 `nosep` 换成 `itemsep=0.25em`。完整用例在[示例 01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)，功能 ID 为 [`LIST-ITEMIZE-01`](../../examples/FEATURES.md)、[`LIST-KEY-LEFTMARGIN-01`](../../examples/FEATURES.md) 和 [`LIST-KEY-NOSEP-01`](../../examples/FEATURES.md)，效果见 [PDF 第 2–3 页](../../examples/compiled/rc-low-pass-filter.pdf)。同文件中增加条目间距的第二种写法对应 `LIST-ITEMIZE-02`；测量条件和总结列表分别对应 `LIST-ITEMIZE-03`（PDF 第 9 页）与 `LIST-ITEMIZE-04`（PDF 第 19 页）。

### 有序列表 `enumerate`

步骤有明确顺序时使用 `enumerate`。下面的 `label` 把编号设为 `(1)` 形式：

```tex
\begin{enumerate}[label=(\arabic*),leftmargin=3em,itemsep=0.25em]
  \item 回路を構成する。
  \item 入力波形を設定する。
  \item 測定値を記録する。
\end{enumerate}
```

如果需要普通的 `1.` 形式，可以写成 `label=\arabic*.`。两种形式分别对应 [`LIST-ENUMERATE-01`](../../examples/FEATURES.md) 和 [`LIST-ENUMERATE-02`](../../examples/FEATURES.md)；源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)与[示例 05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex)，效果见 PDF 第 10 页和第 19 页。

### 术语列表 `description`

符号、字段或误差来源需要“名称—解释”对应时，使用 `description`：

```tex
\begin{description}[leftmargin=8em,labelwidth=6.5em,nosep]
  \item[$R$] 抵抗値
  \item[$C$] 静電容量
  \item[$f_c$] 遮断周波数
\end{description}
```

`labelwidth` 控制标签区域，`leftmargin` 控制正文开始位置。标签较长时，应同时增大这两个明确长度，并在 PDF 中检查是否重叠。符号和频率区域用例对应 [`LIST-DESCRIPTION-01`](../../examples/FEATURES.md) 与 [`LIST-DESCRIPTION-02`](../../examples/FEATURES.md)，源码见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 PDF 第 6 页；CSV 字段和误差来源还分别对应 `LIST-DESCRIPTION-03`（PDF 第 10 页）和 `LIST-DESCRIPTION-04`（PDF 第 19 页），可在[示例功能索引](../../examples/FEATURES.md)中继续定位。

### 基础 `list`

如果需要完全自行决定标签和长度，也可以使用 LaTeX 的基础 `list` 环境。它不使用上面的方括号键，而是在第二个参数中设置长度：

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

该用例对应 [`LIST-BASIC-01`](../../examples/FEATURES.md)，源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，效果见 PDF 第 6 页。

新版键的实际注意事项是：

- 使用 `2.8em`、`3em`、`8em` 这类明确长度，不使用 `leftmargin=*` 等来自其他列表实现的特殊值。
- `diagrams` 模块为 TeX Live 2026 与 `mdframed` 的内部旧式列表调用做了局部兼容；用户仍只需正常使用 `mdframed`，不要调用补丁中的内部名称。

列表键的启用入口对应 [`DOCMETA-01`](../../examples/FEATURES.md)，源码见[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，可见说明在 PDF 第 1 页。

## `config/` 的公开入口与内部实现

干净模板把配置分成几层：

| 文件 | 使用者可以依赖的入口 | 不应作为公开接口调用的内容 |
| --- | --- | --- |
| [`config/metadata.tex`](../../template/config/metadata.tex) | 各报告信息命令的参数 | 没有内部排版逻辑 |
| [`config/commands.tex`](../../template/config/commands.tex) | 元数据命令和正文中的 `\maketitle` | `\@course`、`\@group` 等带 `@` 的存储命令，以及信息表和页眉的实现细节 |
| [`config/preamble.tex`](../../template/config/preamble.tex) | 核心宏包、页面布局、交叉引用行为和三个可选模块入口 | `\labreport@cref` 等兼容包装、参考文献状态开关和连接词格式的实现 |
| [`config/diagrams.tex`](../../template/config/diagrams.tex) | 启用 TikZ、CircuitikZ、`mdframed` 及已加载的 TikZ 库 | `\labreport@legacyitem` 和 `\mdf@trivlist` 补丁 |
| [`config/listings.tex`](../../template/config/listings.tex) | `listings` 模块及其全局 `\lstset` 样式 | 没有新增的模板命令或环境 |
| [`config/plots.tex`](../../template/config/plots.tex) | `report axis base`、`report legend`、`report axis`、`report plot`、`report dual right axis` | PGFPlots 自己的内部实现 |

核心导言区入口对应 [`MODULE-CORE-01`](../../examples/FEATURES.md)，示例源码为[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，影响 PDF 第 1–21 页。页面布局和页眉页脚分别对应 [`LAYOUT-PAGE-01`](../../examples/FEATURES.md) 与 [`LAYOUT-HEADER-01`](../../examples/FEATURES.md)，同样在示例 report.tex 中标记。

## 可选模块

TikZ／CircuitikZ／`mdframed`、`listings` 和 PGFPlots 分别通过下面三个入口启用：

```tex
\input{config/diagrams}
\input{config/listings}
\input{config/plots}
```

干净模板在 [`config/preamble.tex`](../../template/config/preamble.tex) 中已经放置了带注释的入口，使用时只取消所需行的注释。三个模块对应 [`MODULE-DIAGRAMS-01`](../../examples/FEATURES.md)、[`MODULE-LISTINGS-01`](../../examples/FEATURES.md) 和 [`MODULE-PLOTS-01`](../../examples/FEATURES.md)；入口源码见[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，可见效果分别位于 PDF 第 8–9 页、第 10 与 16–17 页、第 13–15 页。

三个入口作为一组启用的说明还对应 [`MODULES-01`](../../examples/FEATURES.md)，可在示例 PDF 第 1 页与各模块后续的实际用例之间对照。

项目没有定义其他自定义正文环境。需要增加专用命令或第三方宏包时，应检查是否覆盖现有命令；更系统的修改流程见[自行修改模板](13-customization.md)。

---

[← 上一篇：正文与章节结构](06-document-structure.md) | [下一篇：图片、表格、代码与绘图 →](08-figures-tables-and-code.md)
