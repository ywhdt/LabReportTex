# 项目目录结构

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/04-project-structure.md)

## 下载后的目录

从 Releases 下载并解压后，会得到一个可以独立编译的干净模板目录。版本号会随 Release 变化，目录结构保持一致：

```text
LabReportTex-template-v<version>/
├── VERSION
├── LICENSE
├── README.md
├── report.tex
├── biblatex-jareport.sty
├── biblatex-jareport-japanese.lbx
├── config/
├── sections/
├── figures/
└── bibliography/
```

打包时会从 `template/` 逐一复制允许发布的文件，并在 ZIP 中生成 `VERSION`；因此开发树的 `template/` 中没有这个文件。`figures/` 在干净模板中是预留的空目录，可以直接向其中加入图片。

编译时应当进入这一层目录，再运行：

```console
latexmk -lualatex report.tex
```

## 平时需要修改的文件

第一次使用时，主要会接触以下位置：

| 路径 | 作用 | 通常是否修改 |
| --- | --- | --- |
| [`report.tex`](../../template/report.tex) | 报告入口，决定配置和章节的读取顺序 | 只在增删或重排章节时修改 |
| [`config/metadata.tex`](../../template/config/metadata.tex) | 标题、作者、课程、学号和实验日期等信息 | 每份报告都要填写 |
| [`sections/`](../../template/sections) | 各章节的正文 | 每份报告都要填写 |
| `figures/` | 外部图片文件 | 使用图片时添加 |
| [`bibliography/references.bib`](../../template/bibliography/references.bib) | BibLaTeX 参考文献数据 | 使用参考文献时填写 |

`report.tex` 已经通过 `\graphicspath{{figures/}}` 指定图片目录，也通过 `\addbibresource{bibliography/references.bib}` 指定参考文献数据库。因此，只要保留默认目录关系，正文中不需要反复写完整路径。

## `config/` 中的设置

`config/` 保存模板配置，通常不需要在第一份报告中全部修改：

- [`metadata.tex`](../../template/config/metadata.tex)：填写每份报告的信息。
- [`preamble.tex`](../../template/config/preamble.tex)：核心宏包、页面尺寸、交叉引用和可选模块入口。
- [`commands.tex`](../../template/config/commands.tex)：标题信息表、页眉和页脚等模板命令。
- [`diagrams.tex`](../../template/config/diagrams.tex)：TikZ、电路图与带框内容支持。
- [`listings.tex`](../../template/config/listings.tex)：代码清单模块。
- [`plots.tex`](../../template/config/plots.tex)：PGFPlots 曲线图模块。

后三个模块默认没有启用。需要使用时，应当在 `config/preamble.tex` 中取消对应 `\input` 行的注释，而不是把整段配置复制进正文。具体用法将在[图片、表格、代码与绘图](08-figures-tables-and-code.md)中说明。

## `sections/` 中的正文

模板将正文拆分为七个文件：

```text
sections/
├── 01-purpose.tex
├── 02-theory.tex
├── 03-method.tex
├── 04-results.tex
├── 05-discussion.tex
├── 06-summary.tex
└── 07-references.tex
```

这些数字方便观察默认顺序，真正决定输出顺序的是 `report.tex` 中各条 `\input` 的排列。正文文件只写章节内容，不需要再次声明文档类型，也不应单独加入 `\begin{document}`。

## Release ZIP 与 GitHub 仓库的区别

Release ZIP 只包含干净模板。GitHub 仓库还会提供：

- `docs/`：这套中文和日文说明。
- `examples/`：完整示例源码和编译后的 PDF。
- `template/`：与 Release 模板对应的源码，便于在线查看。

公开仓库不包含项目内部使用的检查脚本和测试文件。普通使用者通常只需下载 Release ZIP；希望了解实现方式时，再对照仓库中的说明、示例和模板源码。

## 移动文件时的注意事项

模板使用相对路径组织文件。可以在不改变引用关系的情况下重命名整个模板根目录，但不要只移动 `report.tex`，也不要随意拆散 `config/`、`sections/`、`figures/` 和 `bibliography/`。根目录中的 `biblatex-jareport.sty` 与 `biblatex-jareport-japanese.lbx` 也应保持在 `report.tex` 同层。如果确实要调整目录，需要同步修改 `report.tex` 中相应的 `\input`、`\graphicspath`、`\addbibresource` 或 TeX 文件搜索路径。

---

[← 上一篇：快速开始](03-quick-start.md) | [下一篇：报告基本信息 →](05-metadata.md)
