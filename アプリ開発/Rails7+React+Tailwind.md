# 参考

[Railsにesbuildを使ってReactを導入する(Dockerで構築)](https://zenn.dev/naoki0722/articles/272ef57c6dafba)

[Rails 7 + React + tailwind のプロジェクト新規作成（esbuild）](https://zenn.dev/s10akir/scraps/bb9fc52a14a773)

[RailsとReactを利用するならばimportmap-railsを避けた方が良さそう](https://www.tmp1024.com/importmap-rails-and-jsbuilding-rails/)


# importmap-railsはReactとの相性びみょ


> 私は、RailsでWebアプリケーションを作る際に、JavaScriptのライブラリとしてReactを利用しました。しかし、このReactを利用していて気づいたことがありました。

> それは、ReactでHTMLを生成する際に便利なJSXが使えないということです。なぜ使えないのかというと、importmap-railsでは、トランスパイルを行いJSXの記述を変換しないためです。

> Railsでは、importmap-rails以外にも、JavaScriptを利用する方法を提供しています。そのひとつにjsbuilding-railsというGemを使う方法があります。こちらは、Node.jsの環境が必要となりますが、Webpackやesbuildなどのビルド環境を選択でき、それのインストールと一緒に設定まで行います。


> 読んでいただければわかりますが、React では基本 jsx(or tsx)で記載をしていき、jsx は JavaScript にトランスパイルをしてあげなければいけませんので厳しいですね。

> ですので、React を使うのでしたら、「jsbundling-rails」の一択になりそうです。


