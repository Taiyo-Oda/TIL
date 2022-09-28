# Pre Initialization Srvlet

サーブレットの初期化には次の２種類がある
1. lazy initialization
2. pre initialization

## lazy initialization
クライアントからリクエストが来たときに、初期化が行われる。<br>
デフォルトの設定

## pre initialization
クライアントからのリクエストが来る前に初期化を行う<br>
Servletファイル内でアノテーションを使用して行う方法と、設定ファイル(web.xml)で設定する方法の2種類がある<br>

*loadOnStartup要素を使ってコンテナに事前初期化を要求する*
```
// Servletの事前初期化　loadOnStartupの数値が小さいほど優先順位が高い
@WebServlet(urlPatterns="/PreInitServlet", loadOnStartup=0)
```

```
<!-- 事前初期化の設定 -->
<load-on-startup>0</load-on-startup>
```
設定した数値によってコンテナにサーブレットの優先順位を伝える。数値が小さいほど、優先順位が高くなる

起動時にロードされるサーブレットやアプリケーション内で初期化されるサーブレットが複数ある場合、この数値はコンテナにどれを最初にロードすべきか、その順番を知らせる




## 実例
*アノテーションを使用した事前初期化*
```
package com.bharath.trainings.servlets.preinit;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class PreInitServlet
 */
// Servletの事前初期化　loadOnStartupの数値が小さいほど優先順位が高い
@WebServlet(urlPatterns="/PreInitServlet", loadOnStartup=0)
public class PreInitServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public void init() {
    	System.out.println("Inside the init method");
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().write("From the pre init Servlet");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```


*設定ファイルを使用して事前初期化*

設定ファイル
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
	id="WebApp_ID" version="4.0">
	
	<display-name>PreInitDemo</display-name>
	<!-- サーブレット定義 -->
	<servlet>
		<servlet-name>PreInitervlet</servlet-name>
		<servlet-class>com.bharath.trainings.servlets.preinit.PreInitServlet</servlet-class>
		<!-- 事前初期化の設定 -->
		<load-on-startup>0</load-on-startup>
	</servlet>
	<!-- サーブレットマッピング -->
	<servlet-mapping>
		<servlet-name>PreInitServlet</servlet-name>
		<url-pattern>/preInitServlet</url-pattern>
	</servlet-mapping>

</web-app>
```

Servlet
```
package com.bharath.trainings.servlets.preinit;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class PreInitServlet
 */
public class PreInitServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public void init() {
    	System.out.println("Inside the init method");
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().write("From the pre init Servlet");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```
