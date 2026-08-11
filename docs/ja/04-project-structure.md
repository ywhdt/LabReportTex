# プロジェクトのディレクトリ構成

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/04-project-structure.md)

## ダウンロード後のディレクトリ

Releases からダウンロードして展開すると、単独でコンパイルできるクリーンなテンプレートのディレクトリが得られます。バージョン番号は Release ごとに変わりますが、ディレクトリ構成は同じです。

```text
LabReportTex-template-v<version>/
├── VERSION
├── LICENSE
├── README.md
├── report.tex
├── biblatex-jareport.sty
├── biblatex-jareport-japanese.lbx
├── config/
├── sections/
├── figures/
└── bibliography/
```

パッケージ作成時は、`template/` から公開を許可したファイルを一つずつコピーし、ZIP 内に `VERSION` を生成します。そのため、開発ツリーの `template/` にはこのファイルがありません。`figures/` はクリーンなテンプレートでは空の予約ディレクトリであり、そのまま画像を追加できます。

コンパイル時は、この階層に移動してから次を実行します。

```console
latexmk -lualatex report.tex
```

## 通常変更するファイル

初めて使うときに主に扱う場所は、次のとおりです。

| パス | 役割 | 変更の目安 |
| --- | --- | --- |
| [`report.tex`](../../template/report.tex) | レポートのメインファイルです。設定とセクションを読み込む順序を決めます。 | セクションを追加、削除、並べ替えするときだけ変更します。 |
| [`config/metadata.tex`](../../template/config/metadata.tex) | 題目、作成者、科目、学籍番号、実験日などの情報です。 | レポートごとに記入します。 |
| [`sections/`](../../template/sections) | 各セクションの本文です。 | レポートごとに記入します。 |
| `figures/` | 外部画像ファイルです。 | 画像を使うときに追加します。 |
| [`bibliography/references.bib`](../../template/bibliography/references.bib) | BibLaTeX の参考文献データです。 | 参考文献を使うときに記入します。 |

`report.tex` では `\graphicspath{{figures/}}` で画像ディレクトリを、`\addbibresource{bibliography/references.bib}` で参考文献データベースを指定済みです。そのため、既定のディレクトリ関係を保つかぎり、本文で完全なパスを何度も書く必要はありません。

## `config/` 内の設定

`config/` にはテンプレートの設定があります。最初のレポートで、すべてを変更する必要はありません。

- [`metadata.tex`](../../template/config/metadata.tex)：各レポートの情報を記入します。
- [`preamble.tex`](../../template/config/preamble.tex)：中核のパッケージ、ページレイアウト、相互参照、オプションモジュールの読み込み箇所です。
- [`commands.tex`](../../template/config/commands.tex)：基本情報表、ヘッダー、フッターなどのテンプレートコマンドです。
- [`diagrams.tex`](../../template/config/diagrams.tex)：TikZ、回路図、枠付きの内容を扱うモジュールです。
- [`listings.tex`](../../template/config/listings.tex)：コードリスト用モジュールです。
- [`plots.tex`](../../template/config/plots.tex)：PGFPlots のグラフ用モジュールです。

最後の 3 つのオプションモジュールは初期状態では有効になっていません。使う場合は、`config/preamble.tex` にある対応する `\input` 行のコメントを外してください。設定全体を本文へコピーする必要はありません。具体的な使い方は[図、表、コード、グラフ](08-figures-tables-and-code.md)で説明します。

## `sections/` 内の本文

テンプレートでは、本文と参考文献の出力を 7 つのファイルに分けています。

```text
sections/
├── 01-purpose.tex
├── 02-theory.tex
├── 03-method.tex
├── 04-results.tex
├── 05-discussion.tex
├── 06-summary.tex
└── 07-references.tex
```

この番号は既定の順序を見やすくするためのものです。実際の出力順序は、`report.tex` 内の各 `\input` の並びで決まります。最初の 6 つの本文ファイルには各セクションの内容を書きます。`sections/` 内のファイルで文書クラスをあらためて宣言したり、`\begin{document}` を追加したりしないでください。

## Release ZIP と GitHub リポジトリの違い

Release ZIP にはクリーンなテンプレートだけが含まれます。GitHub リポジトリでは、さらに次の内容を参照できます。

- `docs/`：この中国語・日本語の説明です。
- `examples/`：完全なサンプルソースとコンパイル済み PDF です。
- `template/`：Release テンプレートに対応するソースです。ブラウザー上でも内容を確認できます。

公開リポジトリには、プロジェクト内部で使うチェック用スクリプトやテストファイルを含めません。通常は Release ZIP だけで利用でき、実装を詳しく確認したい場合に、リポジトリの説明、サンプル、テンプレートソースを参照できます。

## ファイルを移動するときの注意

テンプレートは相対パスでファイルを構成しています。参照関係を変えなければ、テンプレートのルートディレクトリ全体をリネームできます。ただし、`report.tex` だけを移動したり、`config/`、`sections/`、`figures/`、`bibliography/` を任意に分けたりしないでください。ルートにある `biblatex-jareport.sty` と `biblatex-jareport-japanese.lbx` も、`report.tex` と同じ階層に置いておく必要があります。ディレクトリ構成を変更する場合は、`report.tex` の対応する `\input`、`\graphicspath`、`\addbibresource`、または TeX のファイル検索パスも同時に変更してください。

---

[← 前へ：クイックスタート](03-quick-start.md) | [次へ：レポートの基本情報 →](05-metadata.md)
