# AccessModefers
変数の修飾子によってアクセスできるレベルが異なる。
```
package p1;

public class A {

	//クラスレベル
	private int a = 30;
	//パッケージレベル
			int b = 40;
	//パッケージ内と継承先クラス内
	protected int c = 50;
	//どこからでも
	public int d = 60;
	
	public static void main(String[] args) {
		//非静的メンバにアクセスするため、コンストラクタ(ここではデフォルトコンストラクタ)を呼び出すためのインスタンスを作る
		A aObject = new A();
		System.out.println(aObject.a);
		System.out.println(aObject.b);
		System.out.println(aObject.c);
		System.out.println(aObject.d);
	}

}
```
Aクラスで宣言している変数は、Aクラス内ならすべてにアクセスできる。


```
package p1;

public class B {

	public static void main(String[] args) {
		
		A aObject = new A();
		//修飾子なし（パッケージレベル）
		System.out.println(aObject.b);
		//protected
		System.out.println(aObject.c);
		//public
		System.out.println(aObject.d);
	}

}
```
Aクラスと同じパッケージに所属するBクラスからは、private修飾子がつけられている変数にはアクセスできず、それ以外にはAクラスのインスタンスを作成してアクセスすることが可能

```
package p2;

//p1パッケージのAクラスをインポート
import p1.A;

//importしたAclassを継承
public class C extends A{
	
	public static void main(String[] args) {
		A aObject = new A();
		//public
		System.out.println(aObject.d);
		
		C cObject = new C();
		//protected
		System.out.println(cObject.c);
		//public
		System.out.println(cObject.d);
	}

}
```
Aクラスと別パッケージのCクラスでは、変数a,bにはアクセスすることはできない。
protected修飾子の変数cは、Aクラスを継承することで、アクセスすることができ、publicな変数dには、継承の有無にか関わらず、アクセスすることができる。
