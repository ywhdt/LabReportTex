# 正文与章节结构

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/06-document-structure.md)

## 主文件入口

[`report.tex`](../../template/report.tex) 是整份报告的入口。它依次读取模板配置、报告信息和正文文件：

下面只保留了正文组织相关的行；完整文件开头还包含 `\DocumentMetadata`、文档类、参考文献数据库和图片路径设置。

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


## 默认章节

默认结构对应常见的日文实验报告：

| 文件 | 默认标题 | 主要用途 |
| --- | --- | --- |
| [`01-purpose.tex`](../../template/sections/01-purpose.tex) | `目的` | 说明实验要确认的对象和目标 |
| [`02-theory.tex`](../../template/sections/02-theory.tex) | `原理` | 整理实验涉及的公式、模型和定义 |
| [`03-method.tex`](../../template/sections/03-method.tex) | `実験方法` | 记录装置、条件和操作过程 |
| [`04-results.tex`](../../template/sections/04-results.tex) | `実験結果` | 放置测量结果、表格和图 |
| [`05-discussion.tex`](../../template/sections/05-discussion.tex) | `考察` | 分析结果、误差和现象 |
| [`06-summary.tex`](../../template/sections/06-summary.tex) | `まとめ` | 简要汇总实验得到的结果 |
| [`07-references.tex`](../../template/sections/07-references.tex) | 条件生成的 `参考文献` | 在存在引用条目时输出文献表 |

这些名称是一套起始结构，可以根据需要修改 `\section{...}` 中的标题，也可以增加、删除或重排章节。

前六个正文文件直接包含 `\section`。`07-references.tex` 没有字面上的 `\section`；条件成立时，`\printbibliography` 会生成“参考文献”标题。

## 编写章节正文

章节文件已经包含 `\section`，可以从下一行直接写正文：

```tex
\section{目的}

本実験では，RC低域通過フィルタの周波数特性を測定し，
遮断周波数付近における利得と位相差の変化を確認する。
```

正文中可以继续使用 `\subsection`。需要交叉引用时，为章节、公式、图或表添加 `\label`，再使用模板支持的 `\cref`；详细规则见[公式与交叉引用](09-math-and-cross-references.md)。

## 添加章节

例如需要在总结后加入附录，可以创建 `sections/appendix.tex`：

```tex
\section{付録}

ここに追加の測定結果を示す。
```

然后在 `report.tex` 中决定它的输出位置：

```tex
\input{sections/06-summary}
\input{sections/appendix}
\input{sections/07-references}
```


## 删除或调整章节

暂时不需要某一章时，可以删除或注释对应的 `\input` 行：

```tex
% \input{sections/06-summary}
```

正文文件可以继续保留，便于以后恢复。要调整章节顺序，只需移动 `report.tex` 中的 `\input` 行。

## 连续排版与目录

模板面向篇幅不太长的实验报告，因此默认不生成目录，也没有在章节之间插入 `\clearpage`、`\newpage` 等强制换页命令。`\maketitle` 只生成基本信息表，不会创建单独占满一页的封面；正文从同一页继续排版，后续章节也会按照剩余空间自然衔接。如果页面空间不足，LaTeX 仍会正常换页。

如有需要，可自行加入相应命令。

## 参考文献章节

`sections/07-references.tex` 使用条件判断：有效条目被 Biber 载入时才执行 `\printbibliography`，通常由正文中的 `\cite` 或 `\nocite` 触发。因此，只向 `.bib` 文件加入未引用条目不会显示文献表，空白模板也不会出现没有内容的“参考文献”标题。

使用参考文献时，需要同时：

1. 在 `bibliography/references.bib` 中加入条目。
2. 在正文中使用 `\cite{...}` 引用该条目；需要列出但不在正文引用时，可以使用 `\nocite{...}`。
3. 通过 `latexmk -lualatex report.tex` 编译，让 Latexmk 自动调用 Biber 并完成必要的重复编译。

参考文献数据和项目样式将在[参考文献](10-bibliography.md)中详细说明。

## 与完整示例对照

[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex) 使用相同的章节拆分方式。可以从[示例功能索引](../../examples/FEATURES.md)中的 `STRUCT-SECTION-01`、`TITLEBLOCK-01` 和 `BIB-CONDITION-01` 找到源码与 PDF 页码，再对照[编译后的示例 PDF](../../examples/compiled/rc-low-pass-filter.pdf)。

---

[← 上一篇：报告基本信息](05-metadata.md) | [下一篇：自定义命令与环境 →](07-commands-and-environments.md)
