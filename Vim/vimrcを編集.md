# CopilotをVimで使えるようにしたいと思ってググっている時に発見

[第1回 Vim を最小限の設定で使う](https://www.soum.co.jp/misc/vim-no-susume/1/#id8)


以下抜粋。

2 .vimrc を作成する
.vimrc とは Vim 用の設定ファイルです。普通はホームディレクトリの下に置きます [2]。今回は Vim 
のための高度な設定を解説するつもりはないのですが、残念ながら、.vimrc を作成しない限り、Vim は Vim 
として動作しません。せっかく Vim を使用しているというのに、それではあまりに勿体無いので、まずは最小限の .vimrc 
を作成しましょう。私が考える、Vim の最小設定は以下です。

```
$ cat ~/.vimrc
set nocompatible
syntax enable
filetype plugin indent on
```


3 .vimrc の設定を理解する
今回は 3 行しか設定がありませんが、この 3 行はどういう意味の設定なのかについて解説をします。

まず、set nocompatible は Vim を vi 互換モードではなく、Vim として使用するという命令です。Vim 
を使うのなら、基本的に設定しておくべきです。Vim 
に外部プラグインを導入する場合も、この設定がないとプラグインはまともに動作しないでしょう。Vim の vi 
互換モードについては、今後の連載で解説予定です。

次の syntax enable は、Vim のシンタックスハイライト機能をオンにする設定です。Vim には vi 
にはない豊富なシンタックスハイライトを備えています。シンタックスハイライトは見ためが美しくなる、という他にも、キーワードのタイプミスに気付きやすいという実用的な利点があります。例えば、C 
言語で「retrun」と「return」を間違えてもすぐに気付くことができます [3]。

最後の filetype plugin indent on は、Vim のファイルタイプ [4] 
検出と、ファイルタイププラグインとインデントプラグインをオンにする設定です。ファイルタイプの検出とは、ファイルを開いたときにファイルの種類を検出する機能のことを言います。この機能により、Vim 
は .pl ファイルを開いたとしても、それが Prolog ファイルなのか Perl 
スクリプトなのかを判別することができるのです。ファイルタイププラグインとは、ファイルの種類毎に定義された共通の設定のことです。インデントプラグインはファイルタイプ毎に定義されたインデント設定です。

さて、これであなたの Vim は Vim として動作するようになりました。次回からは本格的に Vim 
の機能について解説を始めようと思います。次回は Vim の「バッファとウインドウ」について解説します。
