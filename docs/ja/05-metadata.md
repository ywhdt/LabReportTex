# レポートの基本情報

[プロジェクトトップ](../../README.md) · [ドキュメント目次](README.md) · [中文](../zh-CN/05-metadata.md)

## 記入する場所

各レポートの情報はすべて [`config/metadata.tex`](../../template/config/metadata.tex) にまとめています。テンプレートをダウンロードした後は、コマンド名を残したまま波括弧の中だけを変更してください。

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

使わない項目は、`\experimentpartner{}` のように空のまま残せます。これならコマンドの形式を変えず、後から記入することも容易です。

`\group{A}` にはテンプレートが自動で「班」を追加するため、`A班` ではなく `A` だけを波括弧に入れてください。テンプレートは項目の長さを検査しません。長い内容は折り返される場合がありますが、コンパイル後に基本情報表とヘッダーの収まりを確認してください。

## 各項目の表示箇所

テンプレートの 1 ページ目には基本情報表があり、すべてのページには共通のヘッダーがあります。各コマンドの用途は次のとおりです。

| コマンド | 内容 | 使用箇所 |
| --- | --- | --- |
| `\title{...}` | レポートの題目 | 1 ページ目の基本情報表、PDF の Title 属性 |
| `\author{...}` | レポートの作成者 | 1 ページ目の基本情報表、ヘッダー、PDF の Author 属性 |
| `\course{...}` | 科目または実験科目 | ヘッダー左側 |
| `\theme{...}` | 実験テーマの番号 | ヘッダーの「テーマ」 |
| `\studentid{...}` | 学籍番号 | ヘッダーの「学籍番号」 |
| `\group{...}` | 班 | ヘッダーの「班」 |
| `\experimentpartner{...}` | 共同実験者 | 1 ページ目の基本情報表 |
| `\staff{...}` | 担当教員または TA | 1 ページ目の基本情報表 |
| `\experimentdate{...}` | 実験日 | 1 ページ目の基本情報表 |
| `\place{...}` | 実験場所 | 1 ページ目の基本情報表 |
| `\experimenttime{...}` | 実験の開始・終了時刻 | 1 ページ目の基本情報表 |
| `\reportfirstdue{...}` | 初回提出日 | 1 ページ目の基本情報表 |

1 ページ目の基本情報表とヘッダーは [`config/commands.tex`](../../template/config/commands.tex) で定義しています。

## 題目・作成者と PDF メタデータ

`\title` と `\author` はページ上に表示されるだけでなく、生成される PDF の Title 属性と Author 属性にもそれぞれ保存されます。`metadata.tex` で利用者が記入する項目のうち、ほかの項目は PDF ページ上の表示にだけ使われ、この 2 つの PDF 属性には対応しません。

題目と作成者には、装飾用のコマンドを含まない、単純な文字列をできるだけ使ってください。複雑な組版コマンドは PDF 文字列へ安全に変換できないことがあり、Hyperref の警告につながる場合があります。

PDF にはさらに、LaTeX の処理系が言語、Creator、Producer、作成時刻、ソースファイル名などの技術情報を自動的に記録します。そのため、「題目と作成者を PDF メタデータに保存する」といっても、PDF にこの 2 項目しかないという意味ではありません。

テンプレートの先頭にある `\DocumentMetadata{lang=ja-JP,tagging=off}` は、文書の言語を日本語に指定し、Tagged PDF の出力を無効にします。また、TeX Live 2026 の新しいリストキーを有効にするためにも使われます。題目や作成者を変えるために、この宣言を削除しないでください。

コンパイル後は、システムの PDF 文書プロパティで題目と作成者を確認できます。端末では次のようにも確認できます。

```console
pdfinfo report.pdf
```

## サンプルとの対応

記入済みの例は[サンプルの metadata.tex](../../examples/source/rc-low-pass-filter/config/metadata.tex)にあり、コンパイル後の 1 ページ目は[サンプル PDF](../../examples/compiled/rc-low-pass-filter.pdf)で確認できます。機能一覧の `META-TITLE-01`、`META-AUTHOR-01`、`META-HEADER-01`、`META-TABLE-01`、`META-TITLE-AUTHOR-01`、`DOCMETA-01`、およびその他の `META-*` 項目には、各項目のソース位置と PDF ページ番号が記録されています。詳しくは[サンプル機能一覧](../../examples/FEATURES.md)を参照してください。

---

[← 前へ：プロジェクトのディレクトリ構成](04-project-structure.md) | [次へ：本文とセクション構成 →](06-document-structure.md)
