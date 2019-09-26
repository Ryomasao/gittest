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


