# 源码、说明与 PDF 对照

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/12-source-doc-pdf-cross-reference.md)

模板把同一个功能放在三处说明：可复制的示例源码、功能清单 [`examples/FEATURES.md`](../../examples/FEATURES.md)，以及编译后的[示例 PDF](../../examples/compiled/rc-low-pass-filter.pdf)。稳定功能 ID 把三处连接起来。

## 示例源码的标注方式

当前示例源码位于 [`examples/source/rc-low-pass-filter/`](../../examples/source/rc-low-pass-filter/)，是一份 21 页的虚构 RC 低通滤波器实验报告。标注紧邻最能代表该功能的入口或用例，例如：

```tex
% [DOC:LIST-ITEMIZE-01] Itemized experimental objectives.
\begin{itemize}[leftmargin=2.8em,nosep]
  \item RC 低域通過フィルタの振幅特性と位相特性を測定する。
\end{itemize}
```

默认编译结果还会在相关效果附近插入彩色日文说明，并在灰色代码框中直接显示实际使用的源码片段。说明采用较简单的 `です・ます` 语气；需要了解更多细节时，再沿功能 ID 查看本说明文档和完整源码。

彩色说明和源码框由 [`config/sample-guide.tex`](../../examples/source/rc-low-pass-filter/config/sample-guide.tex) 中的 `\SampleGuideNote` 与 `\SampleGuideSource` 生成，只属于示例 PDF，不是模板功能。将示例 `report.tex` 中的 `\showsampleguidetrue` 改为 `\showsampleguidefalse`，可以隐藏说明层，只编译黑色实验正文。

## PDF 页码与效果索引

[`examples/FEATURES.md`](../../examples/FEATURES.md) 按七个功能组整理所有 marker。下表给出每组的阅读入口；ID 只列代表项，完整清单、逐项源码链接和准确页码以功能清单为准。

| 功能组 | 代表功能 ID | 主要示例源码 | PDF 范围 |
| --- | --- | --- | --- |
| 文档入口与元数据 | `NOTICE-01`、`DOCMETA-01`、`MODULE-CORE-01`、`META-TITLE-01`、`LAYOUT-HEADER-01`、`CORE-JAPANESE-01`、`COLOR-ANNOTATION-01`、`GUIDE-SOURCE-01`、`GUIDE-TOGGLE-01` | [report.tex](../../examples/source/rc-low-pass-filter/report.tex)、[metadata.tex](../../examples/source/rc-low-pass-filter/config/metadata.tex)、[sample-guide.tex](../../examples/source/rc-low-pass-filter/config/sample-guide.tex)、[00-sample-notice.tex](../../examples/source/rc-low-pass-filter/sections/00-sample-notice.tex) | 1–21 |
| 结构与列表 | `STRUCT-SECTION-01`、`LIST-ITEMIZE-01`、`LIST-ENUMERATE-01`、`LIST-DESCRIPTION-01`、`LIST-BASIC-01` | [01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)、[02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 2–3、6、9–10、19 |
| 数学与交叉引用 | `MATH-EQUATION-01`、`MATH-ALIGN-01`、`REF-SECTION-01`、`REF-RANGE-01`、`REF-MULTI-01`、`LINK-HYPERREF-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 3–5、8–9、15 |
| 表格、图像与装置图 | `TABLE-TABULARX-01`、`TABLE-MULTIROW-01`、`FIG-IMAGE-01`、`FIG-SUBFIGURE-01`、`DIAG-CIRCUIT-01`、`FLOAT-H-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 6–15 |
| 数据与 PGFPlots | `PLOT-DATAFILE-01`、`PLOT-SINGLE-01`、`PLOT-DUAL-01`、`PLOT-BASE-01` | [04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)、[frequency-response.csv](../../examples/source/rc-low-pass-filter/data/frequency-response.csv) | 13–15 |
| 代码清单 | `CODE-INLINE-01`、`CODE-BLOCK-01`、`CODE-HIGHLIGHT-01`、`CODE-WRAP-01`、`CODE-REF-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)、[05-discussion.tex](../../examples/source/rc-low-pass-filter/sections/05-discussion.tex) | 10、16–17 |
| 参考文献 | `BIB-RESOURCE-01`、`BIB-JAPANESE-01`、`BIB-DRIVERS-01`、`BIB-EN-FALLBACK-01`、`BIB-COMPRESS-01` | [07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)、[references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib) | 20–21；引用也见正文 |

---

[← 上一篇：日文排版与字体](11-languages-and-fonts.md) | [下一篇：自行修改模板 →](13-customization.md)
