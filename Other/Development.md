# 開発の基礎的な知識について蓄積していく

## 「PATHを通す」について
### 概要
実行ファイルを（ファイルが置いてある場所を指定しないで）ファイル名を指定するだけで実行できるようにする。[参考](https://wa3.i-3-i.info/word18471.html)

### 手順(FlutterSDK)

1. ダウンロードしたファイルを解凍して任意のフォルダに入れる  
今回は、  
/Users/ユーザフォルダ/development  
 

1. 使用しているシェルを調べる  
`$ echo $SHELL`

1. シェルを開く  
    1. zshの場合  
    `$ vim .zshrc`
    1. bashの場合  
    `$ vim .bash_profile`
  
1. 編集する(pathを追加)  
実行直後にiキーを押して挿入モードに移行後、以下の行を追加  
追加後にEscキーを押してノーマルモードに戻った後、:wqで保存してvimを終了させる  
`export PATH="$HOME/development/flutter/bin:$PATH"`

1. 完了後、再読み込みする  
    1. zshの場合   
    `$ source .zshrc`
    1. bashの場合  
    `$ source ~/.bash_profile`

1. 完了したら正常にパスが通っていることを確認する  
`$ flutter --version`

[vimで使用するコマンドについて](https://fuchiaz.com/vim/#vimLinux)




## githubの使い方
### リポジトリの作成からpushまで
1. github上で任意のリポジトリを作成
2. ローカル環境にローカルリポジトリを作成  
[gitコマンド早見表](https://qiita.com/kohga/items/dccf135b0af395f69144)
