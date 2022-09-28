# Inter Servlet Communication
全てのコードを一つのサーブレットやJSPページに入れるのではなく、処理を分散させ、それらが、互いに通信するようにすることができる。

これをリクエストディスパッチと言い、RequestDispatcherインターフェイスを使ってこれを行う。
```
// 与えられたパスにあるリソースのラッパーとして動作する RequestDispatcher オブジェクトを返す。
RequestDispatcher requestDispatcher = request.getRequestDispatcher("uri");
```
このメソッドのパラメータは、リクエスト送り先のURI

*クライアント（HTML）*
```
<html>
 <head>
  <title> Login Page </title>
 </head>
 <body>
  <form action="loginServlet" method="post">
	Email : <input type="text" name="userName"/><br/>
	Password: <input type="password" name="password"/><br/>
<input type="submit" />
  </form>
 </body>
</html>

```

*Servlet1 (DBとの接続を行う)*
```
package com.bharath.trainings.interservletcommunication;

import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String userName = request.getParameter("userName");
		String password = request.getParameter("password");
		
		try {
			Class.forName("com.mysql.jdbc.Driver");
			Connection con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
			// データベースにSQL文を送信するためのStatementオブジェクトを作成
			Statement statement = con.createStatement();
			// 与えられたSQL文を実行し、1つのResultSetオブジェクトを返す
			ResultSet resultSet = statement.executeQuery("select * from user where email='" + userName + "' and password='" + password + "'");
			// 与えられたパスにあるリソースのラッパーとして動作する RequestDispatcher オブジェクトを返す。
			RequestDispatcher requestDispatcher = request.getRequestDispatcher("homeServlet");
			if(resultSet.next()) {
				// このリクエストの属性を保存する。属性はリクエスト間でリセットされる。このメソッドは RequestDispatcher と一緒に使われることがほとんど
				request.setAttribute("message", "Welcome to InterServlet Communication " + userName);
				// サーブレットからのリクエストをサーバー上の別のリソース(サーブレット、JSPファイル、HTMLファイル)に転送します。
				//このメソッドにより、あるサーブレットがリクエストの前処理を行い、 別のリソースがレスポンスを生成することができます。
				requestDispatcher.forward(request, response);
			} else {
				requestDispatcher = request.getRequestDispatcher("login.html");
				// リソース (サーブレット、JSP ページ、HTML ファイル) のコンテンツをレスポンスにインクルードします
				requestDispatcher.include(request, response);
			}
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```

*Servlet2（Servlet1 からのリクエストを受け取り、レスポンスを生成）*
```
package com.bharath.trainings.interservletcommunication;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


// LoginServletからのリクエストを受け取り、クライアントへのレスポンスを生成するServlet
/**
 * Servlet implementation class HomeServlet
 */
@WebServlet("/homeServlet")
public class HomeServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter out = response.getWriter();
		response.setContentType("text/html");
		// 指定された属性の値を Object として返す。指定された名前の属性が存在しない場合は null を返す。
		out.print(request.getAttribute("message"));
	}
	
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter out = response.getWriter();
		response.setContentType("text/html");
		// 指定された属性の値を Object として返す。指定された名前の属性が存在しない場合は null を返す。
		out.print(request.getAttribute("message"));
	}

}

```
