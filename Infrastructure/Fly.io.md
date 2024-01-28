# 参考サイト

[Fly.io](https://fly.io/)

[サヨナラHeroku 〜アプリケーションの知識だけで本番稼働を実現できる無料のプラットフォームを追い求めて〜](https://zenn.dev/imah/articles/a41e889dbf54da)

[ゆみんぐ　～Yumi+Programming～](https://enrichmylife.hatenablog.jp/entry/2023/02/01/224447)

[Fly.ioでRailsアプリをデプロイしてみた for Mac](https://qiita.com/yuuki-h/items/ec23de152368d36672ea)

[Fly.ioであなたのアプリをオンラインにあげよう](https://railsgirls.jp/deployment/fly-io)

[Fly.ioにデプロイしてみた](https://qiita.com/kmkkiii/items/f07803ddaab267e1a407)

[Rails on Fly.io](https://fly.io/docs/rails/)


# 概要

オルタナティブHerokuを謳っているプラットフォームの中で、無料枠があってポートフォリオを公開するのに適していそうなプラットフォームのひとつ。


# Rails7との組み合わせ🙆

> Fly.ioにした理由
> 当初、自作サービスのプラットフォームはHerokuを予定していましたが、2022.11.28に無料プランの提供を廃止するという発表があったため、別のプラットフォームも含めて検討しました。

> Herokuのほか、Render、Railway、Fly.ioを検討対象にしていましたが、作成している自作サービスがHotwireを使用しているので、Regionの影響を大きく受けるのが判明しました。

> 参考[Herokuを使いづらい](https://nekorails.hatenablog.com/entry/2022/05/16/170434#Heroku%E3%82%92%E4%BD%BF%E3%81%84%E3%81%A5%E3%82%89%E3%81%84)

> Heroku以外の、Render、Railway、Fly.ioの対応Regionsを調べたところ、一番近いRegionsは、Renderがシンガポール、Railwayは西アメリカ、Fly.ioが東京でした。

> Fly.ioはRailsアプリをデプロイする記事あり問題なさそうだったので、Fly.ioで進めることにしました。


フロントにHowwireを使う場合、Herokuと相性が悪いらしい。
フロントにReactなどを採用すれば問題はないのかもしれないが、途中で「やっぱReactやーめた。Howwireでやろ。」はできなさそう。

Fly.ioはそんなことないよというお話。


# CLI操作必須

設定はCLIで行うらしい。
CLI操作に抵抗があるとデメリットかも。
個人的には問題なし。


# 所感

強みが多い。
無料枠。
Redisが使えること。

個人開発のPFをデプロイする際の有力選択肢 （トレンド？）ではありそう。

特に欠点が見当たらない。
強いていうなら日本語ドキュメントが少なそう？

総じて良いとは思う。
