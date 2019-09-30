# gittest
Gitのテスト



# fork運用
ここでは、forkしたリポジトリをorigin、fork元のリポジトリをupsreamとする。


## forkしたリポジトリをcloneする

```
git clone https://github.com/coryomasao/gittest
```

リモートリポジトリの場所を知りたい！
```
git remote -v
origin  https://github.com/coryomasao/gittest.git (fetch)
origin  https://github.com/coryomasao/gittest.git (push)
```

fork元も取り込めるように、upstreamとてリモートリポジトリを追加
```
git remote add upstream https://github.com/Ryomasao/gittest.git
# URLを変更したい場合
git remote set-url upstream https://github.com/Ryomasao/gittest.git
```

追加された。

```
origin  https://github.com/coryomasao/gittest.git (fetch)
origin  https://github.com/coryomasao/gittest.git (push)
upstream        https://github.com/Ryomasao/gittest.git (fetch)
upstream        https://github.com/Ryomasao/gittest.git (push)
```

upsteamのブランチもpushとして存在してるので、直接pushすることもできちゃうみたい。
怖いので、pushのURLは書き換えとく。

https://xuwei-k.hatenablog.com/entry/20130611/1370846923

```
git remote set-url upstream --push no-push
```

## story1
fork元で変更を行う
さらにcommitをひとつ進める
```
git push origin master
```

この状態で、fork先でさきほどの変更を手元に取り込む

```
git fetch upstream/master
```

`upstream/master`は、リモートリポジトリ`upstream`を追跡するリモート追跡ブランチ。
`git fetch`することで、このリモート追跡ブランチに変更を取り込んでいる。
リモート追跡<strong>ブランチ</strong>なので、`git log upstream/master`だって当然できる。



## git rebase -i
これを複数回にわける

## git rebase -i
## git rebase -i
これを複数回にわける
これを複数回にわける








