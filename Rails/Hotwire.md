とても詳しく書かれているサイトを見つけたので備忘。

[Hotwireの良かった点、辛かった点、向いているケース、向いていないケース](https://nekorails.hatenablog.com/#Hotwire%E3%81%AE%E3%83%87%E3%83%A2)

[猫でもわかるHotwire入門 Turbo編](https://zenn.dev/shita1112/books/cat-hotwire-turbo)


# Hotwireとは？

まずはHotwireの簡単な説明を少しだけ（こちらの本のまとめです）。

HotwireはRails7からRailsのフロントエンドのデフォルトとなった技術です。TurboとStimulusという2つのJSのフレームワークから構成されます。Hotwireはこの2つライブラリの総称です。

TurboはTurbo Drive、Turbo Frames、Turbo Streamsという3つの技術から構成されるため、Hotwireの登場人物は以下のようになります。

Turbo
Turbo Drive
Turbo Frames
Turbo Streams
Stimulus

（実際にはモバイルアプリ開発のためのTurbo NativeとStradaという技術もあるのですが、この記事ではWeb開発のための技術に絞ります。）


# Turbo Driveとは？

Turbo Driveは画面遷移を高速にしてくれる技術です。

Turbo DriveはTurbolinksの名前を変えたもので、基本的な機能はTurbolinksと同じです。

リンク、フォームのリクエストをTurbo Driveがインターセプトして、fetchによる非同期リクエストにしてくれます。そしてレスポンスされたHTMLの<body>要素だけを抜き出して、現在のページの<body>要素を差し替えてくれます。

通常の画面遷移がHTMLを丸ごと変えるに対して、Turbo Driveでの画面遷移は<body>だけを置換してくれます（正確には<body>の置換に加えて、<head>の一部がマージされます）。これの何が嬉しいかと言うと、画面遷移しても今のページのCSS・JSをそのまま利用できるため、CSS・JSを初期化してページに適用する処理をスキップできます。これによって画面遷移が高速になります。


# Turbo Framesとは？

Turbo FramesはTurbo Driveの部分置換版です。

Turbo Driveが<body>要素全体を差し替えるのに対して、Turbo Framesは<turbo-frame>...</turbo-frame>というカスタム要素で囲った箇所だけを差し替えます。

画面の一部だけしか更新しないような場合には、Turbo Driveの代わりにTurbo Framesを使うことで高速化できます。


# Turbo Streamsとは？

Turbo Streamsは複数箇所のDOMを同時に更新できます。

Turbo Framesで更新できるのは<turbo-frame>で囲った1箇所だけという制約があります。そのため複数箇所を同時に更新したい場合にはTurbo Streamsを使います。

上のGIFでは編集（edit.html.erb）部分を詳細（_cat.html.erb）に差し替えて、さらにFlash（_flash.html.erb）を新しいものに差し替えています。

あとTurbo FramesがDOMの置換しかできないのに対して、Turbo StreamsではDOMの追加・更新・削除をすることが可能です。

他にもActionCable（WebSocket）と組み合わせて使うことで、チャットのようなリアルタイムなアプリケーションを作れたりします。


# メリット

### サーバーサイドに集中できる

### Railsの資産をフルに活かせる

### 後付けで段階的にSPA風の挙動を追加できる

### 学習コストが低い

### 開発コストが低い

### WebSocketは必須ではない


# デメリット

### DOM更新時にレスポンスを待たないといけない

### SPAのユーザー体験とはだいぶ違う

### Herokuを使いづらい

### TypeScriptを使いづらい

### Stimulusが難しい

### 日本語の情報が少ない

### partialだらけになる

### 分業は難しい

### 後からSPAにするのは辛そう




# 他向き不向きなども。詳しくはサイトで見るべし。


