# 常见问题与故障排除

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/14-troubleshooting.md)

排错时先保留第一次失败产生的 `report.log`。LaTeX 的后续错误常由前面一个缺文件、缺少花括号或未定义命令引起，应当优先处理日志中第一条真正的错误。

在能够直接看到 `report.tex` 的目录运行下面的完整命令，可以让 Latexmk 负责 LuaLaTeX、Biber 和必要的重复编译：

```console
latexmk -lualatex -interaction=nonstopmode -halt-on-error -file-line-error report.tex
```

`-file-line-error` 会尽量显示出错文件和行号，`-halt-on-error` 会在首个致命错误处停止。修正第一处错误后重新运行，直到命令成功；最后仍需检查日志中的非致命排版和字体警告。

## 无法编译

先按下面的顺序缩小范围：

1. 确认当前目录中有 `report.tex`、`config/preamble.tex`、`config/metadata.tex`、`biblatex-jareport.sty` 和 `biblatex-jareport-japanese.lbx`。不要单独编译 `sections/*.tex`。
2. 确认实际调用的是支持的工具链：

   ```console
   latexmk --version
   lualatex --version
   biber --version
   ```

3. 用本篇开头的完整命令编译，记录控制台中第一条带文件名和行号的错误，再查看 `report.log` 的相邻内容。
4. 如果刚修改过宏包、文件名或目录，先撤回最近一类改动；不要同时继续修改其他配置。
5. 只有在保存日志之后才清理辅助文件：

   ```console
   latexmk -c report.tex
   latexmk -lualatex report.tex
   ```

   `latexmk -c` 会清除常见辅助文件并保留 `report.pdf`。它不是修复源码错误的办法；如果清理后仍在同一位置失败，应回到日志检查源码。

常见日志模式可以这样判断：

| 日志文字 | 常见原因 | 首先检查 |
| --- | --- | --- |
| `! LaTeX Error: File '...sty' not found.` | TeX Live 缺少宏包，或本地样式文件被移走 | 用 `kpsewhich` 检查宏包；本项目的两个 `biblatex-jareport` 文件应与 `report.tex` 同层 |
| `Undefined control sequence` | 命令拼错、可选模块未启用、定义文件未读取或读取顺序错误 | 错误行中的命令名称、`config/preamble.tex` 和 `report.tex` 的 `\input` 顺序 |
| `Runaway argument?`、`File ended while scanning use of ...` | 前面缺少 `}`、`]`、`\end{...}` 或参数分隔 | 错误行之前最近修改的环境和命令 |
| `Environment ... undefined` | 所需宏包或模块未启用，或环境名拼错 | 对照[自定义命令与环境](07-commands-and-environments.md)和模块入口 |
| `Emergency stop`、`Fatal error occurred` | 前面已有致命错误 | 向上寻找第一条 `!` 错误，不把这两行当作根因 |
| `Overfull \hbox`、`Overfull \vbox` | 内容超出版心或预留高度 | 日志给出的行号、长 URL、宽表格、代码行、页眉和图片宽度 |

`Overfull` 通常不会让编译命令失败，但表示文字、表格或图像可能超出版心，提交前仍应处理。不要只因为生成了 PDF 就认为排版已经正确。

## 字体缺失或乱码

先确认不是用了错误引擎。模板只支持 LuaLaTeX；pdfLaTeX 或 XeLaTeX 能读入部分源码，也不代表输出受支持。然后检查类文件、LuaTeX-ja 和默认字体是否能被当前 TeX 环境找到：

```console
kpsewhich ltjsarticle.cls
kpsewhich luatexja-preset.sty
kpsewhich HaranoAjiMincho-Regular.otf
```

命令没有输出路径时，说明当前终端使用的 TeX 安装找不到对应文件。若编辑器可以编译而终端不可以，比较两边实际调用的 `latexmk`、`lualatex` 和 `kpsewhich` 路径，不要只比较界面中显示的版本名称。

日志中的 `Missing character:` 表示当前字体没有相应字形，不一定是 UTF-8 文件损坏；先记录具体字符，再判断它是否属于模板支持的日文排版范围。乱码如果只出现在源码编辑器中，则还要确认文件以 UTF-8 保存。大段中文即使部分汉字能够显示，也不属于当前模板的支持范围。

修改过字体预设后出现问题，应先恢复：

```tex
\usepackage[deluxe]{luatexja-preset}
```

不要同时保留两条 `luatexja-preset` 加载。恢复后重新完整编译；若默认配置正常，再按[日文排版与字体](11-languages-and-fonts.md)逐项检查候选预设和字体文件。示例中的日文、欧文与数学混排对应 [`CORE-JAPANESE-01`](../../examples/FEATURES.md)，可以用来判断问题是全局环境还是自己报告中的特殊字符。

## Biber 与参考文献错误

不要用单独运行一次 `lualatex` 或 `biber` 代替 Latexmk。正确入口仍是：

```console
latexmk -lualatex report.tex
```

遇到 `Citation '...' undefined`、要求重新运行 Biber、或参考文献标题没有出现时，依次检查：

1. 正文中是否真的使用了 `\cite{key}` 或 `\nocite{key}`。只把条目写入 `.bib` 不会让它进入文献表。
2. `key` 是否与 `bibliography/references.bib` 中项目种类后面的键完全一致，包括大小写。
3. `.bib` 是否存在缺逗号、波括号不配对、重复键等语法问题。
4. `report.tex` 中是否仍有 `\addbibresource{bibliography/references.bib}`，路径是否相对于主文件目录有效。
5. 是否保留了与 `report.tex` 同层的 `biblatex-jareport.sty` 和 `biblatex-jareport-japanese.lbx`。
6. `report.blg` 中 Biber 的第一条 `WARN` 或 `ERROR` 指向哪个键或字段。

如果控制台提示 Biber 找不到 `report.bcf`，通常是还没有先由 LuaLaTeX 生成控制文件，或者当前目录不对；回到主文件目录让 Latexmk 从头管理流程。若日文条目意外按 IEEE 格式输出，检查是否准确写了：

```bibtex
langid = {japanese}
```

项目只对严格标为 `japanese` 的 11 种项目类型选择日文格式，其他情况回到基础样式。完整数据接口、条件文献表和已知字段回退见[参考文献](10-bibliography.md)；对应示例入口包括 [`BIB-JAPANESE-01`](../../examples/FEATURES.md)、[`BIB-EN-FALLBACK-01`](../../examples/FEATURES.md) 与 [`BIB-CONDITION-01`](../../examples/FEATURES.md)。

## 图片与路径错误

日志出现 `LaTeX Error: File '...' not found.` 时，先确认是在 `report.tex` 所在目录编译。模板的相对路径以这个入口为基础；从上一级目录运行，`config/`、`sections/`、`figures/` 和 `bibliography/` 都可能无法解析。

外部图片的默认目录由下面一行设置：

```tex
\graphicspath{{figures/}}
```

把图片放入 `figures/` 后，可以使用 `\includegraphics{文件名}`。检查文件名时注意大小写、扩展名和全角／半角字符；在一种操作系统上碰巧可用的大小写不一致路径，换到区分大小写的文件系统后可能失败。LuaLaTeX 可以直接处理常见的 PDF、PNG 和 JPEG；SVG 等格式不能仅靠当前配置直接读入。

若文件存在但版面异常，再检查 `width`、旋转和裁剪选项。常见的安全写法是把宽度限制在当前行宽以内：

```tex
\includegraphics[width=0.8\linewidth]{measurement-result.png}
```

宽图、并排子图和 `[H]` 固定浮动位置都可能造成 `Overfull \hbox` 或页面留下大块空白。外部图片、图片目录和子图用例分别对应 [`FIG-IMAGE-01`](../../examples/FEATURES.md)、[`GRAPHICSPATH-01`](../../examples/FEATURES.md) 与 [`FIG-SUBFIGURE-01`](../../examples/FEATURES.md)，可对照示例 PDF 第 8–9 页。

## 编号与交叉引用错误

PDF 中出现 `??`，或日志出现 `Reference ... undefined`、`There were undefined references` 时，先确认 `\label` 与引用键完全一致，再让 Latexmk 完成重复编译。复制章节、公式、图表或清单后，最容易遗漏的是修改重复的 `\label`；日志中的 `multiply defined` 表示至少两个对象使用了同名标签。

标签的位置也会影响结果：图表的 `\label` 应放在 `\caption` 之后；`align` 中每个需要单独引用的行要有自己的标签；章节标签应紧跟对应的 `\section` 或 `\subsection`。多个对象的引用不要手工拼编号，使用模板支持的 `\cref`、`\Cref`、`\crefrange` 和 `\Crefrange`。

模板启用了 `showonlyrefs`，所以带 `\label` 的公式如果从未通过 `\eqref` 或受支持的 Cleveref 命令引用，保持无编号是正常行为。普通、星号和范围形式的兼容逻辑位于 `config/preamble.tex`，不要为解决一个编号问题而再次加载 `cleveref` 或覆盖这些命令。正常行为可对照 [`MATH-SHOWONLYREFS-01`](../../examples/FEATURES.md)、[`REF-STAR-01`](../../examples/FEATURES.md) 和 [`REF-RANGE-01`](../../examples/FEATURES.md)。

引用文字正确但没有彩色边框也不是错误。模板为 `hyperref` 使用 `hidelinks`，链接仍然存在，只是不显示边框。编号策略与各类引用的详细边界见[公式与交叉引用](09-math-and-cross-references.md)。

---

[← 上一篇：自行修改模板](13-customization.md) | [返回文档目录 →](README.md)
