
# 参考サイト

[初心者による初心者のためのRedis解説 - Qiita](https://qiita.com/keinko/items/60c844bcf329bd3f4af8)

# **Redisとは(導入部)**

最初に結論っぽいことをまとめます。

Redisとは、

- 無料で使えるデータベース管理システムの一つ
- 高速にデータを処理することができる、という特徴がある
- データベースの種類としては"NO SQL"というものに分類される

# **RDBMSとRedisの比較**

Redisについて解説する前に、まずは両者の仕組みをイメージしやすいような形で図式化します。

## **RDBMSの仕組みのイメージ**

左の男がみなさん、そして机、本棚、司書さんをまとめてデータベースとします。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2F6801056b-73f7-7fff-57cb-d9eaf175e8d4.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=176f1a698217e3018a9f0719caf5624c

皆さんは好きな本を司書さんに持ってくるように頼み、机に出してもらって本の中身を見ることができます。

このとき、複数の本を持ってきてもらって複数見ることも可能です。

「1995年にベストセラーに選ばれた本全てと、それらの本の著者の別年代に出版された本も全てもってきて」なんていう無茶振りにも司書さんは応えてくれます。

ただ、司書さんはしっかり確実に仕事を行うタイプで仕事のスピードは速いとは言えません。

本を見終わったあとは本棚に戻してもらい、司書さんが安全に管理してくれます。

### **RDBMSの長所・短所**

＜長所＞

- 安全にデータを管理できる
- 検索機能が豊富

＜短所＞

- 処理が遅い

## **Redisの仕組みのイメージ**

基本的には本棚は存在せず、本を机に置きっ放しで管理するのがRedisです。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2Fac7c2c40-b03d-e2ad-4f7e-c341e17f5189.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7b94debeff0f612bcd610b27b63b79ea

一応、それぞれのデータは少しはまとまって整理されています。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2F29a4c282-f002-6675-ac26-5c03b3f75062.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ffe77048089b6ea039bae0f4753e7ba7

例えば、「シリーズものの本はシリーズ順にまとめて紐で縛ってある」とか「とりあえず順番とかいう概念はないけど、関連性のありそうなものを袋にまとめておく」などです。

読みたい本を読むときは、探したい本がその机のどこにあるかは精通している机マスターにお願いしてとってもらいます。

机マスターはとても俊敏に行動するタイプの人です。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2F83666307-b68d-0ec0-e4fa-e092d34d9d44.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=326f76db8e8d7b30289b1cb52b19ddfd

本を置くスペースがなくなってきたら、別の机を用意し、別の机マスターを雇って管理します。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2Fd2eb41f6-aa0a-a515-17fd-b580c4db0c8b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=920396f46693b31b3d14e33c2563feef

やろうとすれば一つの大きなカバンで管理することも可能です。

しかし、読みたい本を高速で持ってきてもらうのがこの管理システムの本質なので、カバンで管理することはあまりないと考えてもいいでしょう。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2Ff60ab5a2-8cde-225a-939b-9897d38a9c63.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6fea6a19d04f122df69ff1c285821af7

### **Redisの長所・短所**

＜長所＞

- 処理が速い

＜短所＞

- 単純なデータしか扱えない
- 複雑な検索は不可能
- 管理は安全でない

## **なぜRedisは速いのか**

今述べたイメージを、より現実のコンピュータの世界の話に近づけて「なぜRedisが速いのか」に関して言及します。

コンピュータの世界では、このように一時的に保存、およびデータを参照する机のようなものを**メモリ**といいます。

一方、本棚のような保存するための場所が**ハードディスクドライブ**です。

(他にも種類はありますが、ここでは簡単のためそう呼ぶこととします。)

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2Fa9b7b70c-66fd-b3eb-1e8f-42e81a61001a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=dd6790d27a27a6279ff1f351bcda70d8

メモリの上にあるものは、コンピュータの電源が落ちてしまえば消えてしまいます。

したがって、データベースは常に電源がついているものですが、万が一電源が切れてしまえばRedisのデータは消えてしまうこととなります。

### **なぜRDBMSは遅いのか**

理由は3つ考えられます。あくまでもRedisと比べて、です。

- メモリとハードディスクの間を行き来する必要があるため
- 複雑な検索の場合、そもそも処理に時間がかかる
- 司書さんの仕事が丁寧だから

3つめについて補足するとRDBMSは安全性を実現させるために、データが消えていないか、データは重複していないか等の確認や、バックアップの作成を司書さんは行なってくれます。このため、一つ一つの処理が重くなります。

### **インメモリデータベース**

Redisのようにメモリの上でデータを管理するデータベースをインメモリデータベースといいます。

特に計測はしたことはありませんが、ハードディスクとメモリでは圧倒的にメモリのほうが速いと考えてください。

そもそもハードディスクのものをメモリに持ってきて処理を行うので当然と言えば当然なんですけれども。

### **つまり**

なぜ速いか= (データ&処理が単純) × (メモリ上で処理を行う)

# **Redisについて**

ここではより具体的にRedisについて見ていくこととします。

## **Redisの仕組み**

少し内容が難しくなるので飛ばしてもらっても構いません。

Redisは複数のコマンドが与えられても、一つ一つ順番に処理を行います。

このような処理を**シングルスレッド**と言います。

このへんの図は[大谷様の資料](https://www.slideshare.net/yujiotani16/redis-76504393)を使わせて頂きます。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2F6498d42c-2cef-b5ce-e9d7-3fd393e3066c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9be024e7887fdb6f1910feb1ddbac028

引用元：[Redisの特徴と活用方法について](https://www.slideshare.net/yujiotani16/redis-76504393)

たとえ同時にコマンドを実行した場合でも、Redisは順番に処理を行なっていきます。

### **Redis cluster(Redisデータ分類機)**

先ほどの図においてRedisと書かれた青い四角の部分が、以下の図の青い点線部分にあたります。

これから説明していくので図に関して疑問に思っても先に進んでください。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2F6c6b9723-5091-eb03-4445-a1509136def8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6d176194fbe0e0830f927f1347b806e0

引用元：[Redisの特徴と活用方法について](https://www.slideshare.net/yujiotani16/redis-76504393)

一つのメモリがもつ容量は少ないため、上図のように複数台のサーバを用意して容量を増やしていきます。

複数台でデータを分散させ、データを保持することを**シャーディング**と呼び、シャーディングを管理するのが**Redis cluster**です。

サーバとデータの関係は一対多対応となっています。

「AというサーバのもつaっていうデータはA以外のサーバB,C,...いずれにも属さない」ということです。

### **slotについて**

Redis clusterの実際の処理としては、まず、データそれぞれにslotと呼ばれるidのようなものを設定し、それぞれ管理するデータベースを分けます。

例えば、「slot0番〜slot5460番はnode1っていうサーバ」、「slot5461番〜slot10922番はnode2っていうサーバ」、「slot10923番〜slot16383番はnode3っていうサーバ」というふうに。

ユーザーは、ある適当なデータベースサーバに「slot〇〇番のデータはありますか」という命令を送信し、ない場合には、データベース同士が通信を行い該当のslot番号のデータを見つけ、そのデータをユーザーに返します。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2F65f32f8e-3a27-9484-eff0-6e6dc26bd94e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=abc523f4fdaca4fad0072df197294007

これがRedisの大まかな仕組みです。

### **実際の構築**

ここで、実際に運用する場合を妄想します。

データがどんどん増えてきたらサーバを増設することによって対応することができます。

Redisにはそれぞれのサーバの担当slotを割り直す機能があります。

これを**リシャーディング**と言います。

しかしながら、いちいちデータが増えてきたら新しいコンピュータを購入してRedisのセッティングをしてデータベースサーバとして運用するのは面倒です。

お金を払えば、そういうことをいい感じに代わりにやってくれるのがAWSのサービスの一つである**Amazon ElastiCache**です。

!https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F208979%2F7a54b256-ba67-883f-647f-1733aaa95bc3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d3c238221660212163b6afb4f75b1401

もし自分で構築することになったらElastiCacheのドキュメントを見てください。

## **データの有効期限設定機能**

少し話は変わりますが、Redisにはデータを自動的に削除する機能が存在します。

これはRedisがメモリ上にデータを保存していく性質上、メモリの容量を削減するためのものです。

有効期限を何秒か後に設定する方法と有効期限を日付で指定する方法があります。

`# "keyname"のkeyのvalueを60秒後に消去
EXPIRE keyname 60

# 上の処理をタイムスタンプ表現の日付で指定
EXPIREAT keyname 802537200`

## **バックアップ作成機能**

インメモリデータベースなので基本的にはメモリの上でデータを管理しますが、バックアップを作成しメモリ外に保存することもできます。

保存の方法は2種類存在します。

同然ですが、これらを有効化すると処理速度が低下してしまいます。

### **秒数指定・回数指定で保存**

保存のタイミングを、「"seconds"秒ごとに、または"changes"回変更されたとき`.rdb`ファイルとして保存する」というように指定する方法です。

`redis.conf`ファイルを変更することによって保存設定を反映できます。

redis.conf

`# 60秒ごとに、または10000回変更がされたときに.rdbファイルとして保存
save 60 10000`

`.rdb`ファイルは次回Redis起動時に自動的に立ち上がるようになっています。

### **更新ごとに保存(AOF方式)**

データベースの更新ごとにバックアップを作成するもの。

redis.conf

`# AOF方式の保存を有効化
appendonly yes

# 常に更新されたら保存
appendfsync always`

# **まとめ**

かなり長くなりましたが、Redisについて今回調べた内容をまとめると以下のようになります。

- (RDBMSよりも、)かなり高速に処理を行える。
- 基本的には消えてもいいデータを保存する。
- (他のNoSQLと比べて、)データの型が豊富に用意されている。
- データの有効制限を簡単に設定できる。
- (インメモリDBだけれども、)データのバックアップをメモリ外にとることができる。
