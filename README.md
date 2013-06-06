# platexpdf #

platex して dvipdfmx するスクリプト

## 概要 ##

本スクリプトはTeXファイルを引数にとりPDFファイルを作成するPerlスクリプ
トです．内部では，`platex`コマンド，`dvipdfmx`コマンドを使っています．
つまり，

    platexpdf hoge.tex

は，

    platex hoge.tex
    dvipdfmx hoge.dvi

とふたつのコマンドを実行することにほぼ等しいです．

## 使い方 ##

Perlが動作する環境で本スクリプトをPATHの通ったところに置くと以下のよう
にコマンドとして使えます．

    platexpdf hoge.tex

`hoge.tex`を引数にとり，DVIファイル及びPDFファイルを一度に作成していま
す．

文字コードを明示的に指定したり，埋め込みたいフォントのフォントマップを
指定することができます．

    platexpdf -k utf8 -f cid-x.map hoge.tex

また，ファイルの変更を監視して，変更されたときに自動でコンパイルを実行
することもできます．この機能を使うには
[Filesys::Notify::Simple](http://search.cpan.org/perldoc?Filesys%3A%3ANotify%3A%3ASimple)
モジュールが必要です．

    # hoge.texが修正されたときに自動でコンパイル
    platexpdf -w -- hoge.tex

    # hoge.texもしくはpiyo.texが修正されたときに自動でhoge.texをコンパイル
    platexpdf -w hoge.tex -w piyo.tex hoge.tex

Windowsで[ActivePerl](http://www.activestate.com/activeperl)を使用して
いる場合は，コマンドラインから

    pl2bat platexpdf

で，`platexpdf.bat`を作成して，それをPATHの通ったところに置いてください．

## オプション ##

本スクリプトは，いくつかのオプションを指定することができます．

* `--bibtex / -b`

  bibTeXを使うときに指定します．

* `--clean EXTENSION / -c EXTENSION`

  削除したい中間ファイルの拡張子を指定します．

* `--fontmap FILENAME / -f FILENAME`

  フォントマップファイルを指定します．`dvipdfmx`の`-f`オプションに相当
  します．

* `--help / -h`

  ヘルプを表示します．

* `--kanji ENCODING / -k ENCODING`

  TeXファイルの文字コードを明示的に指定します．`platex`の`-kanji`オプショ
  ンに相当します．(ENCODING=euc|jis|sjis|utf8)

* `--landscape / -l`

  用紙を横に使うモードでPDFを出力します．`dvipdfmx`の`-l`オプションに相当します．

* `--papersize PAPERSIZE / -p PAPERSIZE`

  用紙サイズを明示的に指定します．`dvipdfmx`の`-p`オプションに相当しま
  す．(e.g. a4)

* `--watch FILE / -w FILE`

  変更を監視するファイルを指定します．FILEを省略した場合は，コンパイル
  対象のTeXファイルを監視します．このオプションを指定すると，ファイルを
  監視し，ファイルが変更されればコンパイルを実行します．このオプション
  を使う場合は，追加モジュールとして
  [Filesys::Notify::Simple](http://search.cpan.org/perldoc?Filesys%3A%3ANotify%3A%3ASimple)
  が必要です．

  Filesys::Notify::Simpleは，プラットフォームごとに
  [Linux::Inotify2](http://search.cpan.org/perldoc?Linux%3A%3AInotify2)，
  [Mac::FSEvents](http://search.cpan.org/perldoc?Mac%3A%3AFSEvents)，
  [Filesys::Notify::KQueue](http://search.cpan.org/perldoc?Filesys%3A%3ANotify%3A%3AKQueue)，
  [Win32::ChangeNotify](http://search.cpan.org/perldoc?Win32%3A%3AChangeNotify)
  を使い分けます．プラットフォームに合ったモジュールをインストールする
  ことで，ファイルの監視を効率的に行えるようになります．

`--clean`オプション，`--fontmap`オプション，`--watch`オプションは以下の
ように複数指定することができます．

    platexpdf --watch piyo.tex --watch hoge.tex --clean log --clean dvi hoge.tex

## 注意 ##

Adobe ReaderでPDFファイルを開いている状態でplatexpdfを実行してもPDFファ
イルを更新できません．これは，Adobe Readerがファイルをロックしているた
めです．

platexpdf実行時にはPDFビューアを閉じるか，ファイルをロックしないような
PDFビューアを使用するようにしてください．たとえば，Windowsでは，
[Sumatra PDF][1]というPDFビューアがあります．Linuxでは，evince等を使え
ば大丈夫です．

[1]:http://blog.kowalczyk.info/software/sumatrapdf/free-pdf-reader.html
