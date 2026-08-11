# 报告基本信息

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/05-metadata.md)

## 在哪里填写

每份报告的信息都集中在 [`config/metadata.tex`](../../template/config/metadata.tex)。下载模板后，保留命令名称，只修改花括号中的内容即可：

```tex
\title{RC低域通過フィルタの周波数特性}
\author{サンプル作成者}

\course{電気電子工学基礎実験}
\group{A}
\studentid{000000000}
\experimentpartner{共同実験者A、共同実験者B}
\staff{担当教員、TA}
\experimentdate{2026年4月15日}
\place{電気系実験室}
\experimenttime{13:00--16:00}
\reportfirstdue{2026年4月22日}
\theme{04}
```

不使用的字段可以保留为空，例如 `\experimentpartner{}`。这样既不会改变命令接口，也方便以后继续填写。

`\group{A}` 会由模板自动追加“班”并显示为 `A班`，因此花括号中不要重复写成 `A班`。模板不检查字段长度；内容较长时虽然可以换行，仍应在编译后检查基本信息表和页眉是否合适。

## 各字段显示在哪里

模板的第一页包含基本信息表，所有页面还会显示统一页眉。各命令的用途如下：

| 命令 | 内容 | 使用位置 |
| --- | --- | --- |
| `\title{...}` | 报告标题 | 第一页信息表、PDF 标题元数据 |
| `\author{...}` | 报告作者 | 第一页信息表、页眉、PDF 作者元数据 |
| `\course{...}` | 课程或实验科目 | 页眉左侧 |
| `\theme{...}` | 实验主题编号 | 页眉中的“テーマ” |
| `\studentid{...}` | 学号 | 页眉中的“学籍番号” |
| `\group{...}` | 班组 | 页眉中的“班” |
| `\experimentpartner{...}` | 共同实验者 | 第一页信息表 |
| `\staff{...}` | 教师或 TA | 第一页信息表 |
| `\experimentdate{...}` | 实验日期 | 第一页信息表 |
| `\place{...}` | 实验地点 | 第一页信息表 |
| `\experimenttime{...}` | 实验起止时间 | 第一页信息表 |
| `\reportfirstdue{...}` | 首次提交日期 | 第一页信息表 |

第一页信息表和页眉由 [`config/commands.tex`](../../template/config/commands.tex) 定义。

## 标题、作者与 PDF 元数据

`\title` 和 `\author` 不只会出现在页面上，也会分别写入生成 PDF 的 Title 和 Author 属性。在 `metadata.tex` 由用户填写的字段中，其他字段只用于可见排版，不映射到这两个 PDF 属性。

标题和作者尽量使用普通文本。复杂排版命令不一定能安全转换为 PDF 字符串，并可能触发 Hyperref 警告。

PDF 还会由 LaTeX 工具链自动记录语言、Creator、Producer、生成时间和源文件名等技术信息。因此，“标题和作者写入 PDF 元数据”不表示 PDF 中只存在这两个元数据字段。

模板开头的 `\DocumentMetadata{lang=ja-JP,tagging=off}` 声明文档语言为日文，并关闭 Tagged PDF 输出。它也用于启用 TeX Live 2026 的新版列表键；不要为了修改标题或作者而删除这条声明。

编译后可以使用系统的 PDF 属性窗口查看标题和作者，也可以在终端中检查：

```console
pdfinfo report.pdf
```

## 与示例对照

完整示例的填写方式见[示例 metadata.tex](../../examples/source/rc-low-pass-filter/config/metadata.tex)，编译后的第一页见[示例 PDF](../../examples/compiled/rc-low-pass-filter.pdf)。功能索引中的 `META-TITLE-01`、`META-AUTHOR-01`、`META-HEADER-01`、`META-TABLE-01`、`META-TITLE-AUTHOR-01`、`DOCMETA-01` 和其他 `META-*` 条目记录了各字段的源码位置及 PDF 页码，参见[示例功能索引](../../examples/FEATURES.md)。

---

[← 上一篇：项目目录结构](04-project-structure.md) | [下一篇：正文与章节结构 →](06-document-structure.md)
