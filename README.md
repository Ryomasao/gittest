# gittest
Gitのテスト

# 基礎知識
https://qiita.com/_ha1f/items/2dca1047c57d4f0bd465
https://made.livesense.co.jp/entry/2017/08/22/080000

## Objectの中身をみてみる


オブジェクトは以下のディレクトリ格納されている。
`00`とかは、ハッシュの先頭2桁。
```
$ ls .git/objects
00	1c	2e	4e	6d
```
オブジェクトには、`commit`、`tree`、`blob`の3種類が存在している。

`cat-file`を使うと、ハッシュ値からオブジェクトについて知ることができる。
以下は、`git log`でとあるハッシュ値の値を見た場合

```
# -tオプジョンでオブジェクトの種類を確認できる
# コミットのハッシュ値をみたので、種類はcommit
$ git cat-file -t b56b489
commit
```

実際にオブジェクトの中身をみてみると、以下の通り

```
# -p オプションでオブジェクトの中身をみることができる
git cat-file -p b56b489
# treeオブジェクトについては後述
tree 944a87d53954a9f07ad3653e5469c84115b099b8
# parentは、文字通りこのコミットの親(派生元・一つ前のコミット)のオブジェクトのハッシュ値を指す
parent c64bf347a302b96acd964c9e47f42283840eea4f
author tohu_masao <konoemario@gmail.com> 1573607072 +0900
committer tohu_masao <konoemario@gmail.com> 1573607085 +0900
resetをメモを追加
```

上記で表示されている、`tree`オブジェクトの中身をみてみる

```
$ git cat-file -p 944a87
# treeには、git配下のディレクトリ、ファイルの情報が全部乗ってる！
# それがディレクトリであれば、treeだし、ファイルあればblob
100644 blob 9f14aa72c6c7c92b395438a1fe742f5eee68bce1	README.md
100644 blob 0d2e1737a82d88ef4d96a9f44c8891591cf3fa92	app.js
040000 tree 995cfcba331e429287f0411784599efef17e76ba	lib
100644 blob d58edc1590708b04695d9a1f28f3c0e6fd01c6ec	testDriver.js
```

blobをみると、実ファイルであることが確認できる。

以下の通り、この`README.md`の内容が表示されている。
```
$ git cat-file -p  9f14
# gittest
Gitのテスト
```

ちなみに`git add`することで、ファイルが圧縮され`blob`オブジェクトになるとのこと。
※ このblobオブジェクトは、差分ではなく、その時点のファイルの内容すべてが含まれているっぽい。


`README.md`をコミットしたあとに、再度、最新コミットのオブジェクト→treeオブジェクトをみてみると
`README.md`のハッシュ値だけがかわっていて、他はそのままであることが確認できる。
コミットごとに全ファイルのblobをつくってるのではないということだね。
```
100644 blob 73fae82ea7c602f6297e0433a11feb25d27b9544    README.md
100644 blob 0d2e1737a82d88ef4d96a9f44c8891591cf3fa92    app.js
040000 tree 995cfcba331e429287f0411784599efef17e76ba    lib
100644 blob d58edc1590708b04695d9a1f28f3c0e6fd01c6ec    testDriver.js
```


# command

## checkout

書いてた記憶があったんだけど、どっかいってしまった。
```
$ git checkout .
```

## reset

`git reset`は基本的に、HEADの状態を特定のコミットの状態に移動させると思ってよさそう。
その際、`--soft`をつけると、最新のコミットをindexに残す。ワーキングツリーはそのまま。
`--mixed`をつけると、indexはHEADに合わせ、ワーキングツリーはそのまま。
`--hard`をつけると、index、ワーキングツリーがHEADに合わせられる。

defaultは`mixed`。

使用例は以下の通り

* indexに追加したものをunstagedにしたいときは、`mixed`
* 全部、とある時点の状態に戻すときは`hard`
* `soft`はとある時点にHEADを戻して、そっから再コミットしたりするときに使う。つまりコミットメッセージを変えたり、コミットをまとめたり。
※とはいえ、コミットメッセージを変更するだけなら`--amend`のほうが素直。コミットをまとめるのも`rebase`でできたりする。



# fork運用についてのメモ
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
