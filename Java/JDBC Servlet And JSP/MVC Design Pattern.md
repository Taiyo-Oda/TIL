# MVC Design Pattern

MVCはWebレイヤー（＊1）を３つのパートに分割するアーキテクチャパターン（＊2）のこと。「ロジックとデザインの分離」という考え方をもとにしている。
* M → Model
* V → View
* C → Controller

## Model
モデルはアプリケーションの「処理」の部分とそれに関する情報の保持を担当する。一方で、画面に対する入出力といった部分に一切関わりを持たない。

## View
モデルによる処理結果の画面への表示を担当する。モデルとは逆で、あくまで画面への表示がその役割であり、情報の処理には一切関わらない。

## Controller
画面からの情報の入力と、モデルの呼び出し及びその結果に従ったビューの呼び出しを担当する。

## MVCパターンのメリット
View（表示）とModel（処理）を分けて開発を行えるため、メンテナンスが行いやすい。<br>
また、UIとビジネスロジックを並行して開発ができるようになる。

## MVCパターンの実例
*View（入力）*
```
<html>
<body>
<h3>Enter two number:</h3>
	<form  action="AverageController" method="post">
		Number 1 : <input name="number1"/><br/>
		Number 2: <input name="number2"/><br/>
		<input type="submit"/>
	</form>
</body>
</html>
```

*Controller*
```
package com.bharath.trainings.servlets.mvc.controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.bharath.trainings.servlets.mvc.model.AverageCaluculator;

/**
 * Servlet implementation class AverageController
 */
@WebServlet("/AverageController")
public class AverageController extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		int num1 = Integer.parseInt(request.getParameter("number1"));
		int num2 = Integer.parseInt(request.getParameter("number2"));
		
		// モデルを使用するためのインスタンスを作成
		AverageCaluculator model = new AverageCaluculator();
		int result = model.calculate(num1, num2);
		// リクエストの属性を保存する
		request.setAttribute("result", result);
		
		// 与えられたパスにあるリソースのラッパーとして機能するRequestDispatcherオブジェクトを返す
		RequestDispatcher dispatcher = request.getRequestDispatcher("result.jsp");
		// サーブレットからのリクエストをサーバー上の別のリソース(サーブレット、JSPファイル、HTMLファイル)に転送する。
		// getRequestDispatcher() で取得した RequestDispatcher では、ServletRequest オブジェクトの path 要素とパラメータはターゲットリソースのパスに合うように調整される。
		dispatcher.forward(request, response);
	}

}
```

*Model*
```
package com.bharath.trainings.servlets.mvc.model;

public class AverageCaluculator {

	public int calculate(int num1, int num2) {
		return (num1 + num2) / 2;
	}
}

```


*View（出力）*
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Result</title>
</head>
<body>
	
	<%
		int result = (Integer) request.getAttribute("result");
		out.print("<b> The Average is : " + result + "</b>");
	%>

</body>
</html>
```


*1 「Layers」と呼ばれるアーキテクチャパターンの一つ。レイヤとは階層のことだが、Layersパターンはシステムを階層化し、上位レイヤが下位レイヤの提供する機能を利用することで各レイヤの作りを単純化していく考えかたである。

*2　ソフトウェアの設計スタイルとその設計に基づく全体構造のことを、「アーキテクチャ」と言い、それをいくつかのパターンに体系化したものを「アーキテクチャパターン」という。
