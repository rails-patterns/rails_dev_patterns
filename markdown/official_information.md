公式情報
========

コンテクスト
------------

利用するRailsの機能について調べようとしている。

問題
----

ウェブを検索して見つけた実装方法を真似ても、期待通りに動作しないことがある。
動作させるために更に検索したり試行錯誤したりして、時間を浪費する。

問題の原因
----------

Q&Aサイトの[Stack Overflow](http://stackoverflow.com/)には「ruby-on-rails」のタグがついた投稿が約18万件あり、PHPのウェブアプリケーションフレームワーク「cakephp」の約2万件、Javaのウェブアプリケーションフレームワーク「struts」の約3千件と比べると、Railsに関する情報がウェブ上に比較的豊富にあることが分かる。

一方、Railsは2008年4月から2014年12月までの間に4つのメジャーバージョンと9つのマイナーバージョンをリリースしており、その度に大小の非互換を生んでいる。

以上のことから、Railsに関してはウェブ検索によって情報は得られるが、開発者のバージョンにおいては必ずしも正しくない状況が発生する。バージョンが一致していても、投稿者の誤解などによって間違った情報が掲載されていることもある。

例えば、Rails 3のセキュリティ機能の一つであるattribute_accesibleはRails 4でStrong Parametersに置き換えられたが、ウェブ検索すると2015年1月時点でもattribute_accesibleの使用方法を解説するブログ記事が見つかる。

フォース
--------

* Railsは互換性を犠牲にすることで積極的に新機能を導入したりコードの保守性を保ったりしている。
* 第三者による情報発信を制限したり、常に正しい情報を求めたりはできない。

解決方法
--------

公式サイトを参照する。公式以外の情報を参考にするときは、バージョンに注意する。

例
--

#### 1. プログラマーズ・ガイド

Railsには充実した[プログラマーズガイド](http://guides.rubyonrails.org/)が用意されている。

#### 2. リファレンス・マニュアル

Railsの[リファレンス・マニュアル](http://api.rubyonrails.org/)はウェブ上で公開されている。
また、[http://api.rubyonrails.org/v4.1.0/](http://api.rubyonrails.org/v4.1.0/)のようにURLにバージョン番号を付ければ、指定したバージョンのマニュアルを参照することもできる。

#### 3. リリースノート

バージョンアップによって変更された点は、公式ブログの[リリースに関する投稿](http://weblog.rubyonrails.org/releases/)に書かれている。

#### 4. ソースコード

以上の文書を読んでも不明な点については、GitHubで公開されている[Railsのソースコード](https://github.com/rails/rails)を読むとよい。

結果
----

初めて使う機能であっても期待通りに動作する。オプションなどの副次的な機能についても完全な情報が得られ、より適切に実装できる。