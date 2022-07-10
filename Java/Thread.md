# Thread
## スレッドとは
処理を実行する流れの単位のこと

* プログラムを*実行する処理の最小単位*
* アプリは*メインスレッド*という単一スレッドで動く
* しかし*単一スレッド*では、ネットワーク通信時などで、アプリ利用者は次の操作を通信終了まで待たないといけなくなる
* スレッドが複数ある（=*マルチスレッド*）と、
ネットワーク通信はバックグラウンドで他のスレッドが実行し、*通信中でもメインスレッドで処理を並行して実行することができる*

```
//マルチスレッドの実装
//1.java.langパッケージのThreadクラスを拡張する
public class MultiThreaded extends Thread{
	
	//以下の処理では、mainとrunの処理が切り替わりながら実行される
	
	public static void main(String[] args) {
		
		MultiThreaded mt = new MultiThreaded();
		//別のスレッドを作成し、そのスレッドがMultiThreadedクラスのrunメソッドを呼び出す
		mt.start();
		
		for (int j = 0; j <= 200; j++) {
			System.out.print("j : " + j + "\t");
		}
	}
	
	public void run() {
		for (int i = 0; i <= 200; i++) {
			System.out.print("i : " + i + "\t");
		}
	}

}
```

threadクラスの実装ではrunメソッドをオーバーライドする
