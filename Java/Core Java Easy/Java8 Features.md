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

匿名インナークラス
```
package com.bharath.lambdas.anonymous;

public class Test {

	public static void main(String[] args) {
		/*
		 * // 以下は匿名インナークラスをラムダ式を使わずに実装した場合 
		 * Thread t = new Thread(new Runnable() {
		 * 
		 * @Override 
		 * public void run() { 
		 * 	for(int i=1; i<=10; i++) {
		 * 		System.out.println("Child Thread"); 
		 * 	} 
		 * }); t.start();
		 * 
		 * for (int i = 1; i <= 10; i++) { System.out.println("Main Thread"); }
		 */

		// 以下はラムダ式を用いて匿名インナークラスを実装した場合
		Thread t = new Thread(() -> {
			for (int i = 1; i <= 10; i++) {
				System.out.println("Child Thread");
			}
		});
		t.start();
		
		for(int i=1; i<=10; i++) {
			System.out.println("Main Thread");
		}
	}

}

```


## Predicateインターフェイス
*Predicate*
１つの引数を持つ関数でboolean値を返す
どんな型の引数でも取ることができるが、常にboolean値を返さなければならない、一つの抽象的なメソッドを持つ関数的なインターフェイスである。

複数のPrediceteを連結して使うことができる
* and()
* or()
* negate()
などのPredicateインターフェイスで利用可能なメソッドを使う

```
package com.bharath.java8.predicate.joins;

import java.util.function.Predicate;

public class PredicateJoins {

	public static void main(String[] args) {

		int[] x = { 0, 7, 10, 20, 30, 40, 50, 60, 70, 73 };

		// 与えられた数字が10より大きい場合はtrue
		Predicate<Integer> p1 = i -> i > 10;
		// 与えられた数字が、偶数がどうかチェックする
		Predicate<Integer> p2 = i -> i % 2 == 0;

		System.out.println("Greater than 10:");
		method1(p1, x);
		System.out.println("Even Numbers:");
		method1(p2, x);

		// negate() : 否定演算子（今回の場合は10より小さい場合trueとなる）
		System.out.println("Not greater 10:");
		method1(p1.negate(), x);
		// and() : AND演算子(p1かつp2の場合にtrueを返す)
		System.out.println("Greater than 10 AND Even");
		method1(p1.and(p2), x);
		// or() : OR演算子(p1かp2のどちらかに一致する場合はtrueを返す)
		System.out.println("Greater than 10 OR Even");
		method1(p1.or(p2), x);

	}

	// 関数にPredicateを渡す
	static void method1(Predicate<Integer> p, int[] x) {

		for (int eachValue : x) {
			if (p.test(eachValue)) {
				System.out.println(eachValue);
			}
		}

	}

}

```


## ダブルコロン演算子
Java8では、ダブルコロン演算子も導入され、それを使ってメソッドやコンストラクタを関数型インターフェイスのメソッドにマップすることができる

唯一のルールは、メソッドの引数が同じでなければならない。
```
package com.bharath.java8.methodref;

public class MethodRefDemo {

	// Runnableのrunメソッドには引数がないため、引数を指定しない
	public static void myMethod() {
		for (int i = 0; i <= 10; i++) {
			System.out.println("Child Thread");
		}
	}

	public static void main(String[] args) {
		/*
		 * RunnableのrunメソッドにmyMethodをマッピングする
		 * →ダブルコロン演算子を使って、自作のメソッドを機能インターフェイスのメソッドにマッピングする
		 */
		Runnable r = MethodRefDemo::myMethod;

		Thread t = new Thread(r);
		t.start();

		for (int i = 0; i <= 10; i++) {
			System.out.println("Parent Thread");
		}
	}

}

```

*インスタンスメソッドの参照*
```
package com.bharath.java8.methodref;

public interface MyInterface {

	public void myMethod(int i);

}
```
```
package com.bharath.java8.methodref;

public class MyClass {

	// インターフェイスの引数と同じ引数を持つ自作メソッドを作成する
	public void myMethod123(int i) {
		System.out.println(i);
	}

	public static void main(String[] args) {
		// myMethodに指定した引数を返すラムダ式を表現している
		MyInterface f = i -> System.out.println(i);
		f.myMethod(10);

		// このクラスのインスタンスを作成し、ダブルコロンを使用して、自作のメソッドをマッピングする
		MyClass c = new MyClass();
		MyInterface f1 = c::myMethod123;
		f1.myMethod(20);
	}

}

```

*コンストラクタの参照*
```
package com.bharath.java8.methodref.constructors;

public interface MyInterface {

	MyClass get(String s);
}

```
```
package com.bharath.java8.methodref.constructors;

public class MyClass {

	private String s;

	MyClass(String s) {
		this.s = s;
		System.out.println("Inside the constructor: " + s);
	}
}

```
```
package com.bharath.java8.methodref.constructors;

public class Test {

	public static void main(String[] args) {
		// MyClassのインスタンスを返すラムダ式を表現している
		MyInterface f1 = s -> new MyClass(s);
		f1.get("Using Lambdas");

		// コンストラクタのマッピングを行う場合は、クラス名::new演算子を使用する
		MyInterface f2 = MyClass::new;
		f2.get("Using Constructor Mapping");
	}
}

```


## Streams
データベースに対して使われるSQLに非常に似た宣言的な方法でデータを処理できるようになる
→コレクションないのデータやオブジェクトを非常に簡単に処理できるようになる
※ファイルの読み書きのためのjava.io.streamとは異なる

streamメソッドを呼び出すことで、コレクション上のストリームを取得することができる。
コレクションを処理するストリームを取得したら、2つのフェーズでそれを実行する

*1. Streamの設定（Configuration）*
Configurationには二つの方法がある
*filter*
filterメソッドを呼び出し、引数にPredicateを渡す
public Stream filter(Predicate<T> p)
コレクション内のオブジェクトはPredicateに基づいてフィルタリングされ、フィルタリングされたオブジェクトを含むstreamが返される
→単にフィルタをかけたい場合は、filterメソッドを使用する

*map*
mapメソッドを呼び出し、引数にFunctionを渡す
public Stream map(Function f)
Functionは新しいコレクションを作成し、コレクション内の各オブジェクトに対して新しいオブジェクトを作成する
→入力に基づいて新しいオブジェクトのセットやコレクションのセットを作成したい場合は、mapを使用することになる。map は入力を対応する出力オブジェクトにマッピングする。

*2. データそのものを処理する（Processing）*
Stream API
* collect()
* count()
* sorted()
* min()
* max()
などのメソッドを使用してデータの処理を行う

*filter*
```
package com.bharath.java8.stream;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class FilterEvenNumbers {

	public static void main(String[] args) {
		/*
		 * 以下はコレクションデータの処理方法 偶数のみを出力できるようにフィルタリングを行う
		 */
		List<Integer> l1 = new ArrayList<>();
		for (int i = 0; i <= 10; i++) {
			l1.add(i);
		}
		System.out.println(l1);

		// 通常の方法
		List<Integer> l2 = new ArrayList<>();
		for (Integer i : l1) {
			if (i % 2 == 0) {
				l2.add(i);
			}
		}
		System.out.println(l2);

		// streamを使用
		List<Integer> collect = l1.stream().filter(i -> i % 2 == 0).collect(Collectors.toList());
		/*
		 * li.stream().filter(i -> % 2 == 0)
		 * →streamの設定（filterメソッドで偶数番号のみをフィルタリングする）
		 * .collect(Collectors.toList())
		 * →処理ステップ(CollectoreクラスのtoList()メソッドを使用してフィルタからの出力をリストに集めている)
		 */

	}

}

```

*map*
```
package com.bharath.java8.stream;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class UpperToLowerCase {

	public static void main(String[] args) {

		List<String> l1 = new ArrayList<>();
		l1.add("JOHN");
		l1.add("BHARATH");
		l1.add("JIM");

		System.out.println(l1);

		List<String> l2 = l1.stream().map(s -> s.toLowerCase()).collect(Collectors.toList());
		/*
		 * l1.stream().map(s -> s.toLowerCase())
		 * →streamの設定（mapメソッドを使用して、コレクション内の各要素を与えられた関数に変換する）
		 * .collect(Collectors.toList())
		 * →処理ステップ(変換後の全ての要素を受け取り、コレクションの中に入れる)
		 */
		System.out.println(l2);
	}

}

```
