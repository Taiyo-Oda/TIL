# Dynamic Web Application
HTTPプロトコル(*1)では、WebクライアントがWebサーバにアクセスするためのいくつかのメソッドをサポートしている<br>
その中でも主要なものが以下の２つである
* GET
* POST
これら２つのメソッドではデータの受け渡し方が異なる

## GETメソッド
リクエストパラメータをクエリ文字列としてURLの中に含めてWebサーバに送る
```
// ?以降がクエリ文字列
/do_calc_get.php?arg1=123&arg2=456
```
Webサーバ側では、受け取ったリクエストライン（*2）の中にクエリ文字列が存在すれば、そこに含まれるパラメータをアプリケーションに渡す

GETメソッドには以下のような問題がある
* URLの中にパラメータが含まれるため、どのような情報をWebサーバに送ったのかが、第三者に漏洩しやすい
* リクエストパラメータの情報が含まれたリクエストラインがサーバのログに残ってしまい、そこから情報が漏洩してしまう可能性がある
*  WebサーバやWebクライアントで、利用できるURLの文字数が指定されている場合、送れる情報量が制限されてしまう

一方で
* URLにパラメータが含まれるのでパラメータごと記憶したり（ブックマークを使ったパラメータの保存）、人に伝える（URLをコピペして共有する）などの場合は便利<br>


## POSTメソッド
リクエストパラメータをHTTPリクエストのメッセージボディの部分に含めてWebサーバに送る
これによって、上記したGETリクエストの問題点を回避することができる

一方で
* メッセージボディにパラメータが含まれるため、ブックマークを使用したパラメータの保存はできない


## GETとPOSTの使い分け
よって<br>
機密情報を送信する場合や、決済処理などの副作用を伴う(*3)処理、大量の情報を送信する場合はPOSTメソッドを使用すべきである<br>
前述の条件に当てはまらず、副作用がない処理ではGETリクエストを使用した方が、パラメータごと保存できるというメリットを活かすことができる


### Servletを使用してUserを作成
*addUser.HTML*
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Add User</title>
</head>
<body>
	<h1>User Registration:</h1>
	<form method="post" action="addServlet">
		<table>
			<tr>
				<td>First Name:</td>
				<td><input name="firstName" /></td>
			</tr>
			<tr>
				<td>Last Name :</td>
				<td><input name="lastName" /></td>
			</tr>
			<tr>
				<td>Email:</td>
				<td><input name="email" /></td>
			</tr>
			<tr>
				<td>Password:</td>
				<td><input name="password" type="password" /></td>
			</tr>
			<tr>
				<td />
				<td><input type="submit" value="Add" /></td>
		</table>
	</form>
</body>
</html>
```

*CreateUserServlet.java*
```
package com.bharath.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CreateUserServlet
 */
// ここではアノテーションを利用してサーブレットをURIにマッピングしている
@WebServlet("/addServlet")
public class CreateUserServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private Connection connection;
       
	// DBとの接続を行う
	public void init() {
		try {
			// Tomcatは自動的にJDBCドライバを検索してロードするサービスプロバイダメカニズムを無効にしているため、ドライバクラスを手動でロードしている
			Class.forName("com.mysql.jdbc.Driver");
			connection = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
		} catch (SQLException e) {
			e.printStackTrace();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String firstName = request.getParameter("firstName");
		String lastName = request.getParameter("lastName");
		String email = request.getParameter("email");
		String password = request.getParameter("password");
		
		try {
			Statement statement = connection.createStatement();
			// INSERT、UPDATE、DELETE文、またはSQL DDL文のような何も返さないSQL文のいずれかを指定したSQL文を実行する
			int result = statement.executeUpdate(
					"insert into user values('" + firstName + "','" + lastName + "','" + email + "','" + password + "')"
					);
			// クライアントに文字テキストを送信することができる PrintWriter オブジェクトを返す
			PrintWriter out = response.getWriter();
			if(result > 0) {
				out.print("<h1>USER CREATED</h1>");
			} else {
				out.print("<h1>Error Creating the User</h1>");
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	// DBとの接続を終了する
	public void destroy() {
		try {
			connection.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```


### Servletを使用してUser情報の更新
*updateUser.HTML*
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Update User</title>
</head>
<body>
	<h1>Update User:</h1>
	<form method="post" action="updateServlet">
		<table>
			<tr>
				<td>Email:</td>
				<td><input name="email" /></td>
			</tr>

			<tr>
				<td>Password:</td>
				<td><input name="password" type="password" /></td>
			</tr>
			<tr>
				<td />
				<td><input type="submit" value="Update" /></td>
		</table>
	</form>
</body>
</html>
```

*UpdateUserServlet.java*
```
package com.bharath.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CreateUserServlet
 */
// ここではアノテーションを利用してサーブレットをURIにマッピングしている
@WebServlet("/updateServlet")
public class UpdateUserServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private Connection connection;
       
	// DBとの接続を行う
	public void init() {
		try {
			// Tomcatは自動的にJDBCドライバを検索してロードするサービスプロバイダメカニズムを無効にしているため、ドライバクラスを手動でロードしている
			Class.forName("com.mysql.jdbc.Driver");
			connection = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
		} catch (SQLException e) {
			e.printStackTrace();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String email = request.getParameter("email");
		String password = request.getParameter("password");
		
		try {
			Statement statement = connection.createStatement();
			// INSERT、UPDATE、DELETE文、またはSQL DDL文のような何も返さないSQL文のいずれかを指定したSQL文を実行する
			int result = statement.executeUpdate(
					"update user set password='" + password + "' where email='" + email + "'"
					);
			// クライアントに文字テキストを送信することができる PrintWriter オブジェクトを返す
			PrintWriter out = response.getWriter();
			if(result > 0) {
				out.print("<h1>Password Updated</h1>");
			} else {
				out.print("<h1>Error Updating the Password</h1>");
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	// DBとの接続を終了する
	public void destroy() {
		try {
			connection.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```


### Servlet を使用してUser情報の読み込み（表示）
*ReadUserServlet.java*
```
package com.bharath.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CreateUserServlet
 */
// ここではアノテーションを利用してサーブレットをURIにマッピングしている
@WebServlet("/readServlet")
public class ReadUserServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private Connection connection;
       
	// DBとの接続を行う
	public void init() {
		try {
			// Tomcatは自動的にJDBCドライバを検索してロードするサービスプロバイダメカニズムを無効にしているため、ドライバクラスを手動でロードしている
			Class.forName("com.mysql.jdbc.Driver");
			connection = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
		} catch (SQLException e) {
			e.printStackTrace();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {		
		try {
			Statement statement = connection.createStatement();
			// executeQuery：与えられたSQL文を実行し、1つのResultSetオブジェクトを返す
			ResultSet resultSet = statement.executeQuery("select * from user");
			// getWriter：クライアントに文字テキストを送信することができる PrintWriter オブジェクトを返す
			PrintWriter out = response.getWriter();
			out.print("<table>");
			out.print("<tr>");
			out.print("<th>");
			out.print("FirstName");
			out.print("</th>");
			out.print("<th>");
			out.print("LastName");
			out.print("</th>");
			out.print("<th>");
			out.print("Email");
			out.print("</th>");
			out.print("</tr>");
			while(resultSet.next()) {
				out.print("<tr>");
				
				out.print("<td>");
				out.print(resultSet.getString(1));
				out.print("</td>");
				out.print("<td>");
				out.print(resultSet.getString(2));
				out.print("</td>");
				out.print("<td>");
				out.print(resultSet.getString(3));
				out.print("</td>");
				
				out.print("</tr>");
			}
			out.print("</table>");
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	// DBとの接続を終了する
	public void destroy() {
		try {
			connection.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```


### servletを使用してUser情報の削除
*deleteUser.HTML*
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Delete User</title>
</head>
<body>
	<h1>Delete User:</h1>
	<form method="post" action="deleteServlet">
		<table>
			<tr>
				<td>Email:</td>
				<td><input name="email" /></td>
			</tr>
			<tr>
				<td />
				<td><input type="submit" value="Delete" /></td>
		</table>
	</form>
</body>
</html>
```

*DeleteUserServlet.java*
```
package com.bharath.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CreateUserServlet
 */
// ここではアノテーションを利用してサーブレットをURIにマッピングしている
@WebServlet("/deleteServlet")
public class DeleteUserServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private Connection connection;
       
	// DBとの接続を行う
	public void init() {
		try {
			// Tomcatは自動的にJDBCドライバを検索してロードするサービスプロバイダメカニズムを無効にしているため、ドライバクラスを手動でロードしている
			Class.forName("com.mysql.jdbc.Driver");
			connection = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
		} catch (SQLException e) {
			e.printStackTrace();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String email = request.getParameter("email");
		
		try {
			Statement statement = connection.createStatement();
			// INSERT、UPDATE、DELETE文、またはSQL DDL文のような何も返さないSQL文のいずれかを指定したSQL文を実行する
			int result = statement.executeUpdate("delete from user where email='" + email + "'");
			// クライアントに文字テキストを送信することができる PrintWriter オブジェクトを返す
			PrintWriter out = response.getWriter();
			if(result > 0) {
				out.print("<h1>User Deleted</h1>");
			} else {
				out.print("<h1>User not found in the datebase</h1>");
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	// DBとの接続を終了する
	public void destroy() {
		try {
			connection.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```


*1　通信プロトコル（情報をやり取りするための取り決め）の一つで、webクライアントと、webサーバが通信を行う際に使用される。通信プロトコルでは主に「伝達の方法」と「情報の意味づけ」を規定している。<br>
*2　HTTPリクエストの一部。メソッド, URI, HTTPバージョンの３つの文字列で成り立っている。<br>
*3　同じ条件を何度繰り返しても、同じ結果が得られることを「副作用がない」という(例：ある位置時点で、Googleに何度同じ検索キーワードを入力しても結果は変わらない)<br>
