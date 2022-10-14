package
packageとは
クラスファイルを整理するためのフォルダのこと

javaには5000もの定義済みクラスがあり、
* java.lang
* java.util
* java.io
* javax.xml
などの独自パッケージにまとめられている。

javaとjavaxはすべての定義済みサブパッケージのスーパーパッケージ

```
package p1;

public class A {

	//静的メソッド
	public static void a1() {
		System.out.println("Inside a1");
	}
	//非静的メソッド
	public void a2() {
		System.out.println("Inside a2");
	}

}
```

```
package p1;

//Aクラスと同じp1パッケージに所属しているため、Aクラスのメソッドを使用可能
public class B {
	
	public static void main(String[] args) {
		//静的メンバにはクラス名で直接アクセスできる
		A.a1();
		//非静的メンバにはオブジェクトでクラスのインスタンスを作成してアクセスする。
		A aObject = new A();
		aObject.a2();
	}

}
```

packageが一緒であれば、別クラスからでも、同じpackageの別クラスにアクセスできる
```
package p2;

//異なるパッケージのクラスを使用するにはimportする必要がある
import p1.A;

public class C {
	
	public static void main(String[] args) {
		
		A.a1();
		
		A aObject = new A();
		aObject.a2();
		
	}

}
```
違うpackageからアクセスするには、importする必要がある。
