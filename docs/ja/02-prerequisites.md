# 環境と依存パッケージ

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/02-prerequisites.md)

## LaTeX ディストリビューション

最低対応バージョンは **TeX Live 2026** です。このテンプレートは日本語を LuaLaTeX で処理するため、pdfLaTeX や XeLaTeX ではコンパイルしないでください。TeX Live 2026 をフルインストールしている場合、テンプレートが使う一般的なパッケージは通常すでに含まれています。最小構成で導入している場合は、レポートのディレクトリにパッケージファイルをコピーするのではなく、TeX Live のパッケージ管理ツールで不足分を追加してください。

テンプレートは、`luatexja-preset`、`graphicx`、`geometry`、`tabularx`、`fancyhdr`、`subcaption`、`multirow`、`booktabs`、`float`、`mathtools`、`amssymb`、`xcolor`、`biblatex`、`hyperref`、`cleveref` などを直接読み込みます。TeX Live 2026 のフルインストールには通常これらが含まれています。最小構成の場合は、コンパイルログに従って不足分を追加してください。TikZ、回路図、`mdframed` による枠付き表示、コードリスト、PGFPlots はオプションモジュールであり、後から必要に応じて有効にするときだけ対応するパッケージが必要です。

## コンパイルツール

レポートのコンパイルには、少なくとも次の 3 つのコマンドを利用できる環境が必要です。

- **LuaLaTeX**：実際に組版を行うエンジンです。
- **latexmk**：必要な回数だけ LuaLaTeX を実行し、必要に応じて Biber も呼び出します。
- **Biber**：`bibliography/references.bib` にある参考文献を処理します。

端末で次のコマンドを実行し、すべてがバージョン情報を返すことを確認してください。

```console
lualatex --version
latexmk -v
biber --version
```

いずれかで `command not found` または同様のエラーが表示された場合は、先にインストールまたは `PATH` の設定を直してください。テンプレートの標準的なコンパイル方法は `latexmk -lualatex report.tex` です。異なるエンジンを手作業で混在させたり、`lualatex` を一度だけ実行したりすると、参照や参考文献が更新されないことがあります。

## 必要なパッケージ

`report.tex` にパッケージを一つずつ追加する必要はありません。クリーンなテンプレートでは、中核となるパッケージを [config/preamble.tex](../../template/config/preamble.tex) でまとめて読み込み、ルートファイルがこの設定を自動的に読み込みます。最初のコンパイルで外部パッケージの `.sty` ファイルが見つからないとログに表示された場合は、そのファイルを提供するパッケージを TeX Live のパッケージ管理ツールで導入し、同じディレクトリから標準コマンドを再実行してください。

`biblatex-jareport.sty` と `biblatex-jareport-japanese.lbx` は Release ZIP に含まれるローカルの参考文献スタイルファイルであり、同名の TeX Live パッケージを別途導入するものではありません。どちらかがない場合は元の ZIP から復元し、`report.tex` と同じ階層に置いてください。

オプションモジュールは初期状態では無効です。

```tex
% \input{config/diagrams}
% \input{config/listings}
% \input{config/plots}
```

この 3 行は [config/preamble.tex](../../template/config/preamble.tex) にあります。対応する機能をまだ使わない場合は、コメントのままにしてください。これらのオプションモジュールの使い方は後続の文書で説明するため、クイックスタートの段階で有効にする必要はありません。

## エディタの設定

ローカルの TeX Live を呼び出せるエディタであれば、どれでも使用できます。たとえば、TeXstudio や VS Code（LaTeX Workshop）を利用できます。端末から直接コンパイルしても構いません。重要なのはエディタの種類ではなく、コンパイラを LuaLaTeX に設定し、プロジェクトのルートファイルを `report.tex` にすることです。

エディタでビルドレシピを選べる場合は、次のコマンドと同じ設定を優先してください。

```console
latexmk -lualatex report.tex
```

## 環境の確認

インストールが終わったら、一時ディレクトリまたは Release ZIP を展開したテンプレートのディレクトリで次を実行します。

```console
latexmk -lualatex report.tex
```

成功すると、カレントディレクトリに `report.pdf` が生成されます。内容をまだ記入していない空テンプレートでも PDF を生成できます。

コマンドが失敗した場合は、まず同じディレクトリの `report.log` に最初に出たエラーを確認してください。LuaLaTeX を使っていない、TeX Live のパッケージが不足している、別のディレクトリでコマンドを実行している、といった原因がよくあります。環境を確認できたら、[クイックスタート](03-quick-start.md)に進んでください。

---

[← 前へ：プロジェクト概要](01-overview.md) | [次へ：クイックスタート →](03-quick-start.md)
