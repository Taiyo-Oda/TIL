# JSTL Introduction
ＪＳＴＬ（JavaServer Pages Standard Tag Library）は、ＪＳＰでよく利用される標準的なカスタムタグをまとめたもの

条件付きチェックをしたり、レスポンスから戻ってきたデータをループさせたり、jspページ内でJavaコードを一行も書かずに他のいくつかのことができる

ただし、これはデザインとロジックの分離を意識した設計ではないため、デザインとロジックを分離した設計を目指すならば、JSTLを使ってxmlやDBの操作をjsp上で行うべきではない

※JSTLを使用するためには専用のjarをダウンロードしてlibファイルに配置する必要がある。

## 実例
```
<%@page import="java.util.List"%>
<%@page import="java.util.ArrayList"%>
<!-- coreタグを使用するためのURIを指定（公式リファレンスに記載） -->
<%@taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>C Out Demo</title>
</head>
<body>
<!-- 自作するカスタムタグと異なり、javaファイルやtldファイルを作成する必要がない -->
<c:out value="${10+9}"/>
<c:set var="testScore" value="${80}" scope="session"/>
<c:out value="${testScore}"/>

<c:if test="${testScore>=80}">
	<p>Your score is Awesome : <c:out value="${testScore}"/></p>
</c:if>

<c:choose>
	<c:when test="${testScore>=80}">
		AGrade
	</c:when>
	<c:when test="${testScore>=60 && testScore<80}">
		BGrade
	</c:when>
	<c:otherwise>
		CGrade
	</c:otherwise>
</c:choose>

<c:forEach var="i" begin="1" end="3">
	<c:out value="${i}"></c:out>
</c:forEach>

<%
	List<String> studentNames = new ArrayList<>();
	studentNames.add("Jhon");
	studentNames.add("Bob");
	studentNames.add("Bharath");
	request.setAttribute("studentNames", studentNames);
%>
<c:forEach var="studentName" items="${studentNames}">
	<c:out value="${studentName}"></c:out>
</c:forEach>

<c:remove var="testScore"/>
After Removal the value is : <c:out value="${testScore}"/> 
</body>
</html>
```
