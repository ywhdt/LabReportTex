# 快速开始

[项目首页](../../README.md) · [文档目录](README.md) · [日本語](../ja/03-quick-start.md)

## 复制空白模板

普通使用者只需要下载公开仓库的 Release ZIP，不需要安装 Git，也不需要克隆仓库。可在 [LabReportTex Releases](https://github.com/ywhdt/LabReportTex/releases) 选择需要的版本，下载名称形如 `LabReportTex-template-v<version>.zip` 的模板压缩包。

ZIP 内已经是可独立编译的干净模板。不要从开发仓库复制示例报告来覆盖它。

## 填写报告信息

打开 `config/metadata.tex`，先填写标题、作者和课程要求中需要显示的字段。下面的内容与模板中已有命令相同，可作为首次填写的起点：

```tex
\title{RC低域通過フィルタの周波数特性}
\author{サンプル作成者}

\course{電気電子工学基礎実験}
\group{A}
\studentid{000000000}
\experimentpartner{共同実験者A}
\staff{担当教員、TA}
\experimentdate{2026年4月10日}
\place{電気系実験室A}
\experimenttime{13:00--16:00}
\reportfirstdue{2026年4月17日}
\theme{03}
```

模板会将以上字段分别用于首页信息表或页眉；其中 `\title` 与 `\author` 还会写入 PDF 元数据。字段的完整对应关系会在[报告基本信息](05-metadata.md)中说明。

## 撰写正文

正文文件位于 `sections/`。默认顺序是目的、原理、实验方法、实验结果、考察、总结和参考文献。前六个正文文件已经包含日文章节标题和一行注释提示；参考文献标题只在存在有效引用时生成。先在已有章节标题下面替换注释或加入自己的文字，例如编辑 `sections/01-purpose.tex`：

```tex
\section{目的}
本実験では，RC回路の周波数特性を測定し，理論値と比較する。
```

根文件 [report.tex](../../template/report.tex) 会依次读取这些文件。

## 编译

在 `report.tex` 所在的模板根目录运行：

```console
latexmk -lualatex report.tex
```

`latexmk` 会使用 LuaLaTeX，并在参考文献或交叉引用需要更新时自动安排额外的编译步骤。


## 检查输出

编译完成后会在根目录生成 `report.pdf` 以及各类辅助文件。


---

[← 上一篇：环境与依赖](02-prerequisites.md) | [下一篇：项目目录结构 →](04-project-structure.md)
