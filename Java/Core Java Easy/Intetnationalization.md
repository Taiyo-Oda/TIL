# Internationalization (I18N)
国際化とはI18Nとも呼ばれる
複数の国や地域からアクセスされるサービスでは、言語、日付、数字の形式などの違いを考慮する必要がある

Javaでは以下のクラスを使用して、国際化を行う
* Locale
* NumberFormat
* DateFormat

## Locale
localeは地理的な位置を示すものである
通常は、国と言語の組み合わせである
例：US_EN → アメリカ 英語

Localeの情報はjava.util.Localeクラスで表現される

```
package classloading;

import java.util.Locale;

public class LocaleTest {

	public static void main(String[] args) {

		// JVMがシステムの起動時にシステムの設定を使ってデフォルトのLocaleを設定する

		// デフォルトのlocale情報を表示する
		Locale l = Locale.getDefault();
		System.out.println(l.getCountry() + " " + l.getLanguage());
		System.out.println(l.getDisplayCountry() + " " + l.getDisplayLanguage());

		// デフォルトの情報を変更することもできる
		Locale.setDefault(Locale.UK);
		System.out.println(Locale.getDefault().getDisplayCountry());

		// サポートされている国の一覧を取得できる
		String[] isoCountries = Locale.getISOCountries();
		for (String eachCountry : isoCountries) {
			System.out.println(eachCountry);
		}

		// サポートされている言語情報を全て取得する
		String[] languages = Locale.getISOLanguages();
		for (String eachLanguage : languages) {
			System.out.println(eachLanguage);
		}
	}

}

```


## NumberFormat
国による数字の表現の違いを管理する
NumberFormatの情報はjava.text.NumberFormatクラスで表現される

```
package classloading;

import java.text.NumberFormat;
import java.util.Locale;

public class NumberFormatTest {

	public static void main(String[] args) {

		// NumberFormatを使用して、与えられた数値をLocaleに従って表現する（以下はフランス表記に変更している）
		Double d = 1232322.3434;
		NumberFormat nf = NumberFormat.getInstance(Locale.FRANCE);
		System.out.println(nf.format(d));
	}

}

```


## DateFormat
国による日付の表し方の違いを表現する
DataFormatの情報はjava.text.DateFormatクラスで表現される
```
package classloading;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

public class DateFotmatTest {

	public static void main(String[] args) {

		// 日付のインスタンスを作成（今日の日付を取得している）
		Date d = new Date();
		// Date Formatで日付のスタイルを指定する
		DateFormat usDF = DateFormat.getDateInstance(0, Locale.US);
		DateFormat ukDF = DateFormat.getDateInstance(0, Locale.UK);

		System.out.println("Date in US Format: " + usDF.format(d));
		System.out.println("Date in UK Format: " + ukDF.format(d));

		// getTimeInstance : 現在時刻を出力する
		DateFormat timeInstance = DateFormat.getTimeInstance();
		System.out.println(timeInstance.format(d));

		// getDateTimeInstance() : 日付と時刻を出力する
		DateFormat dateTimeInstance = DateFormat.getDateTimeInstance(0, 0);
		System.out.println(dateTimeInstance.format(d));

	}

}

```

### SimpleDateFormat
DateFormatクラスの子クラスで、日付を任意のフォーマットに変換できるほか、日付の文字列表現を変換するためのパースメソッドも提供されている
```
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class SimpleDateFormatTest {

	public static void main(String[] args) {

		// SimpleDateFormat : 引数に指定したフォーマットで日付を出力できる
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy/M/dd hh:mm:ss");
		String date = sdf.format(new Date());
		System.out.println(date);

		// parse日付の文字表現を変換する 例）DBから読み取ったデータを変換する
		String dateAsString = "10-12-2022";
		SimpleDateFormat sdf1 = new SimpleDateFormat("dd-M-yyyy");
		try {
			Date date1 = sdf1.parse(dateAsString);
			System.out.println(date1);
		} catch (ParseException e) {
			e.printStackTrace();
		}
	}

}

```
