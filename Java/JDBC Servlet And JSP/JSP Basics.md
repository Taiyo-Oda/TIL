# JSP Basics
JSPは、データベースへの呼び出しを行い、その応答を受け取り、その応答を処理し、適切なHTML応答をWebブラウザに送り返すことができる<br>
つまり、JSPはサーブレットができることはすべてでき、サーブレットの制限を克服したのがJSPである

## Servletの欠点
*HTMLをJavaのコード内に埋め込む*ため、
1. 複雑なデザインを作成するのが難しい
2. デザイナーとプログラマーで作業の分離ができない
3. Servletによって出力されるHTMLが想像しづらい

JSPはこれらの欠点を改善するために考え出された。

## Servletの欠点を補うJSP
*JSPではHTMLの中にJavaを埋め込む*ことによって、
1. JavaとHTMLの分離ができる
2. HTMLのコードとほぼ同じ形のため、デザインの作成が容易である


## 翻訳とコンパイル
JSPファイルはクライアントからリクエストが来たときにJSPコンテナによってServletファイルに変換される。<br>
このプロセスを*翻訳*という

翻訳フェーズで生成されたServletは、コンテナによってJavaコンパイラを使ってClassファイルに*コンパイル*される

翻訳とコンパイルは最初のリクエストが来たときに一度だけ行われる

## implicit objects
全てのJSPページで利用可能な９つの暗黙のオブジェクトがある。<br>
それらを使用するために特別なことを明示的に行う必要がないため、暗黙のオブジェクトと呼ばれている。<br>

* config
* request
* response
* session
* application
* page
* pageContext
* exception
* out

つまり、これらのオブジェクトを定義したり、取得したりすることなく、Servletと同様にこれらを使用することができる。

## Scriptiong Elements
スクリプト要素を使用すると、Javaのコードを直接JSPページに埋め込むことができる。<br>
スクリプト構文には３つのタイプがある

### Declaration
```
<%! 
	int x;
	String y;
	void anyMethod() {}
 %>
```
宣言ブロックは、Servletのグローバルレベルにあるフィールドやメソッドを宣言するために使用される。
このブロックにはどんなJavaのコードでも入れることができ、変数を宣言したり、メソッドを定義したりできる。
ここのブロック内の記述の全ては、メンバ変数とメンバメソッドになる。

### Expression
```
<%= a+b %>
<%= user.getName() %>
```
このブロックには、任意のJava式を入れることができる。
ブラウザはこのブロックを幾つでも持つことができる。

### Scriptlet
```
<%
	int num1 = request.getParameter("num1");
	int num2 = request.getParameter("num2");
	int reslt = num1 + num2;
%>
```
ここには任意のJavaステートメントを入れることができる。ここにはいくらでもロジックを描くことができる。
また、Declarationブロックとは異なり、JSPのページには幾つでもスクリプトを描くことができる。

## 実例
*HTML*
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Addition</title>
</head>
<body>
<form action="addition.jsp">
<h2>Enter the Numbers</h2><br/>
Number1: <input type="text" name="number1"/><br/>
Number2: <input type="text" name="number2"/><br/>
<input type="submit" name="submit"/>
</form>
</body>
</html>
```

*JSP*
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Addition of two Numbers</title>
</head>
<body>

<%
	int num1 = Integer.parseInt(request.getParameter("number1"));
	int num2 = Integer.parseInt(request.getParameter("number2"));
%>

Sum Of <%=num1 %> and <%=num2 %> is <%=num1 + num2 %>
</body>
</html>
```


## 実例２
注：一般的にJSPでDBの操作を行うことはないが、JSPで可能な様々なことを理解するためにこのような手法でテーブルにデータベースレコードを挿入している

*html*
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Open Account</title>
</head>
<body>
	<form action="openaccount.jsp" method="post">
		<input type="text" name="accno" />
		 <input type="text" name="lastname" />
		<input type="text" name="firstname" /> 
		<input type="text" name="bal" />
		<input type="submit" />
	</form>
</body>
</html>
```

*JSP*
```
<!-- このJSPに他のJavaクラスをインポートして使用できるようにしている -->
<%@ page import="java.sql.*" language="java"
	contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>


<%!
	Connection con;
	PreparedStatement ps;
	
	public void jspInit() {
		try {
			Class.forName("com.mysql.jdbc.Driver");
			con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
			ps = con.prepareStatement("insert into account value(?,?,?,?)");
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public void jspDestroy() {
		try {
			ps.close();
			con.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
%>

<%
	int accno = Integer.parseInt(request.getParameter("accno"));
	String lastName = request.getParameter("lastname");
	String firstName = request.getParameter("firstname");
	int bal = Integer.parseInt(request.getParameter("bal"));
	
	ps.setInt(1, accno);
	ps.setString(2, lastName);
	ps.setString(3, firstName);
	ps.setInt(4, bal);
	
	ps.executeUpdate();
%>

<%@ include file="openaccount.html" %>

```

## ErrorHandling
JSPで例外を取得して出力ができるようにすることで、エラーが発生した場合に、ユーザーフレンドリーな画面を作成することができる。

*エラーページ*
```
<!-- isErrorPage：これをtureにすることで、このJSPページに例外を引き渡すことができるようにする -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" isErrorPage="true"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Error Page</title>
</head>
<body>
	<P>Sorry,an error has occurd</P>
	<%= exception.getMessage() %>
</body>
</html>
```

*このページでエラーが発生*
```
<!-- errorPage：どのエラーページを使用するかを指定する -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" errorPage="errorHandler.jsp"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Error Generator</title>
</head>
<body>
<%
	String s = "123abc";
	int num = Integer.parseInt(s);
%>
</body>
</html>
```
