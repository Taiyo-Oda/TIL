# Javaアノテーション
`@ アノテーション名`<br>
で表現する

これは、クラス、フィールド、メソッドに使用することができる。
→アノテーションを使用するとこれらに特別な意味が付与される

例）
Java Persistence APIやHibernateのような高度なフレームワークを使用する場合、アノテーションでクラスやメソッドをマークすると、フレームワークがクラスをスキャンして、SQLを1行も作成せずにクラスのオブジェクトをデータベースの行に自動的に変換できる
→多くのXMLの設定を取り除くことができる。

アノテーションには３種類あり
* コンパイル時
* ビルド時
* ランタイム時
に使用することができる

*コンパイル時*
JavaのコンパイラーはJavaクラスをスキャンして、アノテーションを利用する。アノテーションの一部はコンパイル時に使用される

*ビルド時*
Gradle、Mavenなどのビルドツールがクラスをスキャンしてアーティファクトを生成できるように、コンパイル時だけでなくビルド時にもアノテーションを保持することもできる

*ランタイム時*
コンパイルされたコードでもクラスのアノテーションを保持し、実行時にも利用できるようにすることができる。


## @Deprecated
@Deprecatedアノテーションは使用することが非推奨のクラスやメソッドに指定する
→あるA PIやクラス、インターフェイスをコーダーや開発者に使ってもらいたくない場合に使用する。
以下の場合は、AクラスのmyMethodのみ呼び出し先で非推奨の警告が出る

```

public class A {

	@Deprecated
	public void myMethod() {

	}

	public void myMethod2() {

	}

}

```

```

public class B {
	
	public static void main(String[] args) {
		A a = new A();
		
		System.out.println(a);
		// ここのみ警告が出る
		a.myMethod();
		a.myMethod2();
	}
	
}
```



## @Override
本当に親クラスのメソッドをオーバーライドしているかどうかを確認するためのもの。
→メソッドを正確にオーバーライドしているかどうかのチェックをコンパイラに依頼する
以下のコードはOverrideアノテーションを使用しないとエラーは発生しない
```

public class MyParent {

	public String greet(String name) {
		return "Hello" + name;
	}
	
}

```
```

public class MyChild extends MyParent {

	@Override
	public String greet(Integer name) {
		return "Hi" + name;
	}

}

```

