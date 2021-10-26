# 「PATHを通す」について
## 概要
実行ファイルを（ファイルが置いてある場所を指定しないで）ファイル名を指定するだけで実行できるようにする。[参考](https://wa3.i-3-i.info/word18471.html)

## 手順(FlutterSDK)

ダウンロードしたファイルを解凍して任意のフォルダに入れる    
/Users/ユーザフォルダ/development  
に保存した場合  
格納後、パスを通すために.bash_profileを編集してパスを追加する

vimで.bash_profileを編集する  
`vim ~/.bash_profile`

実行直後にiキーを押して挿入モードに移行後、以下の行を追加  
追加後にEscキーを押してノーマルモードに戻った後、:wqで保存してvimを終了させる  
`export PATH="$HOME/development/flutter/bin:$PATH"`

完了後、.bash_profileを再読み込みする  
`source ~/.bash_profile`

完了したら正常にパスが通っていることを確認する  
`flutter --version`

[vimで使用するコマンドについて](https://fuchiaz.com/vim/#vimLinux)
