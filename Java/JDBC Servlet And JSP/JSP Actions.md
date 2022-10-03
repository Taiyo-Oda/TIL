# JSP Actions
JSPタグにはあらかじめ定義されたアクションタグとカスタムタグの２種類がある。これらは、実行時にjspコンテナに対して指示を行う。

## アクションタグ
* include
* forward
* param
* usebean
* setProperty
* getProperty

## アクションタグを使用した処理の実例
*HTML*

クライアントで入力された値をパラメータとして、displayDetails.jspに引き渡す
```
<html>
 <body>
  	<form action="displayDetails.jsp" method="post">
			Product Id: <input type="text" name="id"/><br/>
			Product Name: <input type="text" name="name"/><br/>
			Product Description: <input type="text" name="description"/><br/>
			Product price:  <input type="text" name="price"/><br/>
			<input type="submit"/>
		</form>
 </body>
</html>
```

*JavaBean*

リクエストパラメータを保管するBean
```
package com.bharath.trainings.jsp;

public class Product {

	private int id;
	private String name;
	private String description;
	private float price;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public float getPrice() {
		return price;
	}

	public void setPrice(float price) {
		this.price = price;
	}

}

```

*カスタムタグを使用したjsp*

<jsp:useBean>でBeanとして使用するJavaファイルを指定している。<br>
<jsp:setProperty>でクライアントからのリクエストパラメータをBeanにセットする<br>
<jsp:getProperty>でBeanにセットした値を取得して、画面に出力している
```
<!-- JSPページ内でJavaBeanを使用する -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Product Ditails</title>
</head>
<body>
	<!-- Product.javaをBeanとして使用する。 -->
	<jsp:useBean id="product" class="com.bharath.trainings.jsp.Product">
		<jsp:setProperty name="product" property="*" />
	</jsp:useBean>
	
	<!-- Beanにセットした値を取得して、出力を行う -->
	Product Ditails
	<br />Id:<jsp:getProperty property="id" name="product"/>
	<br />Name:<jsp:getProperty property="name" name="product"/>
	<br />Description:<jsp:getProperty property="description" name="product"/>
	<br />Price:<jsp:getProperty property="price" name="product"/>

</body>
</html>
```
