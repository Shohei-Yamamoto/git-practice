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


## 参考
[learngitbranching](https://learngitbranching.js.org)
