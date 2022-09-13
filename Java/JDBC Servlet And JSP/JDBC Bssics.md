# JDBC Bssics

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
