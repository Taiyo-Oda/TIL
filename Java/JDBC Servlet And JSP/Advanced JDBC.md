# Advanced JDBC

## バッチ処理（データベースに対して複数のDML文を一度に実行する）
```
package com.bharath.trainings.jdbc.batch;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class BatchDAO {
	public static void main(String[] args) {
		try (Connection con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
				Statement stmt = con.createStatement();) {
			// 指定されたSQLコマンドを、このStatementオブジェクトの現在のコマンドリストに追加する
			// このリストのコマンドは、executeBatch メソッドを呼び出すことにより、バッチとして実行することができる
			stmt.addBatch("insert into account values(1,'clinton','bill',10000)");
			stmt.addBatch("insert into account values(2,'clinton','hillary',20000)");
			// コマンドのバッチをデータベースに送信して実行させ、すべてのコマンドが正常に実行された場合、更新回数の配列を返す
			int[] result = stmt.executeBatch();

			for (int i = 0; i < result.length; i++) {
				System.out.println(result[i]);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}

	}
}

```


## メタデータ（データに関するデータ)の取得方法
```
package com.bharath.trainings.jdbc.metadata;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Statement;

public class MetadataDAO {
	public static void main(String[] args) {
		try (Connection con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
				Statement stmt = con.createStatement();) {
			// 与えられたSQL文を実行し、1つのResultSetオブジェクトを返す
			ResultSet resultSet = stmt.executeQuery("select * from account");
			// この ResultSet オブジェクトの列の数、型、およびプロパティを取得する
			ResultSetMetaData resultSetMetaData = resultSet.getMetaData();
			// この ResultSet オブジェクトのカラム数を取得する
			int columnCount = resultSetMetaData.getColumnCount();

			for (int i = 1; i <= columnCount; i++) {
				System.out.println(resultSetMetaData.getColumnName(i));
				System.out.println(resultSetMetaData.getColumnTypeName(i));
			}

		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```

## トランザクション管理
* 商品在庫の管理
* クレジットカードの請求処理
などの業務上まとめて実行すべき一連の処理を*「トランザクション」*という。トランザクションで重要なのは、含まれるすべての処理が成功しなければならないということである。
