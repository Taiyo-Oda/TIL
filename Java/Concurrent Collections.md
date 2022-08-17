# Concurrent Collections
コレクションクラスをマルチスレッドで扱う場合、thread safe を実現するために*同期*を使用する必要がある
→そのスレッドが使用している間は、他のスレッドはそのコレクションにアクセスできないようにする。

上記したものには2つの問題がある
1. あるスレッドがコレクションをロックすると、コレクション全体がロックされ、他のスレッドはコレクションにアクセスできなくなる
2. Iteratorを使ってコレクションをループしてアクセスしているときに、そのコレクションを操作して、同時に要素を追加したり削除したりしようとすると、同時変更例外が発生する

→これらの問題を解決するために、Java1.5で導入されたコンカレント・コレクションを使用する

複数のスレッドが同じリストにアクセスできる。
複数のスレッドが同じリストにアクセスしようとするとき、リストのコピーが作成され、変更がそのコピー上で行われる
その変更は後で、元のコレクションと同期される
→スレッドのロック解除を待つ必要がない

concurrent collectionを使用することで
* ロックの心配をする必要がなくなる
* iteratorのループ処理で例外の心配をする必要がなくなる

## CopyOnWriteArrayList
```
package concurrentcollections;

import java.util.Iterator;
import java.util.concurrent.CopyOnWriteArrayList;

public class ArrayListProblem {

	public static void main(String[] args) {
		// コレクションをループしてアクセスしているときに、コレクションを操作すると例外が発生する
		// ArrayList<String> courses = new ArrayList<String>();
		
		// 反復中のリストに修正を加えようとするとリストのコピーを作成し、変更はそのコピーに対して行われる
		CopyOnWriteArrayList<String> courses = new CopyOnWriteArrayList<>();
		courses.add("Java");
		courses.add("Python");
		courses.add("AWS");
		courses.add("Docker");

		// Iterator : 集合の要素に順番にアクセスするインターフェイス
		Iterator<String> iterator = courses.iterator();

		// hasNex() : 次の要素がある場合true, ない場合false 
		while (iterator.hasNext()) {
			String course = iterator.next();
			System.out.println(course);
			if (course.equals("Docker")) {
				courses.remove(course);
			}
		}
		
		System.out.println(courses);
	}

}
```

マルチスレッドの場合も同様
```
package concurrentcollections;

import java.util.Iterator;
import java.util.concurrent.CopyOnWriteArrayList;

public class ArrayListProblem extends Thread {
	
	// 反復中のリストに修正を加えようとするとリストのコピーを作成し、変更はそのコピーに対して行われる
	static CopyOnWriteArrayList<String> courses = new CopyOnWriteArrayList<>();
			
	@Override
	public void run() {
		try {
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		courses.add("Kubernetes");
	}

	public static void main(String[] args) throws InterruptedException {
		
		ArrayListProblem arrayListProblem = new ArrayListProblem();
		arrayListProblem.start();
		
		courses.add("Java");
		courses.add("Python");
		courses.add("AWS");
		courses.add("Docker");

		// Iterator : 集合の要素に順番にアクセスするインターフェイス
		Iterator<String> iterator = courses.iterator();

		// hasNex() : 次の要素がある場合true, ない場合false 
		while (iterator.hasNext()) {
			Thread.sleep(2000);
			String course = iterator.next();
			System.out.println(course);
		}
		
		System.out.println(courses);
	}

}

```


## CopyOnWriteArraySet
```
package concurrentcollections;

import java.util.Iterator;
import java.util.concurrent.CopyOnWriteArraySet;

public class ArraySetProblem extends Thread {
	
	// 反復中の配列セットに修正を加えようとすると配列セットのコピーを作成し、変更はそのコピーに対して行われる
	static CopyOnWriteArraySet<String> courses = new CopyOnWriteArraySet<>();
			
	@Override
	public void run() {
		try {
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		courses.add("Kubernetes");
	}

	public static void main(String[] args) throws InterruptedException {
		
		ArraySetProblem arrayListProblem = new ArraySetProblem();
		arrayListProblem.start();
		
		courses.add("Java");
		courses.add("Python");
		courses.add("AWS");
		courses.add("Docker");

		// Iterator : 集合の要素に順番にアクセスするインターフェイス
		Iterator<String> iterator = courses.iterator();

		// hasNex() : 次の要素がある場合true, ない場合false 
		while (iterator.hasNext()) {
			Thread.sleep(2000);
			String course = iterator.next();
			System.out.println(course);
		}
		
		System.out.println(courses);
	}

}

```


## ConcurrentHashMap
```

package concurrentcollections;

import java.util.Iterator;
import java.util.concurrent.ConcurrentHashMap;

public class ConcurrentHashMapDemo extends Thread {
	
	// 反復中のHashMapに修正を加えようとするとHashMapのコピーを作成し、変更はそのコピーに対して行われる
	static ConcurrentHashMap<String, String> courseRatings= new ConcurrentHashMap<String, String>();
			
	@Override
	public void run() {
		try {
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		courseRatings.put("Kubernetes", "4,8");
	}

	public static void main(String[] args) throws InterruptedException {
		
		ConcurrentHashMapDemo arrayListProblem = new ConcurrentHashMapDemo();
		arrayListProblem.start();
		
		courseRatings.put("Java", "5.0");
		courseRatings.put("Python", "4,8");
		courseRatings.put("AWS", "4.7");
		courseRatings.put("Docker", "4.7");

		// Iterator : 集合の要素に順番にアクセスするインターフェイス
		Iterator<String> iterator = courseRatings.keySet().iterator();

		// hasNex() : 次の要素がある場合true, ない場合false 
		while (iterator.hasNext()) {
			Thread.sleep(2000);
			String course = iterator.next();
			System.out.println(course);
		}
		
		System.out.println(courseRatings);
	}

}

```
