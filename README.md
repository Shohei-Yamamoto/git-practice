# git-practice
git practice

##

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


## 参考
[learngitbranching](https://learngitbranching.js.org)
