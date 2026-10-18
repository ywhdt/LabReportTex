# Example feature coverage / 示例功能覆盖 / サンプル機能一覧

当前示例是一份 21 页的 RC 低通滤波器实验报告。黑色正文可以单独读成一个完整故事；彩色日文说明使用一般敬语简要介绍当前效果，灰色代码框显示实际使用的源码。编译结果见 [rc-low-pass-filter.pdf](compiled/rc-low-pass-filter.pdf)。

現在のサンプルは、21 ページの RC 低域通過フィルタ実験レポートです。黒い本文だけでも一つの話として読めます。色付きの日本語説明では現在の効果を簡単に紹介し、灰色のコード欄には実際に使ったソースを示します。組版結果は [rc-low-pass-filter.pdf](compiled/rc-low-pass-filter.pdf) で確認できます。

## 对照规则 / 対応ルール

- 源码中的 `% [DOC:FEATURE-ID]` 是长期使用的稳定索引。
- PDF 只给适合从说明文档直接跳转的核心用例显示 ID；局部参数和效果说明可以不编号。
- ID 不依赖源码行号，可以按本页记录的页码直接核对 PDF 效果。
- `\SampleGuideNote` 与 `\SampleGuideSource` 只在示例专用的 [`sample-guide.tex`](source/rc-low-pass-filter/config/sample-guide.tex) 中定义，不属于模板功能。

- ソース内の `% [DOC:FEATURE-ID]` を長期的な索引として使用します。
- PDF で ID を表示するのは、説明文書から直接参照する主要な用例だけです。局所的な引数や表示の補足には番号を付けません。
- ID はソースの行番号に依存せず、このページ欄から PDF の表示結果を直接確認できます。
- `\SampleGuideNote` と `\SampleGuideSource` は、サンプル専用の [`sample-guide.tex`](source/rc-low-pass-filter/config/sample-guide.tex) だけで定義しています。テンプレートの機能ではありません。

## 文档入口与元数据 / 文書入口とメタデータ

| ID | 功能 / 機能 | 示例源码 / サンプルソース | PDF 页 / PDF ページ |
| --- | --- | --- | --- |
| `NOTICE-01` | 标题前的一句示例声明 / タイトル前の一文だけの注意書き | [00-sample-notice.tex](source/rc-low-pass-filter/sections/00-sample-notice.tex) | 1 |
| `DOCMETA-01` | `\DocumentMetadata`：日文语言、关闭 tagging、新列表键 / 日本語、tagging 無効、新しいリストキー | [report.tex](source/rc-low-pass-filter/report.tex) | 1 |
| `MODULE-CORE-01` | 核心导言区入口 / コアプリアンブルの入口 | [report.tex](source/rc-low-pass-filter/report.tex) | 1–21 |
| `MODULE-DIAGRAMS-01` | TikZ、CircuitikZ、`mdframed` 模块 / 図作成モジュール | [report.tex](source/rc-low-pass-filter/report.tex) | 8–9 |
| `MODULE-LISTINGS-01` | `listings` 模块 / ソースコードモジュール | [report.tex](source/rc-low-pass-filter/report.tex) | 10, 16–17 |
| `MODULE-PLOTS-01` | PGFPlots 模块 / PGFPlots モジュール | [report.tex](source/rc-low-pass-filter/report.tex) | 13–15 |
| `MODULE-COMMANDS-01` | 报告信息命令入口 / レポート情報命令の入口 | [report.tex](source/rc-low-pass-filter/report.tex) | 2 |
| `MODULES-01` | 三个可选模块的集中启用方式 / 3 つのオプションモジュールをまとめて有効にする方法 | [report.tex](source/rc-low-pass-filter/report.tex) | 1–2 |
| `GUIDE-TOGGLE-01` | 显示或隐藏示例说明层 / サンプル説明の表示切替 | [report.tex](source/rc-low-pass-filter/report.tex) | 1 |
| `GUIDE-SOURCE-01` | PDF 中显示实际源码片段 / PDF に実際のソースを表示 | [sample-guide.tex](source/rc-low-pass-filter/config/sample-guide.tex) | 1–4, 6, 8–11, 13–14, 16, 19–20 |
| `TITLEBLOCK-01` | 自定义 `\maketitle` 信息表 / 独自の基本情報表 | [report.tex](source/rc-low-pass-filter/report.tex) | 1 |
| `META-TITLE-01` | `\title`：报告标题与 PDF 标题 / レポート名と PDF タイトル | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-AUTHOR-01` | `\author`：作者与 PDF 作者 / 作成者と PDF 作者 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-COURSE-01` | `\course`：课程名 / 科目名 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1，页眉 / ヘッダー |
| `META-GROUP-01` | `\group`：班组 / 班 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1，页眉 / ヘッダー |
| `META-THEME-01` | `\theme`：实验主题编号 / テーマ番号 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1，页眉 / ヘッダー |
| `META-STUDENT-01` | `\studentid`：学号 / 学籍番号 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1，页眉 / ヘッダー |
| `META-PARTNER-01` | `\experimentpartner`：共同实验者 / 共同実験者 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-STAFF-01` | `\staff`：教师与 TA / 担当教員と TA | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-DATE-01` | `\experimentdate`：实验日期 / 実験日 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-PLACE-01` | `\place`：实验地点 / 実験場所 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-TIME-01` | `\experimenttime`：起止时间 / 実験時間 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-DUE-01` | `\reportfirstdue`：首次提交日期 / 初回提出日 | [metadata.tex](source/rc-low-pass-filter/config/metadata.tex) | 1 |
| `META-TITLE-AUTHOR-01` | 标题与作者写入 PDF 元数据 / タイトルと作者の PDF メタデータ | [report.tex](source/rc-low-pass-filter/report.tex) | 1 |
| `META-HEADER-01` | 页眉字段的对应关系 / ヘッダー項目との対応 | [report.tex](source/rc-low-pass-filter/report.tex) | 1–21 |
| `META-TABLE-01` | `\maketitle` 中的其他字段 / 基本情報表の各項目 | [report.tex](source/rc-low-pass-filter/report.tex) | 1 |
| `LAYOUT-HEADER-01` | `fancyhdr` 页眉和页码 / ヘッダーとページ番号 | [report.tex](source/rc-low-pass-filter/report.tex) | 1–21 |
| `LAYOUT-PAGE-01` | `geometry` 统一页边距 / 全ページ共通の余白 | [report.tex](source/rc-low-pass-filter/report.tex) | 1–21 |
| `CORE-JAPANESE-01` | `luatexja-preset` 的日文、西文、数字与数学混排 / 日本語・欧文・数式の混植 | [report.tex](source/rc-low-pass-filter/report.tex) | 1–21 |
| `COLOR-ANNOTATION-01` | `xcolor` 多色无框说明 / 複数色の枠なし説明 | [sample-guide.tex](source/rc-low-pass-filter/config/sample-guide.tex) | 1–20 |

## 结构与列表 / 構造とリスト

| ID | 功能 / 機能 | 示例源码 / サンプルソース | PDF 页 / PDF ページ |
| --- | --- | --- | --- |
| `STRUCT-SECTION-01` | `section` 与 `subsection` 的层级 / `section` と `subsection` の見出し階層 | [01-purpose.tex](source/rc-low-pass-filter/sections/01-purpose.tex) | 2 |
| `LIST-ITEMIZE-01` | `itemize` 实验目标 / 箇条書きの実験目標 | [01-purpose.tex](source/rc-low-pass-filter/sections/01-purpose.tex) | 2 |
| `LIST-ITEMIZE-02` | 带 `itemsep` 的观测量列表 / 項目間隔を持つ観測量一覧 | [01-purpose.tex](source/rc-low-pass-filter/sections/01-purpose.tex) | 3 |
| `LIST-ITEMIZE-03` | 测量条件列表 / 測定条件一覧 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 9 |
| `LIST-ITEMIZE-04` | 总结中的定量结果 / まとめの定量結果 | [06-summary.tex](source/rc-low-pass-filter/sections/06-summary.tex) | 19 |
| `LIST-KEY-LEFTMARGIN-01` | 新版 `leftmargin` 键 / 新しい `leftmargin` キー | [01-purpose.tex](source/rc-low-pass-filter/sections/01-purpose.tex) | 2 |
| `LIST-KEY-NOSEP-01` | 新版 `nosep` 键 / 新しい `nosep` キー | [01-purpose.tex](source/rc-low-pass-filter/sections/01-purpose.tex) | 2 |
| `LIST-ENUMERATE-01` | `(1)` 形式的测量步骤 / `(1)` 形式の測定手順 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 10 |
| `LIST-ENUMERATE-02` | `1.` 形式的改进顺序 / `1.` 形式の改善手順 | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 19 |
| `LIST-DESCRIPTION-01` | 符号说明 / 記号の説明 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 6 |
| `LIST-DESCRIPTION-02` | 频率区域术语 / 周波数領域の用語 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 6 |
| `LIST-DESCRIPTION-03` | CSV 字段说明 / CSV フィールドの説明 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 10 |
| `LIST-DESCRIPTION-04` | 误差来源 / 誤差要因 | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 19 |
| `LIST-BASIC-01` | 基础 `list` 与传统长度设置 / 基本 `list` と長さ指定 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 6 |

## 数学与交叉引用 / 数式と相互参照

| ID | 功能 / 機能 | 示例源码 / サンプルソース | PDF 页 / PDF ページ |
| --- | --- | --- | --- |
| `MATH-EQUATION-01` | 单个 `equation` / 一つの独立した数式 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 3 |
| `MATH-ALIGN-01` | `align` 多行推导与逐行标签 / 複数行の導出と各行のラベル | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 4 |
| `MATH-FREQUENCY-01` | 幅值、利得和相位公式 / 振幅・利得・位相の数式 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 4 |
| `MATH-DCASES-01` | `mathtools` 的 `dcases` / 条件分岐した数式 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 4 |
| `MATH-COLONEQQ-01` | `\coloneqq` 定义符号 / 定義を表す記号 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 4 |
| `MATH-AMSSYMB-01` | `amssymb` 的 `\mathbb{C}`、`\mathbb{R}` / 数集合の記号 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 3 |
| `MATH-SIUNITX-01` | `siunitx` 数值、单位、范围与角度 / 数値・単位・範囲・角度 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| `MATH-SHOWONLYREFS-01` | 被引用公式有编号、未引用公式无编号 / 参照された数式だけを番号付け | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| `REF-SECTION-01` | `\cref` 引用章节 / 節の参照 | [01-purpose.tex](source/rc-low-pass-filter/sections/01-purpose.tex) | 3 |
| `REF-NAME-01` | 自动输出“式、图、表、节、清单” / ラベル種別名の自動出力 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 4 |
| `REF-CAPITAL-01` | 句首 `\Cref` / 文頭の `\Cref` | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 4 |
| `REF-RANGE-01` | `\crefrange` 连续范围 / 連続した範囲参照 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 4 |
| `REF-RANGE-CAPITAL-01` | 句首 `\Crefrange` / 文頭の範囲参照 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| `REF-STAR-01` | `\cref*`、`\Cref*` 无链接形式 / リンクを付けない単独参照 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| `REF-RANGE-STAR-01` | `\crefrange*`、`\Crefrange*` / リンクを付けない範囲参照 | [02-theory.tex](source/rc-low-pass-filter/sections/02-theory.tex) | 5 |
| `REF-SUBFIG-RANGE-01` | 子图编号的范围格式 / 子図番号の範囲参照 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| `REF-MULTI-01` | 一个 `\cref` 引用不同类型的多个标签 / 異なる種類の複数ラベル | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 15 |
| `LINK-HYPERREF-01` | `hyperref` 链接与 `hidelinks` / 枠を表示しない参照・URL リンク | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 9 |

## 表格、图像与装置图 / 表、画像、装置図

| ID | 功能 / 機能 | 示例源码或数据 / サンプルソースまたはデータ | PDF 页 / PDF ページ |
| --- | --- | --- | --- |
| `TABLE-TABULARX-01` | `tabularx` 的 `X` 自动换行列 / 残り幅を使う `X` 列 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 6–7 |
| `TABLE-PCOLUMN-01` | 可换行固定宽度 `p` 列 / 折り返し可能な固定幅 `p` 列 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 7 |
| `TABLE-BOOKTABS-01` | `\toprule`、`\midrule`、`\bottomrule` | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 7 |
| `TABLE-MULTIROW-01` | `multirow` 跨行频段分组 / 周波数領域の複数行グループ | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 11 |
| `TABLE-REPEAT-01` | 标准 `tabular` 的数值比较 / 通常の数値表 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 12 |
| `FLOAT-H-01` | `float` 的 `[H]` 固定位置 / 現在位置への固定配置 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex), [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex), [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 7–9, 11–15, 17 |
| `DIAG-CIRCUIT-01` | CircuitikZ 的 RC 回路 / CircuitikZ による RC 回路 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| `DIAG-APPARATUS-01` | TikZ 装置连接图 / TikZ の測定装置接続図 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| `DIAG-TIKZ-01` | `positioning` 与 `arrows.meta` / 相対配置と矢印 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| `DIAG-CALC-01` | `calc` 计算两节点中点 / 二つのノードの中点計算 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| `DIAG-FRAME-01` | `mdframed` 作为实验装置外框 / 測定装置全体の枠線 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| `FIG-SUBFIGURE-01` | `subfigure` 子图与总图题 / 小図と全体の図題 | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8 |
| `DIAG-LOGIC-01` | `circuits.logic.US` 的 AND 门 / 測定許可を表す AND ゲート | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 8–9 |
| `GRAPHICSPATH-01` | `\graphicspath` 图像目录 / 画像検索ディレクトリ | [report.tex](source/rc-low-pass-filter/report.tex) | 9 |
| `FIG-IMAGE-01` | `graphicx` 的 `\includegraphics` / 外部 PNG の読み込み | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex), [oscilloscope-waveform.png](source/rc-low-pass-filter/figures/oscilloscope-waveform.png) | 9 |

## 数据与 PGFPlots / データと PGFPlots

| ID | 功能 / 機能 | 示例源码或数据 / サンプルソースまたはデータ | PDF 页 / PDF ページ |
| --- | --- | --- | --- |
| `PLOT-DATAFILE-01` | `\addplot table` 读取 CSV / CSV の列名を指定した読込み | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex), [frequency-response.csv](source/rc-low-pass-filter/data/frequency-response.csv) | 13–15 |
| `PLOT-SINGLE-01` | 单纵轴测量值与理论值 / 単一縦軸の測定値と理論値 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 13 |
| `PLOT-STYLE-AXIS-01` | `report axis` 共通轴样式 / 共通の軸書式 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 13 |
| `PLOT-STYLE-PLOT-01` | `report plot` 共通数据系列 / 共通の系列書式 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 13 |
| `PLOT-STYLE-LEGEND-01` | `report legend` 图例样式 / 凡例の位置・枠・文字 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 13 |
| `PLOT-LOG-01` | `xmode=log` 对数频率轴 / 対数周波数軸 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 13–15 |
| `PLOT-DUAL-01` | 重叠两个 `axis` 的双纵轴图 / 二つの `axis` を重ねた二重縦軸 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 14–15 |
| `PLOT-STYLE-RIGHT-01` | `report dual right axis` 右轴样式 / 右側縦軸の書式 | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 14–15 |
| `PLOT-BASE-01` | `report axis base` 无图例残差图 / 凡例なしの基礎軸スタイル | [04-results.tex](source/rc-low-pass-filter/sections/04-results.tex) | 15 |

## 代码清单 / ソースコード

| ID | 功能 / 機能 | 示例源码 / サンプルソース | PDF 页 / PDF ページ |
| --- | --- | --- | --- |
| `CODE-INLINE-01` | `\lstinline` 行内文件名 / 本文中の短いコード | [03-method.tex](source/rc-low-pass-filter/sections/03-method.tex) | 10 |
| `CODE-BLOCK-01` | Python `lstlisting`、行号、边框和题注 / Python、行番号、枠、キャプション | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 16 |
| `CODE-HIGHLIGHT-01` | 关键字、注释、字符串高亮 / キーワード・コメント・文字列の色分け | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 16 |
| `CODE-WRAP-01` | `breaklines=true` 自动换行 / 長いコード行の自動折返し | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 16 |
| `CODE-REF-01` | 带 `label` 的清单可由 `\cref` 引用 / ラベル付きリストの参照 | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 16 |
| `CODE-BLOCK-02` | 第二个 Python 插值程序 / 二点補間の Python コード | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 17 |
| `CODE-LANGUAGE-02` | `language=bash` 的另一种语言 / Bash の構文表示 | [05-discussion.tex](source/rc-low-pass-filter/sections/05-discussion.tex) | 17 |

## 参考文献 / 参考文献

| ID | 功能 / 機能 | 示例源码 / サンプルソース | PDF 页 / PDF ページ |
| --- | --- | --- | --- |
| `BIB-RESOURCE-01` | `\addbibresource` 文献数据库入口 / 文献データベースの登録 | [report.tex](source/rc-low-pass-filter/report.tex) | 20–21 |
| `BIB-REPOSITORY-01` | 将公开项目仓库作为第一条在线文献引用 / 公開プロジェクトを先頭のオンライン文献として引用 | [00-sample-notice.tex](source/rc-low-pass-filter/sections/00-sample-notice.tex), [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 1, 20 |
| `BIB-JAPANESE-01` | `langid={japanese}` 选择日文格式 / 日本語書式の選択 | [07-references.tex](source/rc-low-pass-filter/sections/07-references.tex) | 20 |
| `BIB-DRIVERS-01` | 11 种日文 driver 的汇总 / 11 種類の日本語 driver | [07-references.tex](source/rc-low-pass-filter/sections/07-references.tex) | 20–21 |
| `BIB-JA-BOOK-01` | 日文 `book` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-ARTICLE-01` | 日文 `article` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-INBOOK-01` | 日文 `inbook` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-INCOLLECTION-01` | 日文 `incollection` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-INPROCEEDINGS-01` | 日文 `inproceedings` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-PROCEEDINGS-01` | 日文 `proceedings` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 21 |
| `BIB-JA-REPORT-01` | 日文 `report` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-THESIS-01` | 日文 `thesis` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-ONLINE-01` | 日文 `online` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-MISC-01` | 日文 `misc` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 20 |
| `BIB-JA-UNPUBLISHED-01` | 日文 `unpublished` | [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 21 |
| `BIB-EN-FALLBACK-01` | 英文 `article` 的 IEEE 回退 / 英語文献の IEEE 書式 | [07-references.tex](source/rc-low-pass-filter/sections/07-references.tex), [references.bib](source/rc-low-pass-filter/bibliography/references.bib) | 21 |
| `BIB-URL-01` | `url` 与 `urldate` / URL と参照日 | [07-references.tex](source/rc-low-pass-filter/sections/07-references.tex) | 20 |
| `BIB-COMPRESS-01` | `numeric-comp` 连续编号压缩 / 連続した引用番号の圧縮 | [07-references.tex](source/rc-low-pass-filter/sections/07-references.tex) | 正文 / 本文 |
| `BIB-CONDITION-01` | 有引用时才打印文献表 / 引用済み文献がある場合だけ出力 | [07-references.tex](source/rc-low-pass-filter/sections/07-references.tex) | 20 |
| `BIB-PRINT-01` | `\printbibliography` / 参考文献一覧の出力 | [07-references.tex](source/rc-low-pass-filter/sections/07-references.tex) | 20–21 |

本表只覆盖模板直接加载的宏包、公开命令、公开 PGFPlots 样式以及示例实际使用的环境；不会把第三方宏包的内部命令或模板中的私有兼容代码作为用户接口。

この表は、テンプレートが直接読み込むパッケージ、公開命令、公開 PGFPlots スタイル、サンプルで実際に使用する環境を対象とします。第三者パッケージの内部命令やテンプレートの非公開互換コードは、利用者向けのインターフェースとして扱いません。
