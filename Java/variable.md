# アウトプット
静的メンバ
* クラスに属する（クラスレベル）
* クラス名で直接アクセスできる
* 静的ブロックはクラスがメモリに読み込まれた時に実行される。
* 静的変数はクラスの読み込み時にメモリを取得し、初期化が行われる。
非静的メンバ
* オブジェクトに属する（オブジェクトレベル）
* オブジェクトでクラスのインスタンスを作成し、そのオブジェクトの名前を使用する
* 非静的ブロックは、オブジェクトが生成されると同時に、コンストラクタが呼び出される前に実行される。
* 非静的変数は、オブジェクト生成時に初期化が行われる。

```
public class BackAccount {
	
	//銀行名はこのクラスのオブジェクトで共通のため、静的なフィールド名とする
	static String bankName;

	public static void main(String[] args) {
		//className.fieldNameとすることで、静的フィールドであるとわかりやすい
		System.out.println(BackAccount.bankName);
		//nullが出力される
	}

}
```

静的フィールドはクラス名で直接アクセスできる。
JVMはclassがメモリにロードされると、すぐにフィールドを初期化し、フィールド（メンバ変数）にアクセスできるようになる。
フィールドのタイプに応じて初期化されるため、文字列の場合はnullとなる。

```
public class BankAccount {
	
	//銀行名はこのクラスのオブジェクトで共通のため、静的なフィールド(メンバ変数)とする
	static String bankName ="Bank Of America";
	//以下の３つはこのクラスの各オブジェクトに対して一意であるため、非静的なフィールド(メンバ変数)としている
	String accountHoldersName;
	String accountNumber;
	float balance;

	public static void main(String[] args) {
		//className.fieldNameとすることで、静的フィールドであるとわかりやすい
		System.out.println(BankAccount.bankName);
		//オブジェクトのインスタンスを作成（フィールドにアクセスできるようにする）
		BankAccount bankAccount = new BankAccount();
		bankAccount.accountHoldersName = "John";
		bankAccount.accountNumber = "123456789";
		bankAccount.balance = 20000f;
		System.out.println(bankAccount.accountHoldersName);
		System.out.println(bankAccount.accountNumber);
		System.out.println(bankAccount.balance);
	}

}
```
非静的フィールド（メンバ変数）にアクセスする際は、オブジェクトのインスタンスを作成する必要がある。
mainメソッド内で、フィールドにアクセスし、データを代入している。

```
public class BankAccount {
	
	//銀行名はこのクラスのオブジェクトで共通のため、静的なフィールド(メンバ変数)とする
	static String bankName ="Bank Of America";
	//以下の３つはこのクラスの各オブジェクトに対して一意であるため、非静的なフィールド(メンバ変数)としている
	String accountHoldersName;
	String accountNumber;
	float balance;

	public static void main(String[] args) {
		//ローカル変数。デフォルトで初期化する必要がある。
		double loanInterestRate = 7.5;
		
		//className.fieldNameとすることで、静的フィールドであるとわかりやすい
		System.out.println(BankAccount.bankName);
		//オブジェクトのインスタンスを作成（参照しているメンバーにアクセスできるようにする）
		BankAccount bankAccount = new BankAccount();
		bankAccount.accountHoldersName = "John";
		bankAccount.accountNumber = "123456789";
		bankAccount.balance = 20000f;
		System.out.println(bankAccount.accountHoldersName);
		System.out.println(bankAccount.accountNumber);
		System.out.println(bankAccount.balance);
	}

}
```

ローカル変数は特定のメソッドの内部で定義する。
定義したローカル変数はスタック領域に保存され、定義しているメソッドやブロックが終了すると、その変数は消える。
JVMはローカル変数を初期化しないため、デフォルトで初期化しておく必要がある。
