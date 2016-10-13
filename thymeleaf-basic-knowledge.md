# Thymeleaf基本知识

[Thymeleaf](http://www.thymeleaf.org/)是一款用于渲染XML、XHTML、HTML5内容的模板引擎。 它也可以轻易 的与Spring MVC等Web框架进行集成作为Web应用的模板引擎， hymeleaf最大的 特点是通过HTML的标签属性渲染标签内容 。

Thymeleaf在web界面引用是需加入引用链接：

> &lt;html xmlns="http:\/\/www.w3.org\/1999\/xhtml" xmlns:th="http:\/\/www .thymeleaf.org"&gt;&lt;\/html&gt;

## 1.Thymeleaf表达式语句

* \#{...}：用户获取国际化配置文件内容 
* ${...}：变量表达式，用户获取对象的值 
* \*{...}：属性选择表达式，一般与th:with配合使用，用户简化表达式内容 
* @{...}：链接表达式，由thymeleaf模板引擎负责解析应用的ContextPath

## 2.常用基本标签

1. th:text标签：用来输出显示文本内容，例如获取用户的姓名：&lt;p th:text="${user.name}"&gt;&lt;\\/p&gt; 
2. th:value标签：用来显示文本框的值，例如&lt;input name="username" th:value="${user.name}" \\/&gt; 
3. th:href标签：用来处理url，支持绝对路径和相对路径，例如访问用户详情的 url： 
  &lt;a th:href="@{\/users\/{id}\/show\(id=${user.id}\)}"&gt;&lt;\/a&gt;。

* i. 注意：@{\\/users}是Context的相对路径，thymeleaf在渲染时会自动添加 当前web应用的context名称。若context名称为app，则路径为 \\/app\\/users。 4. th:each标签：循环标签，用来列表数据进行循环取值。例如列表页对用户信息 进行遍历取值：

