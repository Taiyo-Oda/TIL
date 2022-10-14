# WrapperClasses
オブジェクト型(ラッパークラス)とは
すべてのクラスが共通して継承している、ルートクラスのこと。

```
public class AllInOne {

	public static void main(String[] args) {
		int x = 100;
		//文字列型のオブジェクトに変換
		String y = Integer.toString(x);
		//constructorで整数型に変換(非推奨)
		Integer b = new Integer(y);
		
		//プリミティブな整数型に変換
		int z = b.intValue();
		//constructorで整数型のオブジェクトに変換（非推奨）
		Integer c = new Integer(z);
		//文字列型に変換
		String d = c.toString();
		//文字列をプリミティブな整数型に変換
		int e = Integer.parseInt(d);
	}

}
```
