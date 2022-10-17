# stashで作業を一時避難する
## stashとは
作業が途中でコミットしたくないけど、別のブランチで作業しなければならない。そのような場合に現在の作業を一時避難するために使用する。<br>

### 作業を一時避難する
```
git stash
git stash save
```

ワークツリーとステージの変更分をstashに一時避難させ保存する<br>
stashした変更分は復元するまで無かったことになる

### 避難した作業を確認する
```
git stash list
```

### 避難した作業を復元する
```
// 最新の作業を復元する(ワークツリーのみ)
git stash apply
// ステージの状況も復元する
git stash apply --index

// 特定の作業を復元する
git stash apply <スタッシュ名>
git stash apply stash@{1}
// スタッシュ名はgit stash listで確認できる
// 新しいものほど数値が小さくなる
```

### 避難した作業を削除する
```
// 最新の作業を削除する
git stash drop

// 特定の作業を削除する
git stash drop <スタッシュ名>
git stash drop stash@{1}

//　全作業を削除する
git stash clear
```

