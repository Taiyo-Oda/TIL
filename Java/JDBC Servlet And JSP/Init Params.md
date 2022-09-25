# Init Params
initParamsはサーブレットに供給されるテキスト情報のNameとValueの組みのことで、初期化時にweb.xmlファイルを通して供給される

*web.xml*
```
<servlet>
		<servlet-name>ReadUserServlet</servlet-name>
		<servlet-class>com.bharath.servlet.ReadUserServlet</servlet-class>
		<!-- Servletで行なっていたinitパラメータの設定をweb.xmlで行う -->
		<init-param>
			<param-name>dbUrl</param-name>
			<param-value>jdbc:mysql://localhost/mydb</param-value>
		</init-param>
```

サーブレットクラスを宣言すると、init-param要素を使って、initパラメータを作成することができる<br>
このようにweb.xmlで設定すると、サーブレットコンテナは初期化時にinitメソッドを通して、サーブレットにパラメータを渡せるようになる

*Servlet*
```
// web.xmlの内容を読み込んで、configオブジェクトに格納している
	public void init(ServletConfig config)
```

ServletConfigオブジェクトが作成され、web.xmlファイルを読み込んでその情報をServletConfigオブジェクトにセットする。こうすることで、サーブレットクラスの中で、その情報を簡単に取得することができる

```
String dbUser = config.getInitParameter("dbUrl"),
```

上記のようにgetを使用して、値を取得することができる

init Paramを使用することで、値のハードコーディングを避けることができる.<br>
→DB接続に関する情報が変わるたびに、Servletを確認して、コードを変更する必要がlなくなる

*web.xml*
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
	id="WebApp_ID" version="4.0">
	<display-name>ServletBasics</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.jsp</welcome-file>
		<welcome-file>default.htm</welcome-file>
	</welcome-file-list>
	<!-- サーブレット定義 -->
	<servlet>
		<servlet-name>ReadUserServlet</servlet-name>
		<servlet-class>com.bharath.servlet.ReadUserServlet</servlet-class>
		<!-- Servletで行なっていたinitパラメータの設定をweb.xmlで行う -->
		<init-param>
			<param-name>dbUrl</param-name>
			<param-value>jdbc:mysql://localhost/mydb</param-value>
		</init-param>
		<init-param>
			<param-name>dbUser</param-name>
			<param-value>root</param-value>
		</init-param>
		<init-param>
			<param-name>dbPassword</param-name>
			<param-value>taiyou03</param-value>
		</init-param>
	</servlet>
	<!-- サーブレットマッピング -->
	<servlet-mapping>
		<servlet-name>ReadUserServlet</servlet-name>
		<url-pattern>/readServlet</url-pattern>
	</servlet-mapping>
</web-app>
```

*Servlet*
```
package com.bharath.servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CreateUserServlet
 */
public class ReadUserServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private Connection connection;
       
	// web.xmlの内容を読み込んで、configオブジェクトに格納している
	public void init(ServletConfig config) {
		try {
			// Tomcatは自動的にJDBCドライバを検索してロードするサービスプロバイダメカニズムを無効にしているため、ドライバクラスを手動でロードしている
			Class.forName("com.mysql.jdbc.Driver");
			connection = DriverManager.getConnection(
					config.getInitParameter("dbUrl"),
					config.getInitParameter("dbUser"),
					config.getInitParameter("dbPassword"));
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
