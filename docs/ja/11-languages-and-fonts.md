# 日本語組版とフォント

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/11-languages-and-fonts.md)

このテンプレートは、日本語の実験レポートを組版するためのものです。LuaLaTeX と `ltjsarticle` は日本語文書の基本的な組版方法を決め、`luatexja-preset` は日本語フォントのプリセットを選びます。欧文フォントと数学フォントは、それぞれ別の仕組みで扱われます。実験内容を記入するだけなら、これらの設定を変更する必要はありません。

## LuaLaTeX と `ltjsarticle`

[`report.tex`](../../template/report.tex) の先頭は、次の設定で固定されています。

```tex
% !TeX program = lualatex
% TeX Live 2026: enable the new list keys without tagged-PDF output.
\DocumentMetadata{lang=ja-JP,tagging=off}
\documentclass{ltjsarticle}
```

コンパイルには LuaLaTeX を使い、エディタのエンジンを pdfLaTeX や XeLaTeX に変更しないでください。最も確実なのは、`report.tex` があるディレクトリで次を実行する方法です。

```console
latexmk -lualatex report.tex
```

`ltjsarticle` は LuaTeX-ja が提供する日本語用の `article` クラスで、日本語の文字、改行、レイアウトに必要な基本処理を担当します。テンプレートは他のエンジン向けに同等の設定を保守していません。一部のソースを読み込めたとしても、そのエンジンがサポート対象であるとは限りません。

## 文書言語、tagging、リストキー

`\DocumentMetadata` は、必ず `\documentclass` より前に置きます。現在の二つの引数には、次の意味があります。

- `lang=ja-JP` は文書の主要言語を日本語と宣言します。フォント名ではなく、日本語フォントを自動的に選ぶ設定でもありません。
- `tagging=off` は tagged PDF の出力を無効にします。現在のテンプレートは、アクセシビリティ用のタグ構造を生成することを保証していません。

テンプレートが最低限サポートする TeX Live 2026 では、この宣言によって LaTeX の新しいリストキーも有効になります。サンプルの `leftmargin` と `nosep` は、それぞれ [`LIST-KEY-LEFTMARGIN-01`](../../examples/FEATURES.md) と [`LIST-KEY-NOSEP-01`](../../examples/FEATURES.md) に対応します。ソースは[サンプル 01-purpose.tex](../../examples/source/rc-low-pass-filter/sections/01-purpose.tex)、結果は PDF 第 2 ページです。これらのキーはリストのインデントと間隔を調整するもので、フォント選択とは関係ありません。フォントを変更するために `\DocumentMetadata` を削除しないでください。

文書メタデータの設定は [`DOCMETA-01`](../../examples/FEATURES.md) に対応します。サンプル第 1 ページの色付き注釈で設定場所を示していますが、`lang=ja-JP` と `Tagged: no` は PDF の文書プロパティなので、ページの見た目だけでは確認できません。目に見える日本語の混植は、後述する `CORE-JAPANESE-01` で確認してください。

## 標準のフォントプリセット

中核プリアンブルの [`config/preamble.tex`](../../template/config/preamble.tex) は、次を使います。

```tex
\usepackage[deluxe]{luatexja-preset}
```

このプロジェクトがサポートする TeX Live 2026 環境では、`luatexja-preset` に別のプリセット名を指定しない場合、既定で `haranoaji`、つまり原ノ味フォントのプリセットが選ばれます。したがって、現在のテンプレートの標準日本語フォントは原ノ味フォントです。`deluxe` は、プリセットが提供する複数ウェイトの日本語フォント設定を有効にし、明朝体、ゴシック体、強調用のウェイトを LuaTeX-ja のプリセットに従って対応させます。文書言語を判定する設定ではなく、欧文フォントや数学フォントを同時に置き換えるものでもありません。

サンプルでは、日本語、欧文、数字、数式を一つのレポート内で混植しています。この用例は [`CORE-JAPANESE-01`](../../examples/FEATURES.md) に対応します。設定箇所は[サンプル report.tex](../../examples/source/rc-low-pass-filter/report.tex)、実際の混植は[サンプル PDF 第 1～21 ページ](../../examples/compiled/rc-low-pass-filter.pdf)で確認できます。中核プリアンブルの読み込み箇所は [`MODULE-CORE-01`](../../examples/FEATURES.md) に対応し、その設定はサンプル PDF 第 1～21 ページ全体に作用します。

### 3 種類のフォントの境界

- **日本語フォント**は LuaTeX-ja と `luatexja-preset` が管理します。漢字、平仮名、片仮名、日本語の句読点に使うフォントは、主にこの層で決まります。
- **欧文フォント**はラテン文字などに使われ、既定の対応は文書クラスと LaTeX の欧文フォント設定で決まります。`luatexja-preset` の日本語プリセットを変えても、欧文フォントが同名のファミリへ自動的に切り替わるわけではありません。
- **数学フォント**は、数式モード内の文字、数字、記号に使われます。テンプレートは数学機能のために `mathtools` と `amssymb` を読み込みますが、日本語フォントのプリセットに新しい数学フォント一式を結び付けてはいません。`\mathrm{...}` などのコマンドが選ぶのは数学フォントのスタイルであり、日本語の明朝体ではありません。

現在のサンプル PDF では、欧文本文に主として Latin Modern フォントが埋め込まれ、数学には Computer Modern と AMS の数学フォントが使われています。ただし、テンプレートは追加設定によって、これらの具体的な欧文フォントや数学フォントを公開インターフェースとして固定してはいません。文書クラス、パッケージ、TeX ディストリビューションを変更した場合は、改めて確認する必要があります。

このため、日本語本文、欧文の変数、数式の見た目が完全には同じでないことは正常です。授業で 3 種類のフォントにそれぞれ特定のファミリが指定されている場合は、個別に設定してください。日本語フォントのオプションを一つ変更しただけで、すべてが切り替わったと考えないでください。

## 日本語フォントの変更

別の LuaTeX-ja プリセットだけを使いたい場合は、[`config/preamble.tex`](../../template/config/preamble.tex) の元の 1 行を置き換えます。`luatexja-preset` を重ねて読み込まないでください。たとえば、現在の既定値を明示するには次のようにします。

```tex
\usepackage[deluxe,haranoaji]{luatexja-preset}
```

フォントファイルが利用できることを確認したうえで、IPAex プリセットを選ぶこともできます。

```tex
\usepackage[deluxe,ipaex]{luatexja-preset}
```

まず、必要なフォントファイルを TeX が見つけられるか、ターミナルで確認できます。

```console
kpsewhich HaranoAjiMincho-Regular.otf
kpsewhich ipaexm.ttf
```

パスが出力された場合にだけ、現在の TeX 環境からそのファイルを見つけられます。プロジェクトの TeX Live 2026 環境には、標準の原ノ味フォントがあり、IPAex も見つかります。

ソースファイルの可搬性を保つため、TeX Live に収録され、PDF に埋め込めるフォントの使用を勧めます。

## フォント関連のトラブルシューティング

### 文書クラスや日本語コマンドを使用できないと報告される

まず `latexmk -lualatex report.tex` を実行しているか確認し、次を調べます。

```console
lualatex --version
kpsewhich ltjsarticle.cls
kpsewhich luatexja-preset.sty
```

ファイルが見つからない場合は、TeX Live のインストールが不完全か、エディタとターミナルが別の TeX 環境を使っている可能性があります。

### 日本語フォントが見つからない、または代替される

プリセットの変更後に問題が出た場合は、まず元の `\usepackage[deluxe]{luatexja-preset}` に戻してください。原ノ味、IPAex など TeX Live のフォントは `kpsewhich` で確認できます。OS のフォントに依存するプリセットは、実際のコンパイルログとフォントの解決結果で判断してください。`luatexja-preset` の読み込み行を二つ同時に残さないでください。

### PDF に欠落文字の四角形が出る

欠落文字は、選択したフォントにその文字が含まれていないために起きることが多く、UTF-8 のソースが破損しているとは限りません。該当する文字を記録し、ログの missing character 情報を確認して、テンプレートがサポートする日本語組版の範囲にある文字か調べてください。珍しい記号には、その字形を収録したフォントが必要です。日本語以外の CJK テキストの組版は、テンプレートのサポート範囲外です。

### エディタではコンパイルできるがターミナルではできない、またはその逆

両方で実際に呼び出される `latexmk`、`lualatex`、`kpsewhich` のパスを比較してください。TeX Live のバージョンが違うと、別のプリセットが選ばれたり、同じフォントを見つけられなかったりする場合があります。テンプレートがサポートする最低バージョンは TeX Live 2026 です。まず、エディタのコンパイラとターミナルが同じ TeX 環境を使い、どちらも TeX Live 2026 以上であることを確認してください。

---

[← 前へ：参考文献](10-bibliography.md) | [次へ：ソース・説明・PDF の対応関係 →](12-source-doc-pdf-cross-reference.md)
