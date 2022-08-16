```
package queue;

import java.util.PriorityQueue;
import java.util.Queue;

public class PriorityQueueDemo {

	public static void main(String[] args) {
		// Queueは処理の前に要素を保持するために設計されたコレクション
		Queue<Integer> q = new PriorityQueue<>();
		// peek()はQueueの最初の要素を取得する。からの場合nullを返す
		System.out.println(q.peek());

		// offerは引数をQueueに追加する
		for (int i = 20; i <= 30; i++) {
			q.offer(i);
		}
		System.out.println(q);
		
		// queueの先頭を取り除き、空の場合nullを返す
		q.poll();
		// pollと同様だが、空の場合は例外(NoSuchElementException)
		q.remove();
		// 要素の除去などを行うと、最初に追加した順番は保証されない（順番を保持する場合はTreeSetを使う）
		System.out.println(q);
	}

}
```
