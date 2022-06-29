# Polymorphism
ポリモーフィズムには２種類ある
* Static binding(コンパイル時ポリモーフィズム)
* Dynamic binding(ランタイムポリモーフィズム)

Static binding(コンパイル時ポリモーフィズム)
```

public class CompileTimeBinding {
	
	
	void add(int a, int b){
		int result = a + b;
		System.out.println("Result is " + result);
	}
	
	void add(int a, int b, int c){
		int result = a + b + c;
		System.out.println("Result is " + result);
	}
	
	void add(float a, float b){
		float result = a + b;
		System.out.println("Result is " + result);
	}
	
	public static void main(String[] args) {
		CompileTimeBinding c = new CompileTimeBinding();
		c.add(10, 20);
		c.add(30, 40, 50);
		c.add(60f, 70f);
		
		/*
		 * Result is 30
		 * Result is 120
		 * Result is 130.0
		 * 
		 * パラメータの種類に応じて、適切なメソッドを呼び出す。
		 * Static binding(コンパイル時ポリモーフィズム)
		 */
	}

}
```


Dynamic binding(ランタイムポリモーフィズム)
```
public class MacBook {
	
	void start(){
		System.out.println("Inside MacBook start()");
	}
	
	void shutDown(){
		System.out.println("Inside MacBook shutDown()");
	}

}

```
```

public class MacBookPro extends MacBook {
	
	@Override
	void start() {
		System.out.println("Inside MacBookPros start method");
	}
	
	@Override
	void shutDown() {
		System.out.println("Inside MacBookPros shutDown method");
	}

}

```
```

public class MacBookAir extends MacBook {
	
	@Override
	void start() {
		System.out.println("Inside MacBookAirs start method");
	}
	
	@Override
	void shutDown() {
		System.out.println("Inside MacBookAirs shutDown method");
	}

}

```
```

public class RunTimeBinding {

	public static void main(String[] args) {
		
		//子クラスのインスタンスを作成し、親クラスに代入する
		MacBook m1 = new MacBookPro();
		MacBook m2 = new MacBookAir();
		
		m1.start();
		m1.shutDown();
		
		m2.start();
		m2.shutDown();
		
	}

}

```
