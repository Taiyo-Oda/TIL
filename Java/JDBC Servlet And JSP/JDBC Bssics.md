# JDBC Bssics

JDBC APIを実装するためにSQL Connector jarを取得する必要がある

JDBCアーキテクチャは４つのコンポーネントで構成される
* JDBC Client
* JDBC API
* JDBC Driver
* Driver Manager

JDBC Clientはアプリケーションコードのこと

JDBC APIはいくつかのインターフェイスとクラスで構成されている
それらをJDBC Clientコード内で使用して、JDBC Driverに接続する。
そのドライバーが内部でデータベースに接続し、データベースに対する操作を実行する。

JDBC DriverはJavaアプリケーションとDBをつなぐ役割を果たす
アプリケーション側で指示を受けるとDBとの接続を確立し、sqlをDBに対して実行する
そして、DBが応答を返すと、Driverはその応答をJavaが理解できるように変換する

Driver ManagerはDriverとClientの間のヘルパとして機能する。

データベースに対する操作は４種類あり、それぞれの頭文字をとって
CRUDと呼ばれている

データベースに対する操作は以下の手順で行われる
1. データベースへの接続の確立
2. ステートメントオブジェクトの作成
3. ステートメントの実行
4. ステートメントオブジェクトとDB接続のクローズ

### データベースへの接続の確立
```
package com.bharath.jdbc.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class AccountDAO {

	public static void main(String[] args) {

		try {
			// DBへの接続を行う
			Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
			System.out.println(connection);
		} catch (SQLException e) {
			e.printStackTrace();
		}

	}

}

```


### ステートメントオブジェクトの作成・ステートメントの実行
```
package com.bharath.jdbc.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class AccountDAO {

	public static void main(String[] args) {

		try {
			// DBへの接続を行う
			Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
			System.out.println(connection);

			// ステートメントオブジェクトの作成(sql文をDBに送るためのオブジェクト)
			Statement statement = connection.createStatement();
			
			// executeUpdate() : DBへの挿入、更新、削除を行い、影響（追加、更新、削除）されたレコードの数を返す
			int insert = statement.executeUpdate("insert into account values(1, 'thippireddy', 'bharath', 10000)");
			int update = statement.executeUpdate("update account set bal=5000 where accno=1");
			int delete = statement.executeUpdate("delete from account where accno=1");
			
			// ResultSet : データベースから取得したレコードをオブジェクト指向で表現したもの
			// executeQuery() : 検索結果のレコードの配列を返す
			ResultSet rs = statement.executeQuery("select * form account");
			// 論理ポインタ（レコード領域を移動するカーソル）を移動させる（次のカラムを取得する）
			while(rs.next()) {
				System.out.println(rs.getString(2));
				System.out.println(rs.getString(3));
				System.out.println(rs.getString(4));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}

	}

}

```

### クローズ
```
package com.bharath.jdbc.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class AccountDAO {

	public static void main(String[] args) {

		// tryブロックのリソースセクションに記述することで明示的にcloseする必要がなくなる
		try (Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
				Statement statement = connection.createStatement();
				ResultSet rs = statement.executeQuery("select * form account");) {
			// 論理ポインタ（レコード領域を移動するカーソル）を移動させる（次のカラムを取得する）
			while (rs.next()) {
				System.out.println(rs.getString(2));
				System.out.println(rs.getString(3));
				System.out.println(rs.getString(4));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}

	}

}

```
