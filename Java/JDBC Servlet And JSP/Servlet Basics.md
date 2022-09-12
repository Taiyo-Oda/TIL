# Servlet Basics
## Servletとは
ウェブコンテナ上で動作し、クライアントからのリクエストを処理することができる（DBの呼び出し、ファイルへの書き込み、ファイルからの読み込みなど）

サーブレットには以下の３つのライフサイクルメソッドがある
* init()
* service()
* destroy()

そして、これら３つのメソッドに関連する４つのライフサイクルがある
* instantiation
* initalization
* servicing
* destruction

### instantiation
クライアントから何かしらのリクエストを受けると、コンテナは最初に処理を行うためのサーブレットクラスをメモリにロードし、そのインスタンスを作成する
これを*instantiation*という

### initalization
init()を読んでサーブレットを初期化する<br>
このメソッドはコンテナから一度だけ呼び出される

### servicing
service()を呼び出す<br>
このserviceメソッドにはビジネスロジックが全て格納される。
このメソッドはn回（リクエストの数だけ）呼ばれる

### destruction
destroyを読んでサーブレッドメモリから削除する


## Servletを用いてクライアントからのリクエスト処理を行う例
#### HTML（クライアント）
クライアントでsubmitが行われると、「additionServlet」というパスに対してHTTPリクエストが発行される
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<form action="additionServlet">
	<h2>Enter the Numbers</h2><br>
	Number1: <input type="text" name="number1"><br>
	Number2: <input type="text" name="number2"><br>
	<input type="submit" name="">
</form>
</body>
</html>
```

Servlet
```
package com.bharath.trainings.servlets;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

public class AdditionServlet extends GenericServlet {

	@Override
	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {

		if (req.getParameter("number1") != null && req.getParameter("number2") != null) {
			Long num1 = Long.parseLong(req.getParameter("number1"));
			Long num2 = Long.parseLong(req.getParameter("number2"));

			PrintWriter out = res.getWriter();
			out.println("The Result is " + (num1 + num2));
		}
	}

}

```

#### アプリケーションサーバの定義ファイル(web.xml)
URLとサーブレットの対応をて定義するファイル<br>
サーブレット定義の部分でjavaのクラスにHTTPリクエストで指定するための名前を定義している<br>
サーブレットマッピングの部分では、URLへのHTTPリクエストに対して、サーブレット定義で定義したサーブレットを呼び出す指定をしている
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
		<servlet-name>HelloServlet</servlet-name>
		<servlet-class>com.bharath.trainings.servlets.HelloWorldServret</servlet-class>
	</servlet>
	<!-- サーブレットマッピング -->
	<servlet-mapping>
		<servlet-name>HelloServlet</servlet-name>
		<url-pattern>/hello</url-pattern>
	</servlet-mapping>

	<!-- サーブレット定義 -->
	<servlet>
		<servlet-name>AdditionServlet</servlet-name>
		<servlet-class>com.bharath.trainings.servlets.AdditionServlet</servlet-class>
	</servlet>
	<!-- サーブレットマッピング -->
	<servlet-mapping>
		<servlet-name>AdditionServlet</servlet-name>
		<url-pattern>/additionServlet</url-pattern>
	</servlet-mapping>

</web-app>
```
