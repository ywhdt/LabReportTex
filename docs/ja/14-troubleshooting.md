# よくある質問とトラブルシューティング

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/14-troubleshooting.md)

問題を調べるときは、最初の失敗で生成された `report.log` をまず保存してください。LaTeX では、ファイル不足、波括弧の不足、未定義コマンドといった先に現れた一つの問題から、その後のエラーが連続して発生することがよくあります。ログにある最初の実質的なエラーから直します。

`report.tex` が直接見えるディレクトリで次の完全なコマンドを実行すると、LuaLaTeX、Biber、必要な再コンパイルを Latexmk に任せられます。

```console
latexmk -lualatex -interaction=nonstopmode -halt-on-error -file-line-error report.tex
```

`-file-line-error` は、可能な限りエラーのファイル名と行番号を表示します。`-halt-on-error` は、最初の致命的エラーで停止します。最初の問題を修正してからもう一度実行し、コマンドが成功するまで繰り返してください。最後に、致命的ではない組版警告とフォント警告もログで確認します。

## コンパイルできない

次の順序で原因を絞り込みます。

1. 現在のディレクトリに `report.tex`、`config/preamble.tex`、`config/metadata.tex`、`biblatex-jareport.sty`、`biblatex-jareport-japanese.lbx` があることを確認します。`sections/*.tex` を単独でコンパイルしないでください。
2. 実際に呼び出しているツールチェーンがサポート対象か確認します。

   ```console
   latexmk --version
   lualatex --version
   biber --version
   ```

3. このページの冒頭にある完全なコマンドでコンパイルし、コンソールに最初に出る、ファイル名と行番号を伴うエラーを記録します。その後で `report.log` の前後を確認します。
4. パッケージ、ファイル名、ディレクトリを直前に変更した場合は、まず最後に変更した一種類だけを元に戻します。他の設定を同時に変更し続けないでください。
5. ログを保存した後にだけ、補助ファイルを消します。

   ```console
   latexmk -c report.tex
   latexmk -lualatex report.tex
   ```

   `latexmk -c` は一般的な補助ファイルを削除し、`report.pdf` は残します。ソースのエラーを直すコマンドではありません。削除後も同じ位置で失敗する場合は、ログへ戻ってソースを調べてください。

ログでよく見られるパターンは、次のように判断できます。

| ログの文字列 | よくある原因 | 最初に確認すること |
| --- | --- | --- |
| `! LaTeX Error: File '...sty' not found.` | TeX Live にパッケージがない、またはローカルのスタイルファイルを移動した | `kpsewhich` でパッケージを確認します。このプロジェクトの二つの `biblatex-jareport` ファイルは `report.tex` と同じ階層に置きます。 |
| `Undefined control sequence` | コマンドの綴り間違い、オプションモジュールが無効、定義ファイルが未読、読み込み順序の誤り | エラー行のコマンド名、`config/preamble.tex`、`report.tex` の `\input` 順序 |
| `Runaway argument?`、`File ended while scanning use of ...` | それ以前に `}`、`]`、`\end{...}`、引数の区切りが不足している | エラー行より前で最後に変更した環境とコマンド |
| `Environment ... undefined` | 必要なパッケージまたはモジュールが無効、環境名の綴り間違い | [独自コマンドと環境](07-commands-and-environments.md)とモジュールの有効化箇所を確認します。 |
| `Emergency stop`、`Fatal error occurred` | これより前に致命的エラーがある | 上へ戻って最初の `!` エラーを探し、この二行を根本原因と考えないでください。 |
| `Overfull \hbox`、`Overfull \vbox` | 内容が本文領域または確保した高さからはみ出している | ログの行番号、長い URL、幅広の表、コード行、ヘッダー、画像幅 |

`Overfull` は通常、コンパイルコマンドを失敗させませんが、文章、表、図が本文領域からはみ出している可能性を示します。提出前には解消し、PDF が生成されたという理由だけで組版が正しいと判断しないでください。

## フォントが見つからない、または文字化けする

最初に、誤ったエンジンを使っていないか確認します。テンプレートがサポートするのは LuaLaTeX だけです。pdfLaTeX や XeLaTeX が一部のソースを読み込めても、出力がサポート対象とは限りません。次に、文書クラス、LuaTeX-ja、標準フォントを現在の TeX 環境から見つけられるか調べます。

```console
kpsewhich ltjsarticle.cls
kpsewhich luatexja-preset.sty
kpsewhich HaranoAjiMincho-Regular.otf
```

パスが出力されなければ、現在のターミナルが使う TeX 環境から、そのファイルを見つけられていません。エディタではコンパイルでき、ターミナルではできない場合は、両方で実際に呼び出している `latexmk`、`lualatex`、`kpsewhich` のパスを比較してください。画面に表示されるバージョン名だけを比べても不十分です。

ログの `Missing character:` は、現在のフォントに該当する字形がないことを示し、必ずしも UTF-8 ファイルの破損を意味しません。まず文字を記録し、それがテンプレートのサポートする日本語組版の範囲にあるか判断します。文字化けがソースエディタだけで起きる場合は、ファイルが UTF-8 で保存されていることも確認してください。一部の漢字が表示できても、長い中国語本文は現在のサポート範囲外です。

フォントプリセットを変更してから問題が起きた場合は、まず次へ戻します。

```tex
\usepackage[deluxe]{luatexja-preset}
```

`luatexja-preset` の読み込み行を二つ同時に残さないでください。元に戻してから完全なコンパイルをもう一度実行します。標準設定が正常なら、[日本語組版とフォント](11-languages-and-fonts.md)の手順で候補プリセットとフォントファイルを一項目ずつ調べてください。サンプルの日本語、欧文、数学の混植は [`CORE-JAPANESE-01`](../../examples/FEATURES.md) に対応します。問題が TeX 環境全体にあるのか、自分のレポートの特殊文字にあるのかを判断する基準にできます。

## Biber と参考文献のエラー

`lualatex` または `biber` を単独で 1 回実行して、Latexmk の代わりにしないでください。正しい実行方法は次のとおりです。

```console
latexmk -lualatex report.tex
```

`Citation '...' undefined` が出る場合、Biber の再実行を求められる場合、参考文献の見出しが表示されない場合は、次の順に確認します。

1. 本文で実際に `\cite{key}` または `\nocite{key}` を使っているか。`.bib` に項目を追加しただけでは、参考文献一覧に入りません。
2. `key` が `bibliography/references.bib` の項目種別に続くキーと、大文字・小文字も含めて完全に一致しているか。
3. `.bib` にカンマの不足、対応の取れていない波括弧、重複キーなどの構文エラーがないか。
4. `report.tex` に `\addbibresource{bibliography/references.bib}` が残っているか。そのパスが主ファイルのディレクトリを基準に有効か。
5. `report.tex` と同じ階層に `biblatex-jareport.sty` と `biblatex-jareport-japanese.lbx` が残っているか。
6. `report.blg` の最初の Biber `WARN` または `ERROR` が、どのキーやフィールドを指しているか。

Biber が `report.bcf` を見つけられないと表示する場合は、LuaLaTeX で制御ファイルを先に生成していないか、現在のディレクトリが間違っていることが一般的です。主ファイルのディレクトリへ戻り、最初から Latexmk に処理を任せてください。日本語文献が予期せず IEEE 形式になった場合は、次を正確に記入しているか確認します。

```bibtex
langid = {japanese}
```

プロジェクトが日本語書式を選ぶのは、11 種類の項目種別が厳密に `japanese` と指定された場合だけです。それ以外は基本スタイルへ戻ります。完全なデータインターフェース、条件付き参考文献一覧、既知のフィールドのフォールバックは、[参考文献](10-bibliography.md)で説明しています。サンプルの対応項目は [`BIB-JAPANESE-01`](../../examples/FEATURES.md)、[`BIB-EN-FALLBACK-01`](../../examples/FEATURES.md)、[`BIB-CONDITION-01`](../../examples/FEATURES.md) です。

## 画像とパスのエラー

ログに `LaTeX Error: File '...' not found.` と出た場合は、まず `report.tex` のあるディレクトリでコンパイルしているか確認してください。テンプレートの相対パスは、この主ファイルの位置を基準にします。一つ上のディレクトリから実行すると、`config/`、`sections/`、`figures/`、`bibliography/` を解決できない場合があります。

外部画像の標準ディレクトリは、次の行で設定します。

```tex
\graphicspath{{figures/}}
```

画像を `figures/` へ置いた後は、`\includegraphics{ファイル名}` で読み込めます。ファイル名を確認するときは、大文字・小文字、拡張子、全角・半角文字に注意してください。ある OS で偶然読み込めた、大文字・小文字が一致しないパスが、大文字・小文字を区別するファイルシステムでは失敗する場合があります。LuaLaTeX は一般的な PDF、PNG、JPEG を直接扱えますが、SVG などは現在の設定だけでは直接読み込めません。

ファイルが存在してもレイアウトが不自然な場合は、`width`、回転、切り抜きのオプションを確認します。画像幅を現在の行幅以下に収めるのが、一般的に安全な書き方です。

```tex
\includegraphics[width=0.8\linewidth]{measurement-result.png}
```

幅広の図、横並びのサブ図、`[H]` で固定したフロートは、`Overfull \hbox` やページ内の大きな空白を起こす場合があります。外部画像、画像ディレクトリ、サブ図の用例は、それぞれ [`FIG-IMAGE-01`](../../examples/FEATURES.md)、[`GRAPHICSPATH-01`](../../examples/FEATURES.md)、[`FIG-SUBFIGURE-01`](../../examples/FEATURES.md) に対応します。サンプル PDF 第 8～9 ページと比較できます。

## 番号と相互参照のエラー

PDF に `??` が出る場合や、ログに `Reference ... undefined`、`There were undefined references` と出る場合は、まず `\label` と参照キーが完全に一致しているか確認し、Latexmk に必要な再コンパイルを完了させます。セクション、数式、図表、コードリストをコピーしたときは、重複した `\label` の変更を忘れやすいため注意してください。ログの `multiply defined` は、少なくとも二つの対象が同じラベル名を使っていることを示します。

ラベルを置く位置も結果に影響します。図表の `\label` は `\caption` の後に置きます。`align` では、個別に参照する行ごとにラベルを付けます。セクションのラベルは、対応する `\section` または `\subsection` の直後に置きます。複数の対象を参照するときは番号を手作業でつながず、テンプレートがサポートする `\cref`、`\Cref`、`\crefrange`、`\Crefrange` を使ってください。

テンプレートでは `showonlyrefs` を有効にしています。このため、`\label` があっても、`\eqref` またはサポートされる Cleveref コマンドから一度も参照されない数式が、番号なしになるのは正常です。通常形式、スター付き形式、範囲形式の互換処理は `config/preamble.tex` にあります。一つの番号問題を直すために `cleveref` をもう一度読み込んだり、これらのコマンドを上書きしたりしないでください。正常な動作は [`MATH-SHOWONLYREFS-01`](../../examples/FEATURES.md)、[`REF-STAR-01`](../../examples/FEATURES.md)、[`REF-RANGE-01`](../../examples/FEATURES.md) で確認できます。

参照文字列が正しく、色付きの枠がない場合もエラーではありません。テンプレートは `hyperref` に `hidelinks` を指定しています。リンクは存在しますが、枠を表示しません。番号付けの方針と各参照形式の詳しい境界は、[数式と相互参照](09-math-and-cross-references.md)を参照してください。

---

[← 前へ：テンプレートのカスタマイズ](13-customization.md) | [ドキュメント目次へ戻る →](README.md)
