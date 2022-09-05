# Regular Expressions(正規表現)

正規表現を使うと、拒否パターンを作成し、それを使用して文字列のグループをマッチングさせることができる

```
package com.bharath.java.re;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class PatternAndMatcher {

	public static void main(String[] args) {
		// 文字列の正規表現を受け取り、patternオブジェクトを返す
		Pattern pattern = Pattern.compile("abc");
		// 作成したパターンを作成して、マッチパターンを得る
		Matcher matcher = pattern.matcher("abcdefghijabcdefgabcdefg");
		while (matcher.find()) {
			// マッチング開始インデックスを返してくれる
			System.out.println("start: " + matcher.start());
			// マッチング終了インデックス（abcの次のインデックス）を返してくれる
			System.out.println("end: " + matcher.end());
			// マッチング中に見つかった文字のグループを返す
			System.out.println("group: " + matcher.group());
		}
	}

}

```

以下はメールアドレスを正規表現を使用してチェックしている
```
package com.bharath.java.re;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class EmailValidator {

	public static void main(String[] args) {
		// Eメールアドレスのチェックを行う
		Pattern pattern = Pattern.compile("^[a-zA-Z0-9_.-]+@[a-zA-Z0-9_.-]+$");
		Matcher matcher = pattern.matcher("noda.0630@gmail.com");
		if (matcher.matches()) {
			System.out.println("valid email id");
		} else {
			System.out.println("invalid email");
		}
	}

}

```
