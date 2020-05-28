# git-challenge-minesweeper
Git Challenge Quiz

あってるかわかんない。`git log`でみた限り大丈夫な気がした。

## 解いた方法
`"山口"さんからの要望対応` の部分のコミットのデータが壊れていてチェッカーが通らないようになっていた。
通らない原因としてはチュートリアルにあるように、チップの合計値に整合性が取れていなかったからである。

```
$ git log
```

```
...中略

commit c77445a6614045753c76e4b32456b920affe0652
Author: Kuniwak <orga.chem.job@gmail.com>
Date:   Mon Oct 5 19:48:46 2015 +0900

    給料上げたらミス減りましたね 😮

commit cccd3171acf2b3eef3c7428ebed5b60025d16061
Author: Kuniwak <orga.chem.job@gmail.com>
Date:   Mon Oct 5 19:48:11 2015 +0900

    "山口"さんからの要望対応

commit 391eb34faa0791e1fb3827770c7c554ff40b32d0
Author: Kuniwak <orga.chem.job@gmail.com>
Date:   Mon Oct 5 19:47:27 2015 +0900

    "山本"さんからの要望対応
```

と出てくる。<br>


違いを確認するため
```
$ git diff cccd3171acf2b3eef3c7428ebed5b60025d16061 391eb34faa0791e1fb3827770c7c554ff40b32d0
```
で、csvが壊れる前とのコミットの違いを、

```
$ git diff c77445a6614045753c76e4b32456b920affe0652 cccd3171acf2b3eef3c7428ebed5b60025d16061
```
で、壊れた次の修正を見た。

<br>
最初に打ったコマンドについては

```
-86,cae4371d-3513-4790-ae07-3f896422c954,田中 陽翔,Guido90@gmail.com,4tOP1c2rC3ZkaQ6,0427-93-7324
+85,cae4371d-3513-4790-ae07-3f896422c954,田中 陽翔,Guido90@gmail.com,4tOP1c2rC3ZkaQ6,0427-93-7324
```
と出てきて、こちらでcsvのミスした部分の修正を行ったことがわかる。

そのため、
```
$ git rebase -i 391eb34faa0791e1fb3827770c7c554ff40b32d0
```
を行い、該当する`cccd3171acf2b3eef3c7428ebed5b60025d16061`のコミットの部分を`pick`から`edit`に変更し、

viで該当の行を85->86に修正。

```
$ git add .
$ git commit --amend
```
で該当のコミットを修正


```
$ git rebase --continue
```
で次の変更に移動し、

```
git commit  --allow-empty
```
このコマンドで空コミットを作成した(初めて使った)

```
$ git rebase --continue
```
でmasterブランチに戻ったので、強制プッシュ😇して終えた。
