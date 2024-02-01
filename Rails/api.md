# 参考


## [「Rails API とは？普通の Rails と何が違うの？」と思った人がまず読む記事](https://zenn.dev/ddpmntcpbr/articles/about-rails-api)

## [Railsで超簡単API](https://qiita.com/k-penguin-sato/items/adba7a1a1ecc3582a9c9)

## [RailsでWeb APIを作成する方法とメリット🤔💭](https://qiita.com/digitter/items/53f83ce50036b2773d55)


# Rails + React 構成のやり方をググっている時のこと


やたらapiモードの話が出るなと思ったら、Reactを使うならRailsはapiモードのが良いらしい。

apiモードとは、MVCのVを捨てて、VをReactに丸投げするためのモード。

フロントとバックを疎結合にして、完全に分業させるということらしい。

確かに、Vの中にReactいちいち呼んでたらコードがごちゃごちゃしそうだし、

Reactを使うメリットであるSPAも生かせなさそう。（適当


# APIをハンズオンで試す

20240201 実施。要は別々のポートで、RailsとReactを立ち上げ、データをRailsからReactに渡している。
思ってたより簡単だった。ハンズオンの資料ありがたし。(上記RailsでWebAPIを作成する方法とメリット)
