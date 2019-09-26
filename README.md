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




