# 導入したかったがなかなかうまくいかず

[GitHub Copilotの概要](https://docs.github.com/ja/copilot/using-github-copilot/getting-started-with-github-copilot?tool=vimneovim#prerequisites-3)

[Github Copilot を Vim で使うためのプラグイン「Copilot.vim」を導入する。](https://blog.serverworks.co.jp/copilot_vim)

[filetype - Vim日本語ドキュメント](https://vim-jp.org/vimdoc-ja/filetype.html)

[Vimでモードラインを設定すれば拡張子とは関係なくファイルタイプを指定できる](https://snap.hyuki.net/20141120223813/)

# これがうまくいかない

[![Image from 
Gyazo](https://i.gyazo.com/913623473675220b14a687a14a36f01b.png)](https://gyazo.com/913623473675220b14a687a14a36f01b)


```
:set filetype=ruby
```
をコマンドに打ち込むも、一時的にしか解決しなかった。
(しばらくすると元に戻った)
そもそも、これは拡張子がrubyのものにだけしか対応してくれなさそう。
全ての拡張子でCopilotを使いたいんです。


# これで様子を見てる

[![Image from 
Gyazo](https://i.gyazo.com/8385bddb7bec83bfb3550892e14baeeb.png)](https://gyazo.com/8385bddb7bec83bfb3550892e14baeeb)


# あとは~/.vimrcも少しいじりました。

```
set nocompatible
syntax enable
filetype plugin indent on
```

参考[Vim のすゝめ](https://www.soum.co.jp/misc/vim-no-susume/1/)

