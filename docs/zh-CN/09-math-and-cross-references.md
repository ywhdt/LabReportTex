# 公式与交叉引用

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/09-math-and-cross-references.md)

模板默认加载 `mathtools`、`amssymb`、`hyperref` 和 `cleveref`，因此普通公式、对齐推导、数学符号与带链接的交叉引用都不需要额外启用模块。本篇只说明模板已经采用的接口和编号策略，不展开这些宏包的全部功能。

## 单个公式与多行推导

一个独立公式使用 `equation`，并把 `\label` 放在环境内部：

```tex
\begin{equation}
  Z_C=\frac{1}{j\omega C}
  \label{eq:capacitor}
\end{equation}
```

该用例对应 [`MATH-EQUATION-01`](../../examples/FEATURES.md)，源码见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 [PDF 第 3 页](../../examples/compiled/rc-low-pass-filter.pdf)。

需要让多行等号对齐时，使用 `align`，在每个需要单独引用的行内放置自己的 `\label`：

```tex
\begin{align}
  H(j\omega)
    &\coloneqq\frac{V_{\mathrm{out}}}{V_{\mathrm{in}}}
      \label{eq:definition}\\
    &=\frac{1}{1+j\omega RC}.
      \label{eq:transfer}
\end{align}
```

`&` 标出对齐位置，行末的 `\\` 开始下一行。完整的四行推导对应 [`MATH-ALIGN-01`](../../examples/FEATURES.md)，源码仍在[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 PDF 第 4 页。幅值、利得和相位的三行公式对应 [`MATH-FREQUENCY-01`](../../examples/FEATURES.md)，也位于第 4 页。

## `mathtools` 与 `amssymb` 中实际使用的功能

模板通过 `mathtools` 继承常用的 AMS 数学能力，并在示例中实际使用：

- `\coloneqq` 表示“定义为”，对应 [`MATH-COLONEQQ-01`](../../examples/FEATURES.md)，源码见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 PDF 第 4 页。
- `dcases` 用于带较舒展数学样式的分段公式，对应 [`MATH-DCASES-01`](../../examples/FEATURES.md)，同一源码文件，效果见 PDF 第 4 页。
- `\lvert ... \rvert`、`\frac`、`\sqrt` 等用于幅值、比例和根式；示例的组合写法集中在 `MATH-FREQUENCY-01`。

分段公式可以写成：

```tex
\begin{equation}
  G(f)\simeq
  \begin{dcases}
    0, & f\ll f_c,\\
    -20\log_{10}\!\left(\frac{f}{f_c}\right), & f\gg f_c.
  \end{dcases}
  \label{eq:asymptotic-gain}
\end{equation}
```

`amssymb` 在示例中提供 `\mathbb`，例如 `s\in\mathbb{C}` 和 `f\in\mathbb{R}_{>0}`。该用例对应 [`MATH-AMSSYMB-01`](../../examples/FEATURES.md)，源码见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 PDF 第 3 页。

## 标签与 `\eqref`

标签名称不会显示在 PDF 中，但应保持唯一并反映对象类型。项目示例采用下面的前缀：

- `eq:`：公式，例如 `eq:transfer`
- `fig:`：图和子图，例如 `fig:gain-response`
- `tab:`：表格，例如 `tab:equipment`
- `sec:`、`subsec:`：章节，例如 `sec:theory`
- `lst:`：代码清单，例如 `lst:analysis`

前缀不是 LaTeX 的硬性要求，也不决定 Cleveref 输出的对象类型；类型由 `\label` 所在的公式、图、表、章节或清单计数器决定。前缀的作用是减少跨文件重名并方便维护。复制公式、图表或章节时，必须同时修改标签；同名 `\label` 会造成引用指向不确定。图表标签应放在 `\caption` 之后，`align` 中则为每个需要引用的行分别放置标签。

只需要公式编号、不需要自动输出日文“式”字时，可以使用 `\eqref`：

```tex
式\eqref{eq:capacitor}からインピーダンスを求める。
```

`\eqref{eq:capacitor}` 输出带括号的编号。模板的 `showonlyrefs` 会识别这种引用，因此该公式会得到编号。若希望模板自动判断对象类型并输出“式”“图”“表”等名称，应使用下一节的 `\cref`。

## `showonlyrefs`：只给引用过的公式编号

[`config/preamble.tex`](../../template/config/preamble.tex) 中启用了：

```tex
\mathtoolsset{showonlyrefs=true}
```

它只影响 AMS 数学环境的公式编号，不影响章节、图、表或代码清单。使用 `equation` 或 `align` 并不保证一定显示编号：公式被 `\eqref` 或模板支持的 `cleveref` 命令引用后才显示编号，未引用公式保持无编号。

示例特意放置了一条带 `\label`、但正文从未引用的时定数公式。它对应 [`MATH-SHOWONLYREFS-01`](../../examples/FEATURES.md)，源码见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，PDF 第 5 页可看到该公式没有编号，而附近被引用的公式仍有编号。

这个策略适合实验报告：推导中可以继续使用结构清楚的 `equation`、`align`，只有正文实际讨论的公式占用编号。

## `\cref` 与 `\Cref`

模板为 `cleveref` 设置了日文对象名称和标点。普通写法是：

```tex
\cref{eq:transfer}に伝達関数を示す。
\cref{fig:gain-response}に測定結果を示す。
\cref{tab:equipment}に使用機器を示す。
```

`\cref` 根据标签类型自动生成“式”“図”“表”“節”或“リスト”等名称。模板还使用 `noabbrev`，不采用缩写形式。该行为对应 [`REF-NAME-01`](../../examples/FEATURES.md)，源码见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 PDF 第 4 页。章节引用对应 [`REF-SECTION-01`](../../examples/FEATURES.md)，源码见[示例 01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)，效果见 PDF 第 3 页。

句子从引用开始时可以使用大写形式 `\Cref`：

```tex
\Cref{eq:transfer}の絶対値を求める。
```

日文对象名称本身没有英语式大小写变化，但 `\Cref` 仍保留“句首形式”的接口，便于与其他语言和对象类型保持一致。该用例对应 [`REF-CAPITAL-01`](../../examples/FEATURES.md)，效果见 PDF 第 4 页。

模板为 `cleveref` 使用 `nameinlink`，因此普通引用的对象名称和编号整体都是可点击链接；`hyperref` 的 `hidelinks` 隐藏链接边框，但链接仍存在。该效果对应 [`LINK-HYPERREF-01`](../../examples/FEATURES.md)，源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，效果见 PDF 第 9 页。

## 范围、星号和多标签引用

连续对象用 `\crefrange{起始标签}{结束标签}`，句首形式使用 `\Crefrange`：

```tex
\crefrange{eq:definition}{eq:transfer}
\Crefrange{eq:cutoff-min}{eq:cutoff-max}
```

普通范围和句首范围分别对应 [`REF-RANGE-01`](../../examples/FEATURES.md) 与 [`REF-RANGE-CAPITAL-01`](../../examples/FEATURES.md)，源码见[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 PDF 第 4–5 页。子图范围使用相同接口，对应 [`REF-SUBFIG-RANGE-01`](../../examples/FEATURES.md)，源码见[示例 03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex)，效果见 PDF 第 8 页。

如果只想输出引用文字，不创建超链接，在命令名后加星号：

```tex
\cref*{eq:transfer}
\Cref*{eq:transfer}
\crefrange*{eq:definition}{eq:transfer}
\Crefrange*{eq:definition}{eq:transfer}
```

单个星号形式对应 [`REF-STAR-01`](../../examples/FEATURES.md)，范围星号形式对应 [`REF-RANGE-STAR-01`](../../examples/FEATURES.md)，源码都在[示例 02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)，效果见 PDF 第 5 页。星号只改变链接，不改变 `showonlyrefs` 对“已引用公式”的判断；直接传给这些命令的公式标签仍应显示编号。

多个标签可以放在一个 `\cref` 的参数中，用逗号分隔：

```tex
\cref{tab:frequency-response,tab:repeatability,fig:gain-residual}
```

模板加载 `cleveref` 时启用了 `sort&compress`，会整理同类编号，并将不同类型按各自的日文名称分组。表与图混合引用对应 [`REF-MULTI-01`](../../examples/FEATURES.md)，源码见[示例 04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex)，效果见 PDF 第 15 页。`\crefrange` 适合明确连续的起止对象；不连续或类型不同的对象应使用多标签 `\cref`。

## 模板的独有处理

`mathtools` 的 `showonlyrefs` 能直接识别 `\eqref`，但原始 `cleveref` 命令不会自动把所有星号和范围形式登记为公式引用。为保持两者组合工作，模板在 [`config/preamble.tex`](../../template/config/preamble.tex) 中执行两步：

1. 用 `\NewCommandCopy` 把 `\cref`、`\Cref`、`\crefrange`、`\Crefrange` 的原始实现保存为带 `labreport@` 的内部命令。
2. 用 `\RenewDocumentCommand` 包装四个公开命令，先把标签交给 `\noeqref`，再调用原始的普通或星号形式。

报告正文应继续调用公开的 `\cref` 命令族，不要直接使用 `\noeqref` 或任何 `\labreport@...` 名称。

这里的 `\noeqref` 名称容易产生误解：它在包装层中的作用是让 Mathtools 把标签登记为“已经引用”，并不是取消编号。范围包装只登记明确传入的起点和终点，不承诺自动登记两者之间的所有标签。模板也没有承诺普通 `\ref` 或其他未包装的 Cleveref 命令具有同样的 `showonlyrefs` 兼容行为；公式引用优先使用这里列出的命令族或标准 `\eqref`。

修改导言区时需要特别注意：

- 不要在模板配置之后再次加载 `cleveref`，也不要用新的 `\renewcommand` 覆盖这四个包装命令；否则通过 `\cref` 引用的公式可能重新变成无编号。
- 保持 `hyperref` 先于 `cleveref` 加载。模板已经采用这个顺序，不需要在章节文件中再次加载两者。
- 只调整日文名称或格式时，修改 `\crefname`、`\Crefname`、`\crefformat`、`\crefrangeformat` 和连接词设置即可，不要替换包装层。
- 如果确实需要改变编号策略或与其他引用宏包整合，应同时验证 `\cref`、`\Cref`、两个星号形式、`\crefrange`、`\Crefrange`、两个星号范围形式以及混合类型多标签引用。

## 日文对象名称与连接方式

模板将 figure、table、equation、section／subsection、subfigure、listing／lstlisting 分别显示为“図”“表”“式”“節”“図”“リスト”。单项图、表、公式和清单的显式格式分别是 `図<number>`、`表<number>`、`式(<number>)` 与 `リスト<number>`。

图和子图的范围分隔符明确设置为 `〜`；成对项目用“および”，多个项目的中间用“、”，最后一项再用“および”。源码只为 figure 和 subfigure 显式定义了范围格式，其他对象类型仍由 Cleveref 组合当前名称与连接词，因此不要假定所有类型都经过模板格式处理。

## 三方对照入口

全部稳定 ID 的索引见[示例功能索引](../../examples/FEATURES.md)，本篇实际覆盖如下：

| 功能 | 稳定 ID | 源码 | PDF 页 |
| --- | --- | --- | --- |
| 单公式、对齐与常用数学能力 | `MATH-EQUATION-01`、`MATH-ALIGN-01`、`MATH-FREQUENCY-01`、`MATH-DCASES-01`、`MATH-COLONEQQ-01`、`MATH-AMSSYMB-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 3–4 |
| 只给被引用公式编号 | `MATH-SHOWONLYREFS-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| 普通、句首与对象名称 | `REF-SECTION-01`、`REF-NAME-01`、`REF-CAPITAL-01` | [01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)、[02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 3–4 |
| 范围和句首范围 | `REF-RANGE-01`、`REF-RANGE-CAPITAL-01`、`REF-SUBFIG-RANGE-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex)、[03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 4–5、8 |
| 星号形式 | `REF-STAR-01`、`REF-RANGE-STAR-01` | [02-theory.tex](../../examples/source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| 不同类型的多标签引用 | `REF-MULTI-01` | [04-results.tex](../../examples/source/rc-low-pass-filter/sections/04-results.tex) | 15 |
| 无边框超链接 | `LINK-HYPERREF-01` | [03-method.tex](../../examples/source/rc-low-pass-filter/sections/03-method.tex) | 9 |

---

[← 上一篇：图片、表格、代码与绘图](08-figures-tables-and-code.md) | [下一篇：参考文献 →](10-bibliography.md)
