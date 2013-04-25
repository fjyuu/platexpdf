# platexpdf #

platex して dvipdfmx するスクリプト

## 使い方 ##

Perlが動作する環境で本スクリプトをPATHの通ったところに置くと以下のよう
にコマンドとして使えます．

    platexpdf hoge.tex

`hoge.tex`を引数にとり，DVIファイル及びPDFファイルを一度に作成していま
す．

Windowsで[ActivePerl](http://www.activestate.com/activeperl)を使用して
いる場合は，コマンドラインから

    pl2bat platexpdf

で，`platexpdf.bat`を作成して，それをPATHの通ったところに置いてください．

## 詳細 ##

本スクリプトはTeXファイルを引数にとりPDFファイルを作成するスクリプトで
す．内部では，`platex`コマンド，`dvipdfmx`コマンドを使っています．つま
り，

    platexpdf hoge.tex

は，

    platex hoge.tex
    dvipdfmx hoge.dvi

とふたつのコマンドを実行することにほぼ等しいです．

## オプション ##

本スクリプトは，いくつかのオプションを指定することができます．

* `--bibtex / -b`

  bibTeXを使うときに指定します．このオプションを指定しない場合でも，
  `*.bib`ファイルがあるときには自動的にこのオプションが有効になります．
  この動作を抑制するときには，`--no-bibtex`オプションを使用してください．

* `--fontmap FILENAME / -f FILENAME`

  フォントマップファイルを指定します．`dvipdfmx`の`-f`オプションに相当
  します．

* `--kanji ENCODING / -k ENCODING`

  TeXファイルの文字コードを明示的に指定します．`platex`の`-kanji`オプショ
  ンに相当します．(ENCODING=euc|jis|sjis|utf8)

* `--papersize PAPERSIZE / -p PAPERSIZE`

  用紙サイズを明示的に指定します．`dvipdfmx`の`-p`オプションに相当しま
  す．(ex. a4)

* `--clean EXTENSION / -c EXTENSION`

  削除したい中間ファイルの拡張子を指定します．

* `--help / -h`

  ヘルプを表示します．

`--fontmap`オプションと`--clean`オプションは以下のように複数指定するこ
とができます．

    platexpdf --clean log --clean dvi hoge.tex

## 注意 ##

Adobe ReaderでPDFファイルを開いている状態でplatexpdfを実行してもPDFファ
イルを更新できません．これは，Adobe Readerがファイルをロックしているた
めです．

platexpdf実行時にはPDFビューアを閉じるか，ファイルをロックしないような
PDFビューアを使用するようにしてください．たとえば，Windowsでは，
[Sumatra PDF][1]というPDFビューアがあります．Linuxでは，evince等を使え
ば大丈夫です．

[1]:http://blog.kowalczyk.info/software/sumatrapdf/free-pdf-reader.html
