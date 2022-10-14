# String Pool
* StringクラスはImmutable(不変)である。

```
package immutable;

public class StringPoolDemo {

	public static void main(String[] args) {
		
		String s1 = "abc";
		String s2 = "xyz";
		String s3 = "abc";
		//new演算子を使用すると、JVMが文字列プールの新しいメモリ位置に設定する
		String s4 = new String("abc");
		
		//同じメモリ位置をさしているかを比較する
		System.out.println(s1 == s2);
		//→false
		System.out.println(s1 == s3);
		//→true
		System.out.println(s1 == s4);
		//→false
		
		//文字列を比較するだけで、メモリ位置は気にしない
		System.out.println(s1.equals(s2));
		//→false
		System.out.println(s1.equals(s3));
		//→true
		System.out.println(s1.equals(s4));
		//→true
	}

}

```
