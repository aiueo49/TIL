PF制作におけるメモ。


# 環境

Back  : Rails
Front : React
CSS   : Tailwind(daisyUI)
DB    : PostgreSQL
      : Redis
env   : Docker
PaaS  : Fly.io


# 流れ

[開発の流れの参考に](https://nekorails.hatenablog.com/entry/2019/09/11/145136)

環境構築
デプロイ
CI/CD
アプリのコードを書く

README整備
OGP
独自ドメイン取得
Googleアナリティクス
ロゴ・ファビコンを作る
サイトマップの設定
metaタグを設定
利用規約・プライバシーポリシーを作る

PageSpeed Insightsで表示速度を測定する



## 1. 環境構築

### [Dockerを使ったRailsの環境構築方法](https://school.runteq.jp/v2/mypage/helps/articles/build_docker_environment_rails?gretel_word=Docker)

### [Railsにesbuildを使ってReactを導入する(Dockerで構築)](https://zenn.dev/naoki0722/articles/272ef57c6dafba)



## 2. デプロイ

### [Fly公式日本語ガイド](https://fly.io/docs/rails/getting-started/)

### [Herokuの代替として注目のFly.ioでアプリケーションをデプロイする](https://zenn.dev/hokawa/articles/65ddcd9974448c)



## 3. CI/CD

テストコードの自動化もやりたいけどテストコードを作るのが億劫だ。

デプロイの自動化はやる。

### [【入門】GitHub Actionsとは？概要やメリット、使用例まとめ](https://www.kagoya.jp/howto/it-glossary/develop/githubactions/)

### [GitHub ActionsでFly.ioへのデプロイを自動化する手順](https://zenn.dev/hokawa/articles/956910030b56f2)



## CSS

[Ms.Luka's_daisy_notion](https://www.notion.so/CSS-7da5dc4a5d804a34baa13ffea60ad09e?pvs=4)


## アプリケーション制作

