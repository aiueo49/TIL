# git add . ~ git push までが手間
いちいち、
git add .
git commit -m ''
git push
を入力するのが面倒。
どうにかして省略したい。

# 参考サイト
[Git 作業効率化 Tips】〜git addしてcommitしてpushするのが面倒な全ての人に捧ぐ〜](https://qiita.com/G-awa/items/147dfc88513710d5da3c)

シェルスクリプトを使って、自動化を行う試み。
実際のコードも紹介されているのですぐに試せそう。

[GitHubのコミットメッセージに絵文字を入れて開発効率をあげる](https://qiita.com/Jung0/items/0a9a7a97a2c17f92d3c5)

少し話がそれるかもしれないが、一緒に自動化するときに導入したいのがコミットメッセージに絵文字を入れること。
自動化までは行かずとも、開発効率をあげるという意味で今回一緒に覚えておけばよかろう。


# 実装
~/.zshrcに追記
※追記したらターミナルをsourceするか、別で新しく開く必要あり。

```bash
# add .からpushまでをマクロで定義
gish() {
    # 全てステージにのせる
    git add .
    # コミット対象のファイルを確認
    git status
    read "yesno?Commit with this content. OK? (y/N): "
    case "$yesno" in
        # yes
        [yY]*)
            read "msg?Input Commit Message: "
            git commit -m "$msg"
            CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD)
            git push origin "$CURRENT_BRANCH"
            ;;
        # no
        *)
            echo "Quit."
            ;;
    esac
}
```

# 所感
使う時はgishと打つだけです。
ちょー時短になりました。

