# LabReportTex Template

[中文](#中文) | [日本語](#日本語)

## 中文

本模板用于编写日文实验报告，最低支持 TeX Live 2026。

1. 在 `config/metadata.tex` 中填写报告信息。
2. 在 `sections/` 中编写各章节。
3. 将图片放入 `figures/`，将参考文献写入 `bibliography/references.bib`。
4. 在当前目录运行：

   ```console
   latexmk -lualatex report.tex
   ```

编译需要 LuaLaTeX、Latexmk 和 Biber。`\DocumentMetadata` 用于启用 TeX Live 2026 的新版列表键，无需直接引入 `enumitem`。报告标题和作者会写入 PDF 元数据，但模板不生成带无障碍标签的 PDF。

公式编号采用 `showonlyrefs`，可以照常使用 `\cref`、`\Cref` 及其星号和范围形式。TikZ、电路图、代码清单和 PGFPlots 等功能可在 `config/preamble.tex` 中按需启用。

完整说明请访问 [LabReportTex 项目仓库](https://github.com/ywhdt/LabReportTex)。
本模板采用 [MIT License](LICENSE)。

## 日本語

本テンプレートは日本語の実験レポート用で，最低対応バージョンは TeX Live 2026 です。

1. `config/metadata.tex` にレポート情報を記入します。
2. `sections/` に各セクションの本文を作成します。
3. 図は `figures/`、参考文献は `bibliography/references.bib` に追加します。
4. このディレクトリで次のコマンドを実行します。

   ```console
   latexmk -lualatex report.tex
   ```

コンパイルには LuaLaTeX、Latexmk、Biber が必要です。`\DocumentMetadata` により，`enumitem` を直接読み込まず TeX Live 2026 の新しいリストキーを有効にしています。レポート名と作成者は PDF メタデータにも保存されますが，タグ付き PDF は生成しません。

数式番号には `showonlyrefs` を使い，`\cref`、`\Cref` と各コマンドのスター付き・範囲指定形式をそのまま利用できます。TikZ、回路図、ソースコード、PGFPlots などは `config/preamble.tex` から必要な機能だけを有効にしてください。

詳しい説明は [LabReportTex プロジェクト](https://github.com/ywhdt/LabReportTex)を参照してください。
本テンプレートは [MIT License](LICENSE) で公開されています。
