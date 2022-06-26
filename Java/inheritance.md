inheritance
```
public class Parent {
	
	Parent() {
		System.out.println("Parent Object"+this);
	}
	
}
```
```
//Parentクラスを継承する
public class Child extends Parent {
	
	Child() {
		System.out.println("Child Object"+this);
	}

}
```
```
public class MultiLevelTest {
	//多重継承
	public static void main(String[] args) {
		//Parentクラスを継承したChildクラスのインスタンスを作成
		Child c = new Child();
		
		//同じメモリローケーションを共有している
		//Parent ObjectChild@7344699f
		//Child ObjectChild@7344699f
	
	}
}
```

MultiLevelTestクラスで参照しているコンストラクタ（オブジェクト）は同じメモリ領域を共有している
