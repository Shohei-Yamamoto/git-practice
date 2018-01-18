# git-practice
git practice

## git status
とりあえず基本ステータスが観れる

## git merge
２つの親を持つコミット
２つの親のすべての変更を含むと考える

masterで作業している時

```
git merge bugfix
```
をした場合 masterが一つ進みbugfixも親に持つことになる
bugfixは遅れたままなので、masterと同じところをさすには
```
git checkout bugfix && git merge master
```
とすれば良い。


## git rebase
リベースはコミットを対象先を親にしたものにくっつけるイメージ
```
git checkout bugfix && git rebase master
```
とするとbugfixのmasterに存在していないコードの差分、つまりコミットは今のmasterの上にくっつく
masterのいちは変わっていない
```
git checkout master && git rebase bugfix
```
でmasterもbugfixに追いつく
```
git checkout master && git merge bugfix
```
でもいける


## HEAD
HEADは常に作業中のコミットを指している
普段masterで作業しているなら基本master自信を指している
commit自体(のハッシュ値)にチェックアウトすることで、HEADは直接そのコミットを指すことができる
ハッシュ値は一意に判断できるなら途中までの文字列でも良い("abcde"というハッシュ値なら"abc"までなど)
### 相対リファレンス
HEAD^はHEADの一つ前のコミットを表す。master^などもできる。
```
git checkout master^^
```
などのように使える

HEAD~3 で３つ前のコミットを指す

よく使われるのがbranchの強制的な移動
```
git branch -f master HEAD~3
```
上はmasterブランチをHEADの3つ前のコミットに移動させるコマンド


`git HEAD^2`はマージされた側の親に戻る。mergeの時は親が２つあるのでその時に使う


## git reset
ローカルで使う
```
git reset HEAD^
```
でmasterで作業中の場合コミットを一つ戻る

しかしどうしてもリモートのコミットを取り消したい場合は、
```
git reset HEAD~3 && git push -f
```
しかし超危険なので注意。基本的に職場ではつかわない方がいいでしょう


## git revert
commitをとりけすcommitを追加する
```
git revert HEAD
```
とすると今HEADが指しているcommitが無効化される
別にHEAD^としたからといって2つきえる訳ではなく、一つ前のコミットが消えるようなcommitが行われるだけ

## git log
git logでログが見れる。qで終了
git log --onelineで簡単なログ。ハッシュ値だけ見たかったりする時便利

## git cherry-pick
```
git cherry-pick コミット
```
で他のブランチなどにあるコミットをrebaseのようにくっつけられるコミットは連続して書ける

デバッグの時などにprintなどデバッグ用のコミットと、バグ修正用のcommitがある時に使える。

## git rebase -i
インタラクティブrebase
エディタが開いてcommitのログをまとめたりできる。
[参考](https://qiita.com/tsuuuuu_san/items/f708a9f7ea8ab8eb6945)

例えば `git rebase -i` で少し修正を加えたいコミットを後に持ってきて、
そこで`commit --amend`で少し修正を加えるといったことが可能

## git stash
`git stash`で、ステージされていないファイルたちを一時避難できる
`git stash pop`で元に戻る

## git tag
`git tag TAG_v1 C1`でC1のコミットにタグをつけられる

## git describe

## git bisect

#リモート
## git clone
リモートのブランチをローカルに紐づける

リモートのブランチは origin/developのように*リモート名/ブランチ名*になっている
リモートブランチにチェックアウトして、コミットをしてもHEADが動くだけでremote/developの位置は変わらない

## git fetch
ローカルのリモートブランチよりリモートリポジトリのブランチが進んでる時、ローカルのリモートリポジトリをリモートリポジトリと同じ状態にする。
* リモートにあってローカルにないコミットをダウンロード
* リモートブランチの位置を変更
上の２つの動作を行なっている

ローカルブランチが何か変更される訳ではないので注意！

## リモートの変更を自分の作業に取り込む方法
* git cherry-pick origin/master
* git rebase origin/master
* git merge origin/master
など
`git pull`は　
```
git fetch && git merge origin/master
```
の短縮系
## 参考
[learngitbranching](https://learngitbranching.js.org)

## トラブルシュート
### ユーザー違いで起きる奴
```
git push origin master
remote: Permission to Shohei-Yamamoto/git-practice.git denied to SignalYellow.
fatal: unable to access 'https://github.com/Shohei-Yamamoto/git-practice.git/': The requested URL returned error: 403
```
上のような奴が出たら
```
git remote set-url origin https://Shohei-Yamamoto@github.com/Shohei-Yamamoto/git-practice.git/
```
こちらのようにURLに自分のユーザー名を付与して再度pushで解決