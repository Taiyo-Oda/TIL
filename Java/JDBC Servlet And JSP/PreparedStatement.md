# PreparedStatement

*preparedstatement*は、作成された時点でコンパイルされて保存されるため、ステートメントを実行するたびにコンパイルする必要がなく、アプリケーションのパフォーマンスを向上させることができる

```
con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
// prepareStatement：パラメータ化されたSQL文をDBに送信するためのオブジェクトを作成する
stmt = con.prepareStatement("insert into product values(?,?,?,?)");
```

PreparedStatementでは値をハードコードしたり、動的に挿入したりする代わりに、疑問符を使用している。このようにして、このstatementをここ(オブジェクトが作成された時点)でコンパイルする。

```
int id = Integer.parseInt(request.getParameter("id"));
String name = request.getParameter("name");
String desc = request.getParameter("description");
int price = Integer.parseInt(request.getParameter("price"));
		
		try {
			// 指定されたパラメータに与えられた値を設定する
			stmt.setInt(1, id);
			stmt.setString(2, name);
			stmt.setString(3, desc);
			stmt.setInt(4, price);
			// INSERT、UPDATE、DELETE文、またはSQL DDL文のような何も返さないSQL文のいずれかを指定したSQL文を実行する
			int result = stmt.executeUpdate();
```

この疑問符に値を設定するには、set×××メソッドを使用する。
このようにパラメータをオフセットすることを、パラメータのバインドという。
パラメータのバインドが完了したら、このpreparedstatementに対してexecuteクエリを実行する

statementの場合、executeを実行するときに、パラメータとしてクエリを渡す必要があるが、preparedstatementの場合、preparedstatementオブジェクトが作成された段階でクエリを渡しているので、実行するときに渡す必要がない。<br>
→コンパイルが一度だけですむ

*html*
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> Product Creation</title>
 </head>

 <body>
 <h2>Enter Product Details</h2><br/>
  <form action="ProductServlet" method="post">
    Id : <input type="text" name="id"/><br/>
	Name : <input type="text" name="name"/><br/>
	Description: <input type="text" name="description"/><br/>
	Price: <input type="text" name="price"/><br/>
<input type="submit" />
</form>
 </body>
</html>

```

*servlet*
```
package com.bharath.trainings.servlets.jdbc.preparedstatement;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ProductServlet
 */
@WebServlet("/ProductServlet")
public class ProductServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	Connection con;
	PreparedStatement stmt;
	
	public void init() {
		try {
			Class.forName("com.mysql.jdbc.Driver");
			con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
			// prepareStatement：パラメータ化されたSQL文をDBに送信するためのオブジェクトを作成する
			stmt = con.prepareStatement("insert into product values(?,?,?,?)");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		int id = Integer.parseInt(request.getParameter("id"));
		String name = request.getParameter("name");
		String desc = request.getParameter("description");
		int price = Integer.parseInt(request.getParameter("price"));
		
		try {
			// 指定されたパラメータに与えられた値を設定する
			stmt.setInt(1, id);
			stmt.setString(2, name);
			stmt.setString(3, desc);
			stmt.setInt(4, price);
			// INSERT、UPDATE、DELETE文、またはSQL DDL文のような何も返さないSQL文のいずれかを指定したSQL文を実行する
			int result = stmt.executeUpdate();
			
			response.setContentType("text/html");
			// getWriter:クライアントに文字テキストを送信することができる PrintWriter オブジェクトを返す。
			PrintWriter out = response.getWriter();
			out.print("<br>" + result + "Products Created</br>");
			
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
	}
	
	public void destroy() {
		try {
			stmt.close();
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```
