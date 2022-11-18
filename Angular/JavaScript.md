## スクリプトの読み込み方針
javascriptを使用してページ上の要素（正確には[Document Object Model](https://developer.mozilla.org/ja/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents#the_document_object_model)）を操作している場合、HTMLの前にJSが読み込まれ、解析されてもコードは機能しないため、構文を使用して、エラーを回避する必要がある。

* 内部の例（HTMLファイル内に、JSを記述する場合）
```
document.addEventListener("DOMContentLoaded", function() {
  ...
});
```
これはブラウザーの “DOMContentLoaded” イベントをリッスンするイベントリスナーで、HTML body が完全に読み込まれて解析されたことを示します。このブロック内の JavaScript はそのイベントが発生するまで実行されないため、エラーは回避されます

* 外部の例（JSファイルを作成して、記述する場合）
```
<script src="script.js" defer></script>
```
この場合、スクリプトと HTML の両方が同時に読み込まれ、コードが機能します。（deferは外部スクリプトに対してのみ機能する）

bodyの下部にscript要素を置くことでも解決できるが、その場合、 HTML DOMが読み込まれるまで、スクリプトの読み込みと解析が完全にブロックされるため、JavaScriptがたくさんある大規模なサイトでは、パフォーマンス上の問題を引き起こす可能性があり、サイトが遅くなる。


参考：[JavaScript とは - ウェブ開発を学ぶ | MDN](https://developer.mozilla.org/ja/docs/Learn/JavaScript/First_steps/What_is_JavaScript#script_loading_strategies)


