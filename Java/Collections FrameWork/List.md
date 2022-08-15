# List
## ArrayListClass
オブジェクトを格納するために配列を使用する

ArrayList
* アクセスは非常に高速
* 要素を挿入する場合、終端を除く全ての要素を挿入するため、非常に遅くなる。（挿入を多用する場合にはあまり良いデータ構造とは言えない）
* 初期容量は１０（容量を指定しなければ）
* 容量を超えると最初の配列の1.5倍の大きさの配列が作成され、そこに要素やオブジェクトが丸ごとコピーされる。（容量がわかっている場合は指定しておくのがよい）

```
package list;
//ArrayListクラスで動的な配列を作成する
import java.util.ArrayList;

public class ArrayListDemo {

	public static void main(String[] args) {
		/*
		 * 作成したlistはデータ型を指定していないため、あらゆる種類のオブジェクトを追加したり、取り出したりできる。
		 * →ランタイムの問題を引き起こす可能性が大きい
		 */
		ArrayList list = new ArrayList();
		list.add(new Integer(20));
		list.add(10);
		list.add(30.45);
		list.add("Hello World!");
		
		/*
		 * ジェネリックという概念を使用して、データ型の制限を行う
		 */
		ArrayList<Integer> list2 = new ArrayList<Integer>();
		list2.add(new Integer(20));
		list2.add(10);
		list2.add(30);
		
	}

}
```

## LinkedListClass
LinkList
要素をノードという形で格納する。

node
* 各ノードには3つのフィールドがある
* 前のノードを指すフィールド、オブジェクトを格納するフィールド、次のノードを指すフィールド
* LinkListは全てのノードが前のノードと次のノードを知っているようにリンクされる
メリット
* 挿入と削除が非常に容易であること（2つのノードの間にレコードを挿入したい場合、それらのノードの次と前のポインタを入れ替えるだけでいい）
デメリット
* 各ノードには3つのフィールドがあるため、より多くのメモリが必要になる
* これらのノードのために、メモリがランダムに割り当てられるため、インデックスで要素にアクセスする場合に、配列よりも時間がかかる。

```
package list;

import java.util.LinkedList;
import java.util.List;

public class LindedLIstDemo {

	public static void main(String[] args) {

		Object objects[] = new Object[100000];
		for (int i = 0; i < objects.length; i++) {
			objects[i] = new Object();
		}
		
		//要素をノードという形で格納する
		List<Object> list = new LinkedList<>();
		long start = System.currentTimeMillis();
		for (Object object : objects) {
			list.add(object);
		}
		long end = System.currentTimeMillis();
		System.out.println(end - start);
	}

}
```


## LIstMethods
```
package list;

import java.util.ArrayList;
import java.util.List;

public class ListMethods {

	public static void main(String[] args) {

		List<Integer> list = new ArrayList<>();

		for (int i = 0; i < 100; i = i + 10) {
			list.add(i);
		}
		// 指定された位置に要素を追加する
		list.add(2, 100);
		// 指定された位置の要素を置き換える
		list.set(3, 300);
		System.out.println(list);

		// size(): listの要素数を返す
		for (int i = 0; i < list.size(); i++) {
			// get(): listの指定された位置にある要素を返す
			System.out.println(list.get(i));
		}

	}

}
```
