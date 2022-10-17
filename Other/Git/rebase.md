## リベース
変更を統合するときに履歴をきれいに整えるために使用する

```
git rebase <ブランチ名>
```

このコマンドはブランチの起点となるコミットを別のコミットに移動する<br>
<ブランチ名>で指定したブランチを親ブランチとしてコミットを作成し、作業中のブランチのポインタをそこに移動させる。つまり、枝分かれしていたコミットが一直線になる<br>
→親ブランチの変更分を作業中のブランチに取り込んでいる

### rebaseの流れ
```
// mainブランチの内容を取り込んだ新しいコミットを作成し、featureブランチのポインタをそこに移動する
git checkout feature
git rebase main

// mainブランチのポインタをfastfowardで、作成されたコミットに移動させる（featureブランチをmainブランチにマージする）
git checkout main
git merge feature
```

### リモートにプッシュしたコミットをrebaseするのはNG!!
git push しているコミットをrebaseで変更すると、ローカルとリモートで履歴の内容が相違するため、git pushできなくなってしまう。

### merge と rebase の違い
merge
コンフリクトの解決が比較的簡単
マージコミットがたくさんあると履歴が複雑化する
→作業の履歴を残したい場合はmergeを使う

rebase
履歴をきれいに保つことができる
コンフリクトの解決が若干面倒
→履歴を残す必要がなく、履歴をきれいにしたい場合はrebase を使用する

### pullのリベース型
```
git pull --rebase <リモート名> <ブランチ名>
```

マージコミットを残さずにgithubの内容をpullすることができる

### リベースで履歴を書き換える
コミットをきれいに整えてからpushしたい場合はrebaseで履歴を書き換える(※GitHubにpushしていないコミットのみ)
```
git rebase -i <コミットID>
git rebase -i  HEAD~3

// やり直したいcommitをeditにする
// コミットの並べ替えや削除もここでできる
edit gh21f6d ヘッダー修正
pick 193054e ファイル追加
pick 84gha0d README修正

// コミットをまとめたい場合はsquashにする
pick gh21f6d ヘッダー修正
squash 193054e ファイル追加
squash 84gha0d README修正

// コミットを分割する場合は以下のようにする
pick gh21f6d ヘッダー修正
pick 193054e ファイル追加
edit 84gha0d READMEとindex修正
// HEAD^はHEADの1つ前のコミットを表す
// ここでは「193054e ファイル追加」
git reset HEAD^
git add README
git commit -m 'README修正'
git add index.html
git commit -m 'index.html修正'
git rebase --continue

// やり直したら実行する
git commit --amend

// 次のコミットへ進む（リベース完了）
git rebase --continue
```

1. git rebase -i コマンドで対話的リベースモードに入る
2. 修正したいコミットをeditにしてコミットエディタを終了する
3. pickだとそのままコミット内容を適用して次へ行き、editのコミットのところでコミットの適用が終わる
4. git commit --amendコマンドで修正
5. git rebase --comtinue コマンドで次のコミットへ行く
