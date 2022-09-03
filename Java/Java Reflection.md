# Java Reflection
Reflectionは、実行時にクラスの振る舞いを動的に調査し、変更することができるAPIである。
Reflectionを使用すると、クラスのオブジェクトを動的に作成し、そのオブジェクトのメソッドを呼び出したり、オブジェクトのプライベートフィールドのセッターメソッドを使用せずに直接アクセスしたりすることができる。

JavaReflectionの主要なメソッドは、すべてjava.lang.Classのインスタンスとして表現される
* java.lang.reflect.Constructor
* java.lang.reflect.Method
* java.lang.reflect.Field
* java.lang.reflect.Annotation

→コンストラクタ、メソッド、フィールド、アノテーションなどにアクセスする全てのReflectionメソッドを持っている

```
package com.bharath.java.reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Arrays;

public class Test {

	public static void main(String[] args) {
		try {
			// クラス名を受け取り、それをメモリにロードする
			Class<?> myClass = Class.forName(Calculator.class.getName());
			// クラス名を取得する
			System.out.println(myClass.getName());

			// クラス内のコンストラクタの配列を返してくれる(※privateにはアクセスできない)
			Constructor<?>[] constructors = myClass.getConstructors();
			System.out.println(Arrays.toString(constructors));

			// クラス内のメソッドの配列を返してくれる
			Method[] methods = myClass.getMethods();
			System.out.println(Arrays.toString(methods));

			// 特定のコンストラクタにアクセスする（渡したパラメータに応じて適切なコンストラクタが選択される）
			Constructor<?> constructor = myClass.getConstructor(null);
			Constructor<?> constructor2 = myClass.getConstructor(double.class, double.class);
			// オブジェクト（コンストラクタ）のインスタンスを作成する
			System.out.println(constructor.newInstance(null));
			System.out.println(constructor2.newInstance(5, 10));

			Object myObj = constructor2.newInstance(5, 10);
			// 特定のメソッドにアクセスする（メソッド名とパラメータ型を指定する）
			Method getNum1 = myClass.getMethod("getNum1", null);
			Method getNum2 = myClass.getMethod("getNum2", null);
			// メソッドを呼び出す
			System.out.println(getNum1.invoke(myObj, null));
			System.out.println(getNum2.invoke(myObj, null));

			// 特定のメソッドを呼び出す。(setterのため、パラメータを指定する）
			Method setNum1 = myClass.getMethod("setNum1", double.class);
			Method setNum2 = myClass.getMethod("setNum2", double.class);
			setNum1.invoke(myObj, 6);
			setNum2.invoke(myObj, 8);
			// メソッドを呼び出す
			System.out.println(getNum1.invoke(myObj, null));
			System.out.println(getNum2.invoke(myObj, null));

		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (NoSuchMethodException e) {
			e.printStackTrace();
		} catch (SecurityException e) {
			e.printStackTrace();
		} catch (InstantiationException e) {
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			e.printStackTrace();
		} catch (IllegalArgumentException e) {
			e.printStackTrace();
		} catch (InvocationTargetException e) {
			e.printStackTrace();
		}
	}

}

```
