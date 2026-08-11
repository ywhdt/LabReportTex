# Examples / 示例 / サンプル

> **注意：** 示例中的实验内容、测量数据、计算结果和结论均为模板功能展示而编写，不代表真实实验，也不应作为可靠的实验资料使用。示例内容以便于展示各项特性为优先，不追求正式实验报告的严谨性。

> **注意：** サンプル内の実験内容、測定値、計算結果および結論は、テンプレート機能の説明用に作成したものです。実在する実験結果や信頼できる実験資料を示すものではなく、説明のわかりやすさを優先しています。

示例源码与编译结果使用相同名称配对：

```text
source/<example-name>/
compiled/<example-name>.pdf
```

示例用于展示模板功能和对应的 PDF 效果，不会包含在 Release 的干净模板压缩包中。

当前示例采用一份完整的虚构实验报告作为主线。黑色文字可以单独读成一篇报告；彩色文字用较简单的日文说明当前效果，灰色代码框则显示实际生成该效果的源码。希望进一步了解时，可以再对照仓库中的详细文档和完整源码。

这些说明由 [`config/sample-guide.tex`](source/rc-low-pass-filter/config/sample-guide.tex) 中的 `\SampleGuideNote` 与 `\SampleGuideSource` 生成，只属于示例 PDF，不是模板命令，也不会进入 Release 的干净模板。把 `report.tex` 中的 `\showsampleguidetrue` 改为 `\showsampleguidefalse`，即可隐藏彩色说明和灰色源码框。

带功能 ID 的说明可以在 [`FEATURES.md`](FEATURES.md) 中查找对应源码和 PDF 页码；没有编号的文字则是放在对应内容附近的补充说明。

サンプルソースとコンパイル結果は、同じ名前で対応させます。

```text
source/<example-name>/
compiled/<example-name>.pdf
```

サンプルはテンプレートの機能と PDF の表示結果を示すためのもので、Release の空テンプレートには含めません。

現在のサンプルは、架空の実験を一つの完成したレポートとして構成しています。黒い文章だけでも一つのレポートとして読めます。色付きの文章では、その場で見えている効果を簡単な日本語で説明し、灰色のコード欄には実際に使ったソースを示します。さらに詳しく知りたい場合は、リポジトリの説明文書と完全なソースを参照してください。

これらの説明は [`config/sample-guide.tex`](source/rc-low-pass-filter/config/sample-guide.tex) の `\SampleGuideNote` と `\SampleGuideSource` で作成しています。どちらもサンプル PDF 専用であり、テンプレートのコマンドではなく、Release の空テンプレートにも含まれません。`report.tex` の `\showsampleguidetrue` を `\showsampleguidefalse` に変更すると、色付きの説明と灰色のコード欄を非表示にできます。

機能 ID が付いた説明は、[`FEATURES.md`](FEATURES.md) から対応するソースと PDF ページを探せます。番号のない文章は、その場所に関する補足説明です。

功能覆盖情况请参阅 [FEATURES.md](FEATURES.md)。
