# Java8
## ラムダ式
ラムダ式の目的は、Javaに関数型プログラミングの利点を導入すること
ラムダは、Javaにおける無名関数のこと
→名前、戻り値の型、アクセス修飾子を持たない関数

利点として
* コードの記述量を少なくできる
* 匿名インナークラスの実装が簡単である
* 他のメソッドにパラメータとして渡すことができる
ラムダを実装するためには、関数型インターフェイスを使用する必要がある

## 機能的インターフェイス
→抽象的なメソッドを一つだけ持つインターフェイス
*機能的メソッド*
→機能的インターフェイスの持つ抽象的メソッド

機能的インターフェイスの例
* Runnable（runメソッド）
* Comparator（compareToメソッド）
Java8ではインターフェイスにデフォルトメソッドを追加できるが、
機能的インターフェイスに定義する抽象的メソッドは１つだけであるべきである

FunctionalInterfaceというアノテーションでインターフェイスをマークすると、そのインターフェイスには１つの抽象メソッドしか定義できなくなる
→ラムダ式を実装するために必要

### @FunctionalInterface
```
// @FunctionalInterfaceでマークすることで抽象的なメソッドを１つ以上定義できなくする(必須ではない)
@FunctionalInterface
public interface A {

	void myMethod();
	
}

```
```

public class Test {

	public static void main(String[] args) {
		// ラムダ式で実装すると以下のような構文で処理を実装できる
		A a = () -> System.out.println("Inside MyMethod");
		a.myMethod();

	}

}

```

パラメータを持つラムダ式
```
package com.bharath.java8.lambdas.parameters;

@FunctionalInterface
public interface Sum {

	void add(int a, int b);
	
}

```
```
package com.bharath.java8.lambdas.parameters;

public class Test {

	public static void main(String[] args) {

		Sum s = (x, y) -> System.out.println("Sum is: " + (x + y));
		s.add(10, 20);
	}

}

```

関数型インターフェイス（以下はRunnableインターフェイス）
```
package com.bharath.java8.lambdas.interfaces;

public class Test {

	public static void main(String[] args) {
		// 以下は通常の方法で実装した場合
		/*
		 * // Runnable実装クラスを別で作成し、インスタンス化する 
		 * Runnable r = new MyRunnableImpl(); 
		 * // Runnable実装クラスのインスタンスを引数にThreadクラスをインスタンス化する 
		 * Thread t = new Thread(r);
		 * t.start();
		 * 
		 * for (int i = 1; i <= 10; i++) { System.out.println("Main Thread"); }
		 */
		
		// ラムダ式で実装した場合
		Runnable r = () -> {
			for (int i = 1; i <= 10; i++) {
				System.out.println("Child Thread");
			}
		};

		Thread t = new Thread(r);
		t.start();

		for (int i = 1; i <= 10; i++) {
			System.out.println("Main Thread");
		}
	}

}

```
