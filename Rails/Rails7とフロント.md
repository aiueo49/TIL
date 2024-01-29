# どうやらRails7以降とフロントエンド(JS)の相性？が悪い？

Rails7以降ではJSの力をかりずしてモダンなWebアプリを開発しようという流れがきているらしい。


# 参考サイト

[今更調べてみる Ruby on Rails7 フロントエンド周りの選択肢(と個人的な採用方針)](https://qiita.com/jonakp/items/a80f53f082a974765e2c)



# 選択肢(以下抜粋)

要約するとRails7のフロントエンドは大まかに3つの選択肢を提供している。

1つ目。これは前述のJSのバンドルやトランスパイルを使わない道。Webpackerの代わりにimport maps(importmap_rails) & Hotwireを用いる。

2つ目。従来通りWebpack等を利用してJSのバンドルやトランスパイルを行う道。ただしWebpackerは推奨されていないようだ。Rails7ではjsbundling-rails 
というgemが用意されており、esbuild, rollback, 
webpack等から自分で選択する。推奨されていないだけでWebpackerも利用はできる。ただし今後バージョンアップは行われなくなり、後継としてShakapackerが登場するらしい。

3つ目。Railsは単なるAPIとして機能する(いわゆるAPIモード)。フロントエンドは別に好きなものを用意する。


# 所感

従来通りの2つ目を採用する。
今後の動向を見て1つ目に移行することになるかも？


