# 日文排版与字体

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/11-languages-and-fonts.md)

模板的排版对象是日文实验报告。这里需要分开理解三个问题：LuaLaTeX 和 `ltjsarticle` 决定日文文档的基本排版方式，`luatexja-preset` 选择日文字体预设，而西文和数学字体仍属于各自的字体系统。只想填写实验内容时无需修改这些设置。

## LuaLaTeX 与 `ltjsarticle`

[`report.tex`](../../template/report.tex) 的开头固定为：

```tex
% !TeX program = lualatex
% TeX Live 2026: enable the new list keys without tagged-PDF output.
\DocumentMetadata{lang=ja-JP,tagging=off}
\documentclass{ltjsarticle}
```

请使用 LuaLaTeX 编译，不要把编辑器的引擎改成 pdfLaTeX 或 XeLaTeX。最稳妥的入口仍是在 `report.tex` 所在目录运行：

```console
latexmk -lualatex report.tex
```

`ltjsarticle` 是 LuaTeX-ja 提供的日文 `article` 类，负责日文字符、换行与版面所需的基础处理。模板没有同时维护其他引擎的等价配置，因此“能够读入某些源文件”不等于其他引擎受支持。

## 文档语言、tagging 与列表键

`\DocumentMetadata` 必须写在 `\documentclass` 之前。模板当前的两个参数含义为：

- `lang=ja-JP` 把文档主语言声明为日文；它不是字体名称，也不会自动选择日文字体。
- `tagging=off` 关闭 tagged PDF 输出。模板当前没有承诺生成可访问性标签结构。

在模板最低支持的 TeX Live 2026 中，这一声明还会启用 LaTeX 新版列表键。示例中的 `leftmargin` 和 `nosep` 分别对应 [`LIST-KEY-LEFTMARGIN-01`](../../examples/FEATURES.md) 与 [`LIST-KEY-NOSEP-01`](../../examples/FEATURES.md)，源码见[示例 01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)，效果见 PDF 第 2 页。这些键调整的是列表缩进和间距，与字体选择无关；不要为了更换字体删除 `\DocumentMetadata`。

文档元数据入口对应 [`DOCMETA-01`](../../examples/FEATURES.md)，示例第 1 页的彩色批注显示其配置位置；`lang=ja-JP` 和 `Tagged: no` 属于 PDF 文档属性，不能只根据页面外观确认。可见的日文混排请按下文的 `CORE-JAPANESE-01` 查看。

## 默认字体预设

核心导言区 [`config/preamble.tex`](../../template/config/preamble.tex) 使用：

```tex
\usepackage[deluxe]{luatexja-preset}
```

在项目支持的 TeX Live 2026 环境中，`luatexja-preset` 没有收到其他预设名称时，默认选择 `haranoaji`，也就是 Harano Aji 字体预设。因此当前模板的日文默认字体是 Harano Aji。`deluxe` 启用预设所提供的多字重日文字体配置，使明朝体、哥特体及强调字重按 LuaTeX-ja 的预设关系工作；它不负责判断文档语言，也不会把西文或数学字体一起替换。

示例把日文、西文、数字与数学放在同一份报告中，覆盖项为 [`CORE-JAPANESE-01`](../../examples/FEATURES.md)。入口见[示例 report.tex](../../examples/source/rc-low-pass-filter/report.tex)，实际混排可在[示例 PDF 第 1–21 页](../../examples/compiled/rc-low-pass-filter.pdf)查看。核心导言区的加载边界另见 [`MODULE-CORE-01`](../../examples/FEATURES.md)，其作用贯穿示例 PDF 第 1–21 页。

### 三类字体的边界

- **日文字体**由 LuaTeX-ja 和 `luatexja-preset` 管理。汉字、平假名、片假名以及日文标点的字体选择主要落在这一层。
- **西文字体**用于拉丁字母等西文内容，默认关系由文档类和 LaTeX 的西文字体设置决定。改变 `luatexja-preset` 的日文预设，不会自动把西文字体换成同名家族。
- **数学字体**用于数学模式中的字母、数字和符号。模板加载 `mathtools` 与 `amssymb` 来提供数学功能，但没有在日文字体预设中绑定一套新的数学字体。`\mathrm{...}` 等命令选择的是数学字体样式，不是日文明朝体。

当前示例 PDF 中，西文正文主要嵌入 Latin Modern 字体，数学内容使用 Computer Modern 与 AMS 数学字体。模板没有用额外配置把这些具体西文和数学字体冻结为公开接口，因此更换文档类、宏包或 TeX 发行版后仍应重新检查。

因此，日文正文、西文变量和公式看起来并不完全相同是正常的。若课程要求三类字体分别采用特定家族，需要分别配置，不能只改一个日文字体选项后假定全部已经更换。

## 更换日文字体

只需要另一种 LuaTeX-ja 预设时，在 [`config/preamble.tex`](../../template/config/preamble.tex) 中替换原有的一行，不要重复加载 `luatexja-preset`。例如把当前默认写得更明确：

```tex
\usepackage[deluxe,haranoaji]{luatexja-preset}
```

或者在确认字体文件可用后选择 IPAex 预设：

```tex
\usepackage[deluxe,ipaex]{luatexja-preset}
```

可以先从终端确认关键字体文件能否由 TeX 找到：

```console
kpsewhich HaranoAjiMincho-Regular.otf
kpsewhich ipaexm.ttf
```

命令输出路径才表示当前 TeX 环境能够找到对应文件。项目的 TeX Live 2026 环境提供 Harano Aji 默认字体，也能找到 IPAex。

为了让源文件更便于移植，建议使用 TeX Live 提供且可嵌入 PDF 的字体。

## 字体故障排除

### 编译器报告文档类或日文命令不可用

先确认执行的是 `latexmk -lualatex report.tex`，并检查：

```console
lualatex --version
kpsewhich ltjsarticle.cls
kpsewhich luatexja-preset.sty
```

缺少文件通常表示 TeX Live 安装不完整，或编辑器使用的 TeX 与终端不是同一套。

### 日文字体找不到或被替代

如果问题在更换预设后出现，先恢复原来的 `\usepackage[deluxe]{luatexja-preset}`。Harano Aji、IPAex 等 TeX Live 字体可以用 `kpsewhich` 检查；依赖操作系统字体的预设则应以实际编译日志和字体解析结果为准。不要同时保留两个 `luatexja-preset` 加载行。

### PDF 中出现缺字方框

缺字通常是所选字体不包含该字符，不一定是 UTF-8 源码损坏。记录具体字符，检查日志中的 missing character 信息，并确认它属于模板支持的日文排版范围。罕见符号需要换用覆盖该字形的字体。注意，非日文 CJK 文本的排版不在模板支持范围内。

### 编辑器能编译，终端不能，或反过来

比较两边实际调用的 `latexmk`、`lualatex` 和 `kpsewhich` 路径。不同 TeX Live 版本可能选择不同预设或找不到相同字体。模板最低支持 TeX Live 2026，排查时应先确认编译器和终端使用同一套、且版本不低于 TeX Live 2026 的 TeX 环境。

---

[← 上一篇：参考文献](10-bibliography.md) | [下一篇：源码、说明与 PDF 对照 →](12-source-doc-pdf-cross-reference.md)
