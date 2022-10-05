# Creating Custom Tags
JSPの仕様やAPIに付属しているタグだけでは、業務上の必要性に対応できない場合、カスタムタグを作成できる。

カスタムタグの作成は次の２ステップで行われる。
1. タグがjspページで使われたときにやりたいことを定義するJavaクラスを作成する
2. 作成したJavaクラスとタグ名を対応させるために、CustomLipDescriptor(TLD)と呼ばれるファイル内にマッピングする。

## 実例(Userの登録情報を表示するカスタムタグの作成)
*HTML*<br>
Emailを入力して、DBからUserの詳細な情報を取得する
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>User Input</title>
</head>
<body>
	<form action="userdetails.jsp">
		Email<input type="text" name="email" />
		<br /><input type="submit" value="submit" />
	</form>
</body>
</html>
```

*Javaファイル*<br>
タグがjspページで使われたときにやりたいことを定義するJavaクラス
```
package com.bharath.trainings.jsp.customtags;

import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import javax.servlet.ServletRequest;
import javax.servlet.jsp.JspException;
import javax.servlet.jsp.JspWriter;
import javax.servlet.jsp.tagext.Tag;
import javax.servlet.jsp.tagext.TagSupport;

// TagSupport : Tagを実装した新しいタグハンドラを定義するための基底クラス
public class ResultHandler extends TagSupport {

	Connection con;
	PreparedStatement stmt;

	public ResultHandler() {
		try {
			Class.forName("com.mysql.jdbc.Driver");
			con = DriverManager.getConnection("jdbc:mysql://localhost/mydb", "root", "taiyou03");
			stmt = con.prepareStatement("select * from user where email=?");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	@Override
	public int doStartTag() throws JspException {
		// 入力されたEmailの値をリクエストオブジェクトから取得している
		ServletRequest request = pageContext.getRequest();
		String email = request.getParameter("email");

		try {
			// 検索するEmailの値をSQL文にセット
			stmt.setString(1, email);
			// SQL クエリを実行し、クエリによって生成された ResultSet オブジェクト作成
			ResultSet rs = stmt.executeQuery();
			JspWriter out = pageContext.getOut();
			if (rs.next()) {
				out.print("User Details are :<br/>First Name : ");
				out.print(rs.getString(1));
				out.print("<br/>Last Name : ");
				out.print(rs.getString(2));
			} else {
				out.print("Invalid Email Enterd");
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		// コンテナに次に何をすべきかを伝える整数値を返す
		// 今回のタグではボディを持たないので、アンダースコア・ボディをスキップすることにしている
		return Tag.SKIP_BODY;

	}

	@Override
	public void release() {
		try {
			stmt.close();
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

}

```

*CustomLipDescriptor(TLD)ファイル*
```
<?xml version="1.0" encoding="UTF-8"?>
<taglib>
<tlibversion>2.0</tlibversion>
<jspversion>2.0</jspversion>
<shortname>userinfomation</shortname>
<info>This library displays the user infomation.</info>
<uri>http://bharath.com</uri>
	<tag>
		<name>displayuser</name>
		<tagclass>com.bharath.trainings.jsp.customtags.ResultHandler</tagclass>
		<info>Displays the user infomation</info>
	</tag>
</taglib>
```

*JSP（カスタムタグを使用して情報を表示する）*
```
<!-- 作成したカスタムタグをbharathという名前で使用する -->
<%@taglib prefix="bharath" uri="http://bharath.com" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Display user info</title>
</head>
<body>
<!-- http://bharath.comのdisplayuserを使用する -->
<bharath:displayuser/>
</body>
</html>
```
