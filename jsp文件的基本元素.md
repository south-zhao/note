[TOC]

## jsp文件的基本元素

1. 一般情况下分为五类：
   1. 注释
   2. 模板元素 ==》jsp的静态html或xml内容，其就是网页的框架
   3. 脚本元素 
      1. 声明（Declaration）==》就是声明合法的变量和方法，定义变量和函数
      2. 表达式（Expression）==》<%= 代码 %> 就是一个输出结果
      3. Scriptlets==》<% 代码 %> 里面是一些面向过程的java代码
   4. 指令元素（<%@ 代码 %>）
      1. 页面指令（page）
      2. include指令
      3. taglib指令
   5. 动作元素==>jsp规范定义了一系列的标准动作，用jsp作为前缀

### 指令元素

#### 页面指令

1. 用来定义jsp文件的全局属性

2. 一个jsp页面可以有很多个页面指令，出来import以外，其他的页面指令定义的属性、值只能出现一次

3. 格式：`<%@ page attribute="value"...%>`
   
   1. attribute=language|import|contentType|extends|pageEncoding|
      
      ![1669888481635](C:\Users\south\AppData\Roaming\Typora\typora-user-images\1669888481635.png)
      
      ![1669888508831](C:\Users\south\AppData\Roaming\Typora\typora-user-images\1669888508831.png)

#### include指令

1. 通知容器将当前jsp页面中内嵌的，在指定位置上的资源内容包含，被包含的内容可以被jsp解析
2. 代码:`<%@ include file=-"filename" %>`
3. 简单来说就是将其他的内容显示到你现在的这个文件里面，可以是jsp，html，文本文件等等

#### taglib指令

1. 允许使用者自定义标签，用来引用标签库并设置前缀，为标签库编写.tld配置文件
2. 代码:`<%@ taglib uri="标签库的URI" prefix="自定义标签前缀" %>`
3. uri标签文件或标签库的存放位置，prefix指定该标签库使用的前缀

### 脚本元素

#### 声明

1. 是一段java代码，可以用来定义在产生的类文件中的类的属性和方法，声明的变量和方法可以在jsp的任意地方使用，可以声明方法，也可以声明变量
2. 代码格式:`<%! Variable declaration method declaration(paramType param,...)%>`

#### 表达式

1. jsp请求处理阶段计算它的值，将结果转换为字符串并与模板数据组合在一起
2. 格式:`<%= Some java expression %>`

```
<%= "Hello World"%>
out.println("Hello World")
```

#### Scriptlets

1. jsp页面中处理请求时执行的Java代码
2. 格式:`<% java code statements %>`

### 动作元素

#### <jsp:param>

通过”名-值“的形式为其他标签提供附加信息

`<jsp:param name="paramName" value="paramValue"/>`

#### <jsp:include>

允许在现成的jsp页面里加载包含静态或动态的资源 

`<jsp:include page="fileName"/>`

#### <jsp:forward>

相当于页面的跳转(自动跳转，可用在判断语句中)

`<jsp:forward page="url">`

#### <jsp:setProperty>

#### <jsp:getProperty>

#### <jsp:useBean>

#### <jsp:plugin>

#### <jsp:fallback>

## jsp内建对象

1. 在JSP中，内置对象又称为隐含对象，是指在不声明和不创建的情况下就可以使用的一些成员变量。JSP一共提供有9个内置对象：request（请求对象）、response（响应对象）、pageContext（页面上下文对象）、session（会话对象）、application（应用程序对象）、out（输出对象）、config（配置对象）、page（页面对象）、exception（例外对象）。

### out

![1670030025432](C:\Users\south\AppData\Roaming\Typora\typora-user-images\1670030025432.png)

### request

![1670030266289](C:\Users\south\AppData\Roaming\Typora\typora-user-images\1670030266289.png)

![1670030281458](C:\Users\south\AppData\Roaming\Typora\typora-user-images\1670030281458.png)

### page

jsp实现类对象的一个句柄，只有在jsp页面范围内合法

### exception

运行时的异常，只有在错误页面，在页面指令有isErrorPage=true中才能使用
