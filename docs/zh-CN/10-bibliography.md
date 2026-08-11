# 参考文献

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/10-bibliography.md)

模板使用 BibLaTeX 管理正文引用和文末参考文献，并由 Biber 读取 `.bib` 数据。通常只需要填写文献数据、在正文引用，然后继续运行与普通报告相同的 Latexmk 命令。

## 最小引用流程

参考文献数据库已经在 [`report.tex`](../../template/report.tex) 中注册：

```tex
\addbibresource{bibliography/references.bib}
```

先在 [`bibliography/references.bib`](../../template/bibliography/references.bib) 中加入一条日文文献。下面以在线资料为例：

```bibtex
@online{probe-guide,
  author       = {{電子計測資料センター}},
  title        = {受動プローブの補償と入力容量},
  organization = {電子計測資料センター},
  date         = {2025-11-18},
  url          = {https://example.com/measurement/passive-probe},
  urldate      = {2026-08-10},
  langid       = {japanese}
}
```

然后在正文中引用同一个键：

```tex
プローブの入力容量については公開資料を参照した\cite{probe-guide}。
```

最后在 `report.tex` 所在目录运行：

```console
latexmk -lualatex report.tex
```

Latexmk 会在需要时调用 Biber，并重复运行 LuaLaTeX，直到引用和文献表稳定。不要用一次 `lualatex` 或单独运行一次 Biber 代替完整命令。模板的数据库入口对应 [`BIB-RESOURCE-01`](../../examples/FEATURES.md)，源码见[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，参考文献结果见[示例 PDF 第 20–21 页](../../examples/compiled/rc-low-pass-filter.pdf)。

只把条目写入 `.bib` 并不会自动显示它。条目需要在正文中由 `\cite{...}` 引用；如果希望列出但不在正文形成引用，可以使用 `\nocite{...}`，而 `\nocite{*}` 会收录当前数据库中的全部条目，应谨慎使用。引用键区分大小写，必须和 `.bib` 中条目类型后花括号内的键完全一致。

示例还在标题信息前的一句声明中，把当前项目仓库作为首条在线文献引用，用来展示“正文引用—文末条目”的完整连接。该用例对应 [`BIB-REPOSITORY-01`](../../examples/FEATURES.md)，源码见[示例 00-sample-notice.tex](../../examples/source/rc-low-pass-filter/sections/00-sample-notice.tex) 与[示例 references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)，效果见 PDF 第 1、20 页。

## `biblatex-jareport` 的范围

核心导言区使用：

```tex
\usepackage[
  sorting=none,
  bibstyle=ieee,
  citestyle=numeric-comp
]{biblatex-jareport}
```

实际源码写在一行中，以上只是为了看清三个选项。这里的分工是：

- [`biblatex-jareport.sty`](../../template/biblatex-jareport.sty) 是项目维护的本地包装层。它把选项传给 BibLaTeX，保存基础样式的 driver，再对严格标为日文的 11 种条目类型选择项目自己的输出宏。
- [`biblatex-jareport-japanese.lbx`](../../template/biblatex-jareport-japanese.lbx) 是本地日文语言文件。它继承英文的 biblatex 字符串与附加设置，只把 bibliography／references 两个标题设为“参考文献”，不是完整的日文本地化文件。
- `bibstyle=ieee` 是非日文条目的基础参考文献样式；`citestyle=numeric-comp` 使用数字引用并压缩连续编号；`sorting=none` 保持引用出现的顺序。

这两个本地文件由项目的发布 ZIP 随模板一起提供，不是需要另外从 CTAN 安装的依赖。在本项目的标准目录中，它们与 `report.tex` 位于同一层；移动模板时如果漏掉 `.sty` 或 `.lbx`，LaTeX 会报告找不到文件或语言映射。

项目样式的编排规则只**参考 SIST 02-2007**，但不表示或保证完全符合该标准。

## `langid={japanese}` 必须严格填写

11 个包装 driver 都使用同一项严格判断：只有字段值恰好为 `japanese` 时，才进入日文格式。

```bibtex
langid = {japanese}
```

漏写 `langid`、写成其他语言值，或用别的拼写，都不会自动识别为日文。此时条目会回到所选基础样式，即当前的 IEEE driver。反过来，如果给英文条目错误地写上 `langid={japanese}`，它也会进入项目的日文格式。模板不会根据标题文字或作者姓名自动判断语言。

这一选择行为对应 [`BIB-JAPANESE-01`](../../examples/FEATURES.md)，说明源码见[示例 07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)，日文条目数据见[示例 references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)，效果见 PDF 第 20–21 页。

## 11 种日文 driver 与实际字段

项目为 `article`、`book`、`inbook`、`incollection`、`inproceedings`、`misc`、`online`、`proceedings`、`report`、`thesis`、`unpublished` 提供日文分支。下面列出各分支实际尝试输出的主要字段和常见用途。字段缺失时，日文分支可能省略该项，也可能按下表规则回退到其他字段。Biber 可能报告部分数据问题，但本项目的日文分支不会判断条目是否满足课程、学会或 SIST 的完整性要求。

| 类型 | 常见用途 | 日文分支实际使用的主要字段 |
| --- | --- | --- |
| `article` | 期刊论文 | `author`（缺少时尝试 `editor`）、`title`、`journaltitle`、`year`、`volume`、`number`、`pages`、`eid`、`doi`、`url`、`urldate` |
| `book` | 独立成册的书籍 | `author`／`editor`、`title`、`edition`、`location`、`publisher`、`year`、`pagetotal`（缺少时尝试 `pages`）、`series`、`number`、`isbn`、`url`、`urldate` |
| `inbook` | 书中的一章或一部分 | `author`、`title`、`booktitle`（必要时回退到 `maintitle` 或 `title`）、`editor`、`edition`、`location`、`publisher`、`year`、`pages`、`series`、`number`、`isbn`、`url`、`urldate` |
| `incollection` | 编辑文集中的独立文章 | 使用与日文 `inbook` 相同的项目格式，经典字段为 `author`、`title`、`booktitle`、`editor`、出版信息、`pages`、`url`、`urldate` |
| `inproceedings` | 会议论文或讲演论文 | `author`、`title`、`booktitle`／`eventtitle`、`editor`、`venue`、`eventdate`、`organization`、`publisher`、`year`、`pages`、`series`、`number`、`url`、`urldate` |
| `proceedings` | 整本会议录 | `editor`（缺少时尝试 `author`）、`title`、`venue`、`eventdate`、`organization`、`publisher`、`year`、`pagetotal`、`url`、`urldate` |
| `report` | 技术报告、机构报告 | `author`、`title`、`location`、`institution`（必要时回退到 `organization`）、`year`、`number`、`pagetotal`、`url`、`urldate` |
| `thesis` | 学士、硕士或博士论文 | `author`、`title`、`location`、`institution`（必要时回退到 `organization`）、`year`、`pagetotal`、`type`、`url`、`urldate`；缺少 `type` 时显示“学位論文” |
| `online` | 网页、在线说明、在线数据库 | `author`／`editor`、`title`、`organization`（必要时回退到 `publisher`）、`date`、`url`、`urldate` |
| `misc` | 技术资料等难以归入其他类型的资料 | `author`、`title`、`howpublished`、`year`、`note`、`url`、`urldate` |
| `unpublished` | 未刊行稿、内部资料 | `author`、`title`、`journaltitle`、`year`、`note`、`url`、`urldate` |

`inbook` 与 `incollection` 的日文分支共用同一个项目宏，但非日文条目仍分别回到基础样式原本的 driver。`inbook` 和 `incollection` 应填写 `booktitle`；`inproceedings` 应填写 `booktitle` 或 `eventtitle`。容器标题缺失时退回 `title` 只是当前实现的兜底行为，可能使同一标题输出两次，不是推荐填法。

除 `proceedings` 的专门处理外，各日文分支在缺少 `author` 时会尝试使用 `editor`。对之后还会单独输出容器编辑者的 `inbook`、`incollection` 和 `inproceedings`，缺少作者时可能重复输出同一编辑者。会议类型中，`organization` 与 `publisher` 不同时会依次输出两者，相同时只输出一次；缺少 `organization` 时也只输出一次 `publisher`。这些情况仍应根据目标格式填写，并在编译后检查。

上表中 `book`、`inbook`、`incollection` 和 `inproceedings` 的 `number` 只有在同时存在 `series` 时才会显示。`urldate` 也不会单独输出，只在条目同时包含 `url` 时才会显示。`online` 使用完整 `date`，会议类型还会使用 `eventdate`，多数其他日文 driver 只输出年份。总页数使用 `pagetotal`，文章页码使用 `pages`。这些是当前实现行为，不应推广为所有 biblatex 样式的通用规则。

11 种 driver 的汇总对应 [`BIB-DRIVERS-01`](../../examples/FEATURES.md)。各类型的示例数据与 PDF 页码为：

| 类型 | 稳定功能 ID | 示例 PDF 页 |
| --- | --- | --- |
| `book` | `BIB-JA-BOOK-01` | 20 |
| `article` | `BIB-JA-ARTICLE-01` | 20 |
| `inbook` | `BIB-JA-INBOOK-01` | 20 |
| `incollection` | `BIB-JA-INCOLLECTION-01` | 20 |
| `inproceedings` | `BIB-JA-INPROCEEDINGS-01` | 20 |
| `proceedings` | `BIB-JA-PROCEEDINGS-01` | 21 |
| `report` | `BIB-JA-REPORT-01` | 20 |
| `thesis` | `BIB-JA-THESIS-01` | 20 |
| `online` | `BIB-JA-ONLINE-01` | 20 |
| `misc` | `BIB-JA-MISC-01` | 20 |
| `unpublished` | `BIB-JA-UNPUBLISHED-01` | 21 |

所有源码都在[示例 references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)，编译结果见[示例 PDF](../../examples/compiled/rc-low-pass-filter.pdf)。

## 非日文条目与 IEEE 回退

对 `langid` 不等于 `japanese` 的上述条目类型，包装层会调用加载时保存的基础 driver。不在上述 11 种包装 driver 内的条目类型，从一开始就沿用基础样式。当前 `bibstyle=ieee`，因此示例中的英文 `article` 按 IEEE 参考文献格式输出，而不是套用日文标点与字段顺序。

英文回退对应 [`BIB-EN-FALLBACK-01`](../../examples/FEATURES.md)，说明见[示例 07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)，数据见[示例 references.bib](../../examples/source/rc-low-pass-filter/bibliography/references.bib)，效果见 PDF 第 21 页。这里的“回退”只表示保留所选基础样式，不表示模板对所有语言分别提供了本地化 driver。

## 条件打印与引用编号

[`config/preamble.tex`](../../template/config/preamble.tex) 使用 `\AtDataInput`：当 biblatex 从 Biber 生成的 `.bbl` 中读入至少一条条目时，将内部状态设为真；[`sections/07-references.tex`](../../template/sections/07-references.tex) 只在该状态成立时执行：

```tex
\printbibliography[title={参考文献}]
```

因此，空白模板不会出现没有内容的“参考文献”标题，只向 `.bib` 添加未引用条目也不会显示文献表。正文中的 `\cite` 或 `\nocite` 会使相应条目被纳入当前参考文献处理流程。该行为对应 [`BIB-CONDITION-01`](../../examples/FEATURES.md) 和 [`BIB-PRINT-01`](../../examples/FEATURES.md)，源码见[示例 07-references.tex](../../examples/source/rc-low-pass-filter/sections/07-references.tex)，效果见 PDF 第 20–21 页。

引用采用 `citestyle=numeric-comp`。多个连续文献编号会压缩为范围，对应 [`BIB-COMPRESS-01`](../../examples/FEATURES.md)；实际的连续引用可见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)。`sorting=none` 使编号按首次引用顺序排列。URL 与参照日期用例对应 [`BIB-URL-01`](../../examples/FEATURES.md)，可在示例 07-references.tex 的说明和 PDF 第 20 页核对。

## 常见错误

### 文中出现未解析引用

如果 PDF 中出现未解析标记或日志报告未定义引用，依次检查：

1. `\cite{key}` 的 `key` 是否与 `.bib` 条目键完全一致。
2. `.bib` 是否有缺少逗号、花括号不配对、重复键等语法问题。
3. 是否从 `report.tex` 所在目录运行 `latexmk -lualatex report.tex`。
4. `biber --version` 是否能正常返回版本。

修改后重新运行完整 Latexmk 命令，不要只删除 PDF 或只执行一次 LuaLaTeX。

### 日文条目显示为 IEEE 格式

检查条目是否严格包含 `langid={japanese}`。标题和作者使用日文并不会触发日文 driver；其他拼写也不会被自动归一化。

### 有数据但没有参考文献标题

确认正文存在 `\cite{...}` 或 `\nocite{...}`。只有 `.bib` 数据、但没有通过 `\cite` 或 `\nocite` 纳入当前参考文献处理流程时，条件打印仍保持关闭。

### 某个字段没有显示

先确认条目类型是否正确，再对照上面的 driver 字段表。字段存在于 `.bib` 并不表示当前 driver 一定输出它；例如日文 `online` 使用 `organization`、`date`、`url`、`urldate`，并不会显示任意附加字段。

### 找不到样式或语言文件

确认 `biblatex-jareport.sty`、`biblatex-jareport-japanese.lbx` 与 `report.tex` 位于同一层，并且没有只移动主文件。参考文献数据包含日文时应保存为 UTF-8，并使用 Biber 而不是传统 BibTeX 处理。


---

[← 上一篇：公式与交叉引用](09-math-and-cross-references.md) | [下一篇：日文排版与字体 →](11-languages-and-fonts.md)
