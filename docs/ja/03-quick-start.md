# クイックスタート

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/03-quick-start.md)

## 空テンプレートを用意する

通常の利用者は、公開リポジトリの Release ZIP をダウンロードするだけでよく、Git をインストールしたりリポジトリをクローンしたりする必要はありません。[LabReportTex Releases](https://github.com/ywhdt/LabReportTex/releases) で必要なバージョンを選び、`LabReportTex-template-v<version>.zip` の形式のテンプレート圧縮ファイルをダウンロードしてください。

ZIP には単独でコンパイルできるクリーンなテンプレートが入っています。開発リポジトリのサンプルを空テンプレートに上書きしないでください。

## レポート情報を記入する

`config/metadata.tex` を開き、まず題目、作成者、授業で指定された項目を記入します。次の内容はテンプレートにあるコマンドと同じであり、初回の記入例として使えます。

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

上記の項目は、項目ごとに 1 ページ目の基本情報表またはヘッダーへ表示されます。このうち `\title` と `\author` は PDF メタデータにも保存されます。項目と表示箇所の対応は、[レポートの基本情報](05-metadata.md)で詳しく説明します。

## 本文を作成する

本文のファイルは `sections/` にあります。既定の順序は目的、原理、実験方法、実験結果、考察、まとめ、参考文献です。最初の 6 つの本文ファイルには日本語のセクション見出しと 1 行のコメントがあり、参考文献の見出しは有効な引用がある場合だけ生成されます。まずは既存のセクション見出しの下でコメントを置き換えるか、自分の文章を追加してください。たとえば `sections/01-purpose.tex` を次のように編集します。

```tex
\section{目的}
本実験では，RC回路の周波数特性を測定し，理論値と比較する。
```

ルートファイルの [report.tex](../../template/report.tex) が、これらのファイルを順に読み込みます。

## コンパイル

テンプレートのルートディレクトリ、すなわち `report.tex` がある場所で実行します。

```console
latexmk -lualatex report.tex
```

`latexmk` は LuaLaTeX を使用し、参考文献や相互参照の更新に必要な回数の再コンパイルを自動的に行います。

## 出力を確認する

コンパイルが完了すると、ルートディレクトリに `report.pdf` と各種補助ファイルが生成されます。

---

[← 前へ：環境と依存パッケージ](02-prerequisites.md) | [次へ：プロジェクトのディレクトリ構成 →](04-project-structure.md)
