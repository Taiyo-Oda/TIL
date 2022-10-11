# Advanced JDBC

## バッチ処理（データベースに対して複数のDML(*1)文を一度に実行する）
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

### トランザクション管理の実例
```
package com.bharath.trainings.jdbc.tm;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class TransactionDAO {

	public static void main(String[] args) {
		Connection con = null;
		try {
			con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
			Statement stmt = con.createStatement();
			// commitメソッドまでのSQLを一つのトランザクションとして実行する
			con.setAutoCommit(false);
			stmt.executeUpdate("update account set bal=bal-500 where accno=2");
			stmt.executeUpdate("update account set bal=bal+500 where accno=1");
			// 前回のコミット/ロールバック以降のすべての変更を永続化し、この Connection オブジェクトが現在保持しているデータベースロックをすべて解放する
			con.commit();

		} catch (SQLException e) {
			e.printStackTrace();
			try {
				con.close();
				// 現在のトランザクションで行われたすべての変更を取り消し、この Connection オブジェクトによって現在保持されているすべてのデータベース ロックを解放する
				con.rollback();
			} catch (SQLException e1) {
				e1.printStackTrace();
			}

		}

	}
}

```

*1 : DML(Data Manipulation Language)は SELECT/INSERT/UPDATE/DELETE などテーブルに対するデータの取得・追加・更新・削除を行うコマンド


