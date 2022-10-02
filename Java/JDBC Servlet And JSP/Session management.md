# Session management
## HTTPはステートレスプロトコル
プロトコルがステートレスであるということは、サーバがリクエストを処理してレスポンスを送り返した後、継続的な接続を維持しない(状態を保てない＝ステートレスである)ことを意味する。<br>

これには以下のような利点がある

### パフォーマンスの向上
ステートレスなプロトコルはオーバーヘッド(*1)を維持する必要がないため。ステートフルなプロトコルに比べて通信のパフォーマンスが良い

### アプリケーションの拡張性
ステートレスなプロトコルで通信を行う場合、クライアントからリクエストがあるたびに接続を確率し、レスポンスを返した後は、接続を維持しないため、同じアプリケーションを複数のサーバでホストすることができる。
→状態を保持する必要がないため、どのサーバでどのアプリケーションがリクエストを処理してもOK

一方で
ログイン情報、ショッピングカート内の情報など、*状態を維持したい場合でも、状態を維持できないというデメリット*がある


## セッショントラッキング
セッショントラッキングとは、アプリケーションのリクエストにまたがって状態を維持すること。<br>
例）ログイン状態の維持、カートの中身の情報の維持など

### セッション
webアプリケーションなどの一連の処理（あるウェブサイトに初めてアクセスしてから、ブラウザを閉じるか、そのウェブサイトを離れるまでの全時間）の流れのことを*セッション*という<br>

*web.xml(設定ファイル)*
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
	id="WebApp_ID" version="4.0">
	<servlet>
		<servlet-name>sourceServlet</servlet-name>
		<servlet-class>com.bharath.trainings.servlet.SourceServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>sourceServlet</servlet-name>
		<url-pattern>/sourceServlet</url-pattern>
	</servlet-mapping>
	
	<servlet>
		<servlet-name>targetServlet</servlet-name>
		<servlet-class>com.bharath.trainings.servlet.TargetServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>targetServlet</servlet-name>
		<url-pattern>/targetServlet</url-pattern>
	</servlet-mapping>
	
</web-app>
```

*HTML*<br>
名前を入力してsubmitボタンを押下すると、POSTメソッドによってパラメータがSourceServletに送られる
```
<html>
 <head>
  <title> User Page </title>
 </head>

 <body>
 <h1>Enter User Name:</h1>
  <form method="post" action="sourceServlet">
  User Name :<input name="userName"/>
  <input type="submit" value="send" name="submitButton"/>
  </form>
 </body>
</html>

```

*SourceServlet*<br>
リクエストパラメータからuserNameを取得し、作成したセッションに取得した値をセットする。
クライアントには、TargetServletへリクエストを送るためのリンクを返す。
```
package com.bharath.trainings.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class SourceServlet extends HttpServlet {
	
	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
		String userName = req.getParameter("userName");
		// このリクエストに関連付けられた現在のセッションを返します。 リクエストがセッションを持っていない場合は、セッションを作成します。
		HttpSession session = req.getSession();
		// 指定された名前を使用して、このセッションにオブジェクトをバインドします。同じ名前のオブジェクトがすでにセッションにバインドされている場合、そのオブジェクトは置き換えられます。
		session.setAttribute("user", userName);
		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		out.print("<a href='targetServlet'>Click Here To Get The User Name</a>");
	}

}

```


*TargetServlet*<br>
TargetServletはSourceServletが設定したセッションからuserNameを取得して、それを表示する
```
package com.bharath.trainings.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class TargetServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

		HttpSession session = req.getSession();
		// このセッションで指定された名前でバインドされているオブジェクトを返します。
		String userName = (String)session.getAttribute("user");
		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		out.print("<h1>User Name Is : " + userName + "</h1>");
	}
	
}

```

Client → SourceServlet → Client → TargetServletという一連の処理の中で、*Coockie*を使用してセッションを記憶している

## Coockie
HTTPのリクエスト・ヘッダの一部として、クライアントとサーバ間でデータを交換するために使用できるテキスト情報の名前と値の組みのこと。
WebコンテナとWebクライアントの間のセッションを維持するために使われ、Webコンテナが作成するセッションIDという一意の値をCoockieとして送受信することでセッションの管理を行う

*SourceServlet*
```
package com.bharath.trainings.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class SourceServlet extends HttpServlet {
	
	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
		// クライアントがこのリクエストで送信したすべてのCookieオブジェクトを含む配列を返します。
		Cookie[] cookies = req.getCookies();
		for (int i = 0; i < cookies.length; i++) {
			System.out.println(cookies[i].getName());
			System.out.println(cookies[i].getValue());
		}
		// 指定されたクッキーをレスポンスに追加します。(セッションIDをカスタマイズしたいときに使用する）
		resp.addCookie(new Cookie("securityToken", "12345"));
		
		String userName = req.getParameter("userName");
		// このリクエストに関連付けられた現在のセッションを返します。 リクエストがセッションを持っていない場合は、セッションを作成します。
		HttpSession session = req.getSession();
		// 指定された名前を使用して、このセッションにオブジェクトをバインドします。同じ名前のオブジェクトがすでにセッションにバインドされている場合、そのオブジェクトは置き換えられます。
		session.setAttribute("user", userName);
		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		out.print("<a href='targetServlet'>Click Here To Get The User Name</a>");
	}

}

```

*TorgetServlet*
```
package com.bharath.trainings.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class TargetServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
		Cookie[] cookies = req.getCookies();
		for (int i = 0; i < cookies.length; i++) {
			System.out.println(cookies[i].getName());
			System.out.println(cookies[i].getValue());
		}

		HttpSession session = req.getSession();
		// このセッションで指定された名前でバインドされているオブジェクトを返します。
		String userName = (String)session.getAttribute("user");
		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		out.print("<h1>User Name Is : " + userName + "</h1>");
	}
	
}

```


## URLリライト
クライアントでCookieが無効になっている場合の対策
※ここは完全に理解できていないため復習が必要

*SourceServlet*
```
package com.bharath.trainings.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class SourceServlet extends HttpServlet {
	
	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		
		// クライアントがこのリクエストで送信したすべてのCookieオブジェクトを含む配列を返します。
		Cookie[] cookies = req.getCookies();
		// ブラウザでCookieが無効になっている場合は表示されない
		if (cookies != null) {
			for (int i = 0; i < cookies.length; i++) {
				System.out.println(cookies[i].getName());
				System.out.println(cookies[i].getValue());
			}
		}
		// 指定されたクッキーをレスポンスに追加します。(セッションIDをカスタマイズしたいときに使用する）
		resp.addCookie(new Cookie("securityToken", "12345"));
		
		String userName = req.getParameter("userName");
		// このリクエストに関連付けられた現在のセッションを返します。 リクエストがセッションを持っていない場合は、セッションを作成します。
		HttpSession session = req.getSession();
		// 指定された名前を使用して、このセッションにオブジェクトをバインドします。同じ名前のオブジェクトがすでにセッションにバインドされている場合、そのオブジェクトは置き換えられます。
		session.setAttribute("user", userName);
		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		
		// URLリライト（Cookieが無効になっている場合に備えて、URLのセッションIDの末尾を書き換える）
		String url = "targetServlet?sessionId=123";
		
		out.print("<a href='" + url + "'>Click Here To Get The User Name</a>");
	}

}

```

*1 本来の処理を行うために余計に必要となってしまう処理のこと。
