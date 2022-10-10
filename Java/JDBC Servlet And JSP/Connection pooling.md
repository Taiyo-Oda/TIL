# Connection pooling
## Pooling
プーリングとは、（データベースの接続に限ったものではなく）オブジェクトなどを、ある場所（プール）にあらかじめ一定数保持しておき、クライアントの要求に応じて使用していないオブジェクトを貸し出し、使い終わったらプールに返却してもらい、またプールに入れて再利用するという仕組みのこと。<br>
以下のようなメリットがある

### パフォーマンスの向上
事前にDBとの接続を確立しておくことで、クライアントからのリクエストごとに接続処理を行う必要がなくなるためパフォーマンスが向上する。（一般にオブジェクトのインスタンス化やデータベースへの接続、切断は負荷の大きい処理と言われているため、プーリングを使用するとこういった処理を大幅に軽減することができる

### リソースの有効利用
webアプリケーションの場合、同時にアクセスするクライアント数を予測することが難しく、クライアントからのリクエストに対して際限なくDB接続を行なってしまうとサーバのリソースを圧迫したりDB製品の同時接続数のライセンスに違反してしまうことも考えられる。プールで接続を一括して管理することで、同時接続数に制限をつけることもできるため、このような問題も解決することができる。

## Conecction Poolingの設定手順
1. Apache Tomcatのlibフォルダにjarファイルを用意する
2. context.xmlのResource要素を設定する

*context.xml*
```
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
--><!-- The contents of this file will be loaded for each web application --><Context>

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
    
    <Resource name="myds" auth="Conteiner" type="javax.sql.DataSource"
    	driverClassName="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost/mydb" username="root" password="taiyou03"/>
</Context>
```

*Servlet(DBに接続できるか確認を行う)*
```
package com.bharath.trainings.servlets.cp;

import java.io.IOException;
import java.sql.Connection;
import java.sql.SQLException;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.sql.DataSource;

/**
 * Servlet implementation class ConnectionPoolingServlet
 */
@WebServlet("/ConnectionPoolingServlet")
public class ConnectionPoolingServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public ConnectionPoolingServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		try {
			Context context = new InitialContext();
			DataSource ds = (DataSource)context.lookup("java:comp/env/myds");
			Connection connection = ds.getConnection();
			System.out.println(connection);
		} catch (NamingException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```
