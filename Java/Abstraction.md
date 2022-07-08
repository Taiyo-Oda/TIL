# Abstraction
インターフェイスと抽象クラスの違い
interface
* interface内のメソッドは実装を持たない
* デフォルトですべてのメソッドはpublicでabstruct
* interfaceに含まれる変数はpublic staticとfinal(定数で変更できない)
* 変数が宣言された時点で初期化する必要がある
* 静的ブロックやインスタンスブロック、コンストラクタを定義できない
抽象クラス
* メソッドは実装をもつ（実装を持たないクラスはabstractキーワードをつける）
* public abstract以外の修飾語も使用できる
* 変数も、public staticとfainal以外も使用できる
* 変数は宣言された時点では初期化する必要はない
* 静的ブロックやインスタンス、コンストラクタを持つことができる

```
package abstractionIssue;

//インターフェイス
public interface TouchScreenLapTop {
	//インターフェイスは実装を持たない
	void scroll();
	void click();
	
}
```
```
package abstractionIssue;
//抽象クラス
public abstract class DELL implements TouchScreenLapTop {

	//抽象クラスでは実装を持つことができる
	@Override
	public void scroll() {
		System.out.println("Inside Dell Scroll");
	}
	
}
```
抽象クラスの拡張
```
package abstractionIssue;

//抽象クラスを継承
public class DELLNotebook extends DELL{
	
	//抽象クラスのインスタンスは作成できない。
	//継承したメソッドのオーバーライドは可能
	@Override
	public void click() {
		System.out.println("Inside DELLNotebook click");
	}

}
```
```
package abstractionIssue;

public class Test {
		
		/*
		 *抽象クラスのインスタンスは作成できないため、抽象クラスを継承したクラスのインスタンスを作成
		 */
		DELLNotebook den = new DELLNotebook();
		den.scroll();
		den.click();
	}
	
}
```
ああああ
