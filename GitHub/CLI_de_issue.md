# ターミナルでissueを立てたい
なぜならGUI操作がめんどくさく感じるようになってきたから。
なるべくターミナルで作業したい。


# GitHub CLI
RUNTEQカリキュラムのおかげですでにインストールはされていた。
あとはコマンド操作を覚えるだけ。

# 操作

### issueを立てる

```
gh issue create
```

### オープンしているissueを確認する

```
gh issue list
```

### issue1の詳細を見る

```
gh issue view 1
```

### issue1にコメントする

```
gh issue comment 1 --body "これはコメントです。"
```

```
gh issue comment 1
```
このあとnanoを開いてコメントをすることも可能。

### issue1を閉じる

```
gh issue close 1
```


# 参考サイト
[Git】GitHubCLIでターミナル上でIssueを投稿/確認する](https://www.mtioutput.com/entry/github-cli-issue)
