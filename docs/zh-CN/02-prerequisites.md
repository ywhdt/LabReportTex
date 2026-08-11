# 环境与依赖

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/02-prerequisites.md)

## LaTeX 发行版

最低支持 **TeX Live 2026**。模板使用 LuaLaTeX 处理日文，因此不应改用 pdfLaTeX 或 XeLaTeX 编译。已经安装 TeX Live 2026 的完整方案时，模板所需的常用宏包通常已包含在内；如果使用精简安装，请通过 TeX Live 的包管理工具补齐缺少的宏包，而不是把宏包文件复制到报告目录中。

模板直接加载 `luatexja-preset`、`graphicx`、`geometry`、`tabularx`、`fancyhdr`、`subcaption`、`multirow`、`booktabs`、`float`、`mathtools`、`amssymb`、`xcolor`、`biblatex`、`hyperref` 和 `cleveref` 等组件。TeX Live 2026 的完整方案通常已经包含这些组件；精简安装需要按编译日志补齐。TikZ、电路图、带框内容、代码清单和 PGFPlots 是可选模块，只有在后续按需启用时才需要对应组件。

## 编译工具

每份报告至少需要以下三个命令：

- **LuaLaTeX**：实际进行排版的引擎；
- **latexmk**：负责按正确次数调用 LuaLaTeX，并在需要时调用 Biber；
- **Biber**：处理 `bibliography/references.bib` 中的参考文献。

在终端中运行下面的命令，确认它们均能返回版本信息：

```console
lualatex --version
latexmk -v
biber --version
```

只要其中一个命令显示“command not found”或类似错误，就先修复安装或 `PATH` 配置。模板的标准编译方式是 `latexmk -lualatex report.tex`；不建议手工混用不同引擎或只运行一次 `lualatex`，否则引用和参考文献可能不会更新。

## 所需宏包

不需要在 `report.tex` 中逐个添加宏包。干净模板已经在 [config/preamble.tex](../../template/config/preamble.tex) 中集中加载核心宏包，根文件会自动读取该配置。首次编译时如果日志明确指出某个外部宏包的 `.sty` 文件缺失，请使用 TeX Live 的包管理工具安装提供该文件的宏包，然后从同一目录重新运行标准编译命令。

`biblatex-jareport.sty` 和 `biblatex-jareport-japanese.lbx` 是 Release ZIP 自带的本地参考文献样式文件，不是需要另外安装的同名 TeX Live 宏包。如果其中之一缺失，应从原 ZIP 恢复，并保持它们与 `report.tex` 位于同一层目录。

可选模块默认处于关闭状态：

```tex
% \input{config/diagrams}
% \input{config/listings}
% \input{config/plots}
```

上面的三行来自 [config/preamble.tex](../../template/config/preamble.tex)。在还没有使用相应功能时，请保持注释状态。如何使用这些模块将在后续文档说明，快速开始不需要启用它们。

## 编辑器配置

任意能够调用本机 TeX Live 的编辑器都可以使用，例如 TeXstudio、VS Code 配合 LaTeX Workshop，或直接使用终端。关键不是编辑器名称，而是编译器设置必须使用 LuaLaTeX，并且项目根文件是 `report.tex`。

如果编辑器会让你选择构建配方，优先选择等价于下面命令的配置：

```console
latexmk -lualatex report.tex
```


## 环境检查

完成安装后，在一个临时目录或 Release ZIP 解压出的模板目录中运行：

```console
latexmk -lualatex report.tex
```

成功后，当前目录会生成 `report.pdf`。即使尚未填写内容，空模板也应能生成 PDF。

若命令失败，先查看同目录的 `report.log` 中最先出现的错误。常见原因是没有使用 LuaLaTeX、缺少 TeX Live 宏包，或在错误的目录执行命令。确认环境后，继续阅读[快速开始](03-quick-start.md)。

---

[← 上一篇：项目概述](01-overview.md) | [下一篇：快速开始 →](03-quick-start.md)
