---
typora-copy-images-to: image
---

# Bootstrap框架

## 学习目标

1. 能够创建bootstrap的模板
2. 能够使用boostrap的两种布局容器
3. 能够理解bootstrap的响应式布局的特点
4. 能够查询文档创建bootstrap的按钮、表格、表单等常用组件
5. 能够理解bootstrap的栅格系统
6. 能够查询文档使用bootstrap的导航条
7. 能够查询文档使用bootstrap的轮播图
8. 能够利用Bootstrap完成黑马旅游的首页

# 第1章 boostrap的概述

## 1.1 boostrap的简介

中文官网：[www.bootcss.com]()

![01-bootstrap官网](image/01-bootstrap官网.png)

## 1.2 bootstrap的作用

1. 前端框架，在Web开发中，用于表示层。

2. 基于HTML、CSS、JS、jQuery等技术

## 1.3 Bootstrap的优势

1. 移动设备优先：自 Bootstrap 3 起，框架包含了贯穿于整个库的移动设备优先的样式。

2. 浏览器支持：Bootstrap支持所有的主流浏览器。如：Internet Explorer、 Firefox、 Opera、 Google Chrome、Safari。

   ![02-浏览器](image/02-浏览器.png)

3. 容易上手：只要您具备 HTML 和 CSS 的基础知识，您就可以开始学习 Bootstrap。

4. 响应式设计：Bootstrap 的响应式 CSS 能够自适应于台式机、平板电脑和手机。

## 1.4 响应式设计

### 1.4.1 传统的网页

同一个网站使用的是两个不同的页面

如：https://m.jd.com 手机版京东 https://m.taobao.com  手机版淘宝

淘宝网站在电脑上的效果：

![03-淘宝2](image/03-淘宝2.png)



淘宝网站在手机上的效果：

![03-淘宝](image/03-淘宝.png)

### 1.4.2 响应式设计

在不同的分辨率下网页的宽度进行自动适配

如：索尼 http://www.sony.com  苹果中国： http://www.apple.com.cn 

苹果网站在电脑上的效果：

![03-apple](image/03-apple.png)

苹果网站在手机上的效果：

![03-apple2](image/03-apple2.png)

```
概念：一个网站能够兼容多个终端——而不是为每个终端做一个特定的版本，这个概念是为解决移动互联网浏览而诞生的。响应式布局可以为不同终端的用户提供更加舒适的界面和更好的用户体验。响应式布局基于HTML5和CSS3，才可以实现。
```

# 第2章 boostrap的使用

## 2.1 准备使用Bootstrap

### 2.1.1 Boostrap下载

http://www.bootcss.com，下载用于生产环境的Bootstrap即可

![04-下载](image/04-下载.png)

### 2.1.2 Bootstrap包含的内容

1. 全局CSS：基本的 HTML 元素均可以通过 class 设置样式并得到增强效果；还有先进的栅格系统。

2. 组件：无数可复用的组件，包括字体图标、下拉菜单、导航、警告框、弹出框等更多功能

3. JavaScript 插件：是jQuery插件，带了一些其它的功能。如：轮播图

### 2.1.3 Bootstrap的目录结构

```
 bootstrap/
  ├── css/  全局CSS样式
  │   ├── bootstrap.css    可阅读的完整版本  
  │   ├── bootstrap.min.css  在开发使用这个版本，压缩版，功能与上面的相同
  │   ├── bootstrap-theme.css  
  │   ├── bootstrap-theme.min.css    
  ├── js/ 使用JavaScript插件，就要导入这个JS文件
  │   ├── bootstrap.js    可阅读的完整版本          
  │   └── bootstrap.min.js       在开发使用这个版本，压缩版，功能与上面的相同   
  └── fonts/  字体图标 
      ├── glyphicons-halflings-regular.eot
      ├── glyphicons-halflings-regular.svg
      ├── glyphicons-halflings-regular.ttf
      ├── glyphicons-halflings-regular.woff
      └── glyphicons-halflings-regular.woff2
```

### 2.1.4 目录下的字体图标

![05-字体](image/05-字体.png)

## 2.2 创建Bootstrap模板

### 2.2.1 创建Bootstrap模板文件的步骤

只需要创建一次，以后可以直接复制这个模板使用

1. 复制三个文件夹css、js、fonts到项目目录下
2. 复制jQuery框架到js目录下
3. 复制“起步 ==> 基本模板"的代码到HTML中

### 2.2.2 模板的详细解释

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
  	<!--设置页面的字符集-->
    <meta charset="utf-8">
    <!--强制浏览器按照最新的标准去渲染当前的网页-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
   <!-- 
   	viewport：视口，支持响应式布局。
    width: 初始宽度与设备的宽度相同
    initial-scale：初始的缩放比，1:1
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1. 导入bootstrap中的css样式文件 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
     <!-- 2. 导入jQuery框架 -->
    <script src="js/jquery-3.2.1.min.js"></script>
    <!-- 3. 导入bootstrap的js文件 -->
    <script src="js/bootstrap.min.js"></script>
  </head>
  <body>
    
  </body>
</html>
```



## 2.3 栅格系统

### 2.3.1 组成

​	Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。
	栅格系统用于通过一系列的行（row）与列（column）的组合来创建页面布局，你的内容就可以放入这些创建好的布局中。下面就介绍一下 Bootstrap 栅格系统的特点：

- “行（row）”必须包含在 .container （固定宽度）或 .container-fluid （100% 宽度）中，以便为其赋予合适的排列（aligment）和内间距（padding）。

- 通过“行（row）”在水平方向创建一组“列（column）”。

- 你的内容应当放置于“列（column）”内，并且，只有“列（column）”可以作为行（row）”的直接子元素。

- 栅格系统中的列是通过指定1到12的值来表示其跨越的范围。例如，三个等宽的列可以使用三个 .col-xs-4 来创建。

- 如果一“行（row）”中包含了的“列（column）”大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列。
### 2.3.2 网页布局的两种容器

| **布局的两种容器的类样式名**    | **说明**            |
| ------------------- | ----------------- |
| **container**       | 在不同的分辨率下显示不同的固定宽度 |
| **container-fluid** | 始终以100%的宽度显示页面    |

#### 案例效果：

![06-两种容器](image/06-两种容器.png)

#### 案例代码：

```html
<div class="container" style="border: 1px solid red; height: 200px;">
	container容器：固定大小
</div>
<div class="container-fluid" style="border: 1px solid red; height: 200px;">
	container-fluid：100%宽度
</div>
```

### 2.3.3 媒介查询@media

​	通过不同的媒介类型和条件定义样式表规则。媒介查询让CSS可以更精确作用于不同的媒介(设备)类型和同一媒介的不同条件。媒介查询的大部分媒介特性都接受min和max用于表达“大于或等于”和“小于或等于”。
​	打开文件：bootstrap.css，可以看到以下代码：

```css
 /* 当设备的最小宽度大于或等于768时，容器的宽度是750px */
 @media (min-width: 768px) {
   .container {
     width: 750px;
   }
 }
 /* 当设备的最小宽度大于或等于992时，容器的宽度是970px */
 @media (min-width: 992px) {
   .container {
     width: 970px;
   }
 }
```

所以当我们创建div的类样式名container，则会根据屏幕的大小发生变化

### 2.3.4 基本写法

| **栅格系统**                                 | **描述**                                   | **类似于表格** |
| ---------------------------------------- | ---------------------------------------- | --------- |
| **.container或.container-fluid**          | 整个布局的容器                                  | table     |
| **.row**                                 | 容器中的一行                                   | tr        |
| **.col-xx-n**  **<br />xx有四个取值<br />1)  lg大型设备，如：电视机**  <br />**2)  md 中型设备，如：电脑**  <br />**3)  sm小型设备，如：平板**  <br />**4)  xs微型设备，如：手机**  **<br />n 这一列占几格，一行最多12格** | 表示row中的一列  如：  <br />1)     col-md-4 中型设备上占4列  <br />2)     col-xs-6 微型设备上占6列 | td        |

### 2.3.5 栅格的参数

![15-栅格系统](image/15-栅格系统.png)

### 2.3.6 栅格系统的结构

#### 示例1需求：

​	栅格系统的基本结构。为了让div可见，设置div的边框和高度的样式。

#### 案例效果：

![151-栅格系统-1](image/151-栅格系统-1.png)

#### 案例代码：

```html
<style type="text/css">
    .container div {
        border: 1px solid red;
        height: 200px;
    }
</style>
<!--容器-->
<div class="container">
    <!--一行-->
    <div class="row">
        <!--三列-->
        <div class="col-md-4">
            电脑
        </div>
        <div class="col-md-4">
            电脑
        </div>
        <div class="col-md-4">
            电脑
        </div>
    </div>
    <!--一行-->
    <div class="row">
        <!--三列-->
        <div class="col-md-4">
            电脑
        </div>
        <div class="col-md-4">
            电脑
        </div>
        <div class="col-md-4">
            电脑
        </div>
    </div>
</div>
```

------

#### 示例2的需求：

​	省略row的情况下，在container中直接写6个col-md-3，即每个块占3格，如果超过4个div，则会自动变成2行。

#### 案例效果：

![151-栅格系统-2](image/151-栅格系统-2.png)

#### 案例代码：

```html
<style type="text/css">
    .container div {
        border: 1px solid red;
        height: 200px;
    }
</style>
<!--容器-->
<div class="container">
    <!--三列-->
    <div class="col-md-3">
        电脑
    </div>
    <div class="col-md-3">
        电脑
    </div>
    <div class="col-md-3">
        电脑
    </div>
    <div class="col-md-3">
        电脑
    </div>
    <div class="col-md-3">
        电脑
    </div>
    <div class="col-md-3">
        电脑
    </div>
</div>
```

------

#### 示例3：需求：

​	不同屏幕的适配。每个div设置三个样式，col-md-3 col-sm-4 col-xs-6，格子的数量会随着屏幕尺寸的变化而不同。

#### 案例效果：

![151-栅格系统-3](image/151-栅格系统-3.png)

#### 案例代码：

```html
<style type="text/css">
    .container div {
        border: 1px solid red;
        height: 200px;
    }
</style>
<!--容器-->
<div class="container">
    <!--三列-->
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
</div>
```

------

#### 示例4：需求：

​	显示与隐藏块。不同屏幕尺寸的时候，让某些div块的显示，也可以让某些div隐藏。

#### 案例效果：

![151-栅格系统-4](image/151-栅格系统-4.png)

#### 案例代码：

```html
<style type="text/css">
    .container div {
        border: 1px solid red;
        height: 200px;
    }
</style>
<!--容器-->
<div class="container">
    <!--三列-->
    <div class="col-md-3 col-sm-4 col-xs-6 visible-sm">
        我只在平板上显示
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6 hidden-xs">
        我只在手机上隐藏
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
    <div class="col-md-3 col-sm-4 col-xs-6">
        电脑
    </div>
</div>
```

### 2.3.7 栅格系统中类的小结

| **类样式名**                 | **作用**               |
| ------------------------ | -------------------- |
| **.container**           | 栅格系统容器，类似于table，固定宽度 |
| **.container-fluid**     | 占100%宽度              |
| **.row**                 | 表示一行                 |
| **.col-xs-n**            | 微型设备占n列              |
| **.col-sm-n**            | 小型设备占n列              |
| **.col-md-n**            | 中型设备占n列              |
| **.col-lg-n**            | 大型设备占n列              |
| **.hidden-lg/md/sm/xs**  | 在某种设备上隐藏             |
| **.visible-lg/md/sm/xs** | 只在某种设备上显示            |

## 2.4 全局CSS样式

### 2.4.1 按钮

#### 按钮：普通按钮

\<a>、\<button> 或 \<input> 元素都可以设置成按钮的样式

![7-按钮1](image/07-按钮1.png)

#### 预定义样式的按钮：可以快速创建一个带有预定义样式的按钮

![08-按钮2](image/08-按钮2.png)

#### 案例代码：

```html
<div class="container">
	<h3>按钮</h3>
	<a href="#" class="btn btn-default">链接</a>
	<button class="btn btn-default">Button</button>
	<input type="button" id="" value="普通按钮" class="btn btn-default"/>
	<input type="submit" id="" name="提交按钮" class="btn btn-default"/>
	
	<h3>有颜色的按钮</h3>
	<input type="button" id="" value="普通按钮" class="btn btn-primary"/>
	<input type="button" id="" value="普通按钮" class="btn btn-success"/>
	<input type="button" id="" value="普通按钮" class="btn btn-danger"/>
</div>
```

### 2.4.2 图片

#### 响应式图片

​	为图片添加 **.img-responsive** 类可以让图片支持响应式布局。其实是为图片设置了 max-width: 100%;、 height: auto; 和 display: block; 属性，从而让图片在其父元素中更好的缩放。如果需要让使用了 .img-responsive 类的图片水平居中，请使用 **.center-block** 类

#### 图片形状

​	通过为 `<img>` 元素添加以下相应的类，可以让图片呈现不同的形状。分别是圆角，圆形，相册的效果。其中相册类型的图片也是支持响应式布局的。

```html
<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">
```

![085-图片形状](image/085-图片形状.png)

### 2.4.3 表单

​	所有设置了 **.form-control** 类的 \<input>、\<textarea> 和 \<select> 元素都将被默认设置宽度属性为 width: 100%;。 将 label 元素和前面提到的控件包裹在 **.form-group** 中可以获得最好的排列。

#### 案例需求：

​	使用bootstrap创建如图所示的添加联系人的表单

#### 案例效果：

![11-表单的制作](image/11-表单的制作.png)

#### 案例代码：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>新增联系人</title>
<link href="css/bootstrap.min.css" rel="stylesheet">
<script src="js/jquery-3.2.1.min.js"></script>
<script src="js/bootstrap.min.js"></script>
</head>
<body>
<div class="container" style="max-width: 400px;">
<h3>添加联系人</h3>
<form >
    <div class="form-group">
        <label for="user">姓名：</label>
        <input class="form-control" type="text" id="user" value="" placeholder="请输入姓名"/>
    </div>

    <div class="form-group">
        <label>性别：</label>
        <input type="radio" name="gender" id="male" value="" /> 
      	<label for="male">男</label>
        <input type="radio" name="gender" id="female" value="" /> 
      	<label for="female">女</label>
    </div>

    <div class="form-group">
        <label for="birth">生日：</label>
        <input class="form-control" type="date" name="birth" id="birth" value="" />
    </div>

    <div class="form-group">
        <label for="edu">学历：</label>
        <select class="form-control" name="edu" id="">
            <option value="高中">高中</option>
            <option value="大专">大专</option>
            <option value="本科">本科</option>
        </select>
    </div>

    <div class="text-center">
        <input type="button" value="注册" class="btn btn-primary"/>
        <input type="button" value="取消"  class="btn btn-default"/>
        <input type="button" value="退出"  class="btn btn-danger"/>
    </div>
</form>
</div>
</body>
</html>
```

### 2.4.4 表格

#### 与表格有关的类样式

| **表格的样式**  | **类名**                                   |
| ---------- | ---------------------------------------- |
| **基本实例**   | table                                    |
| **条纹状表格**  | table-striped 类可以给 \<tbody>  之内的每一行增加斑马条纹样式 |
| **带边框的表格** | table-bordered 类为表格和其中的每个单元格增加边框。        |
| **鼠标悬停**   | table-hover 类可以让 \<tbody>  中的每一行对鼠标悬停状态作出响应。 |

#### 表格中的状态类

​	即单独对某个td或tr设置不同的颜色，通过这些状态类可以为行或单元格设置颜色。

| Class    | 描述                 |
| -------- | ------------------ |
| .active  | 鼠标悬停在行或单元格上时所设置的颜色 |
| .success | 标识成功或积极的动作         |
| .info    | 标识普通的提示信息或动作       |
| .warning | 标识警告或需要用户注意        |
| .danger  | 标识危险或潜在的带来负面影响的动作  |

#### 状态类的效果

![091-表格](image/091-表格.png)

```html
<!-- 针对行 -->
<tr class="active">...</tr>
<tr class="success">...</tr>
<tr class="warning">...</tr>
<tr class="danger">...</tr>
<tr class="info">...</tr>

<!-- 针对列 (`td` 或 `th`) -->
<tr>
  <td class="active">...</td>
  <td class="success">...</td>
  <td class="warning">...</td>
  <td class="danger">...</td>
  <td class="info">...</td>
</tr>
```

#### 表格案例：

##### 案例需求：

​	使用bootstrap制作如下所示的表格效果，支持响应式布局

##### 案例效果：

![09-表格](image/09-表格.png)

##### 案例代码：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>表格</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <script src="js/jquery-2.1.0.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
</head>
<body>
<div class="container">
    <h3>表格</h3>
    <table class="table table-striped table-bordered table-hover">
        <tr class="success">
            <td>编号</td>
            <td>姓名</td>
            <td>性别</td>
            <td>电话</td>
        </tr>
        <tr>
            <td>1302</td>
            <td>贝吉塔</td>
            <td>男</td>
            <td>19509869504</td>
        </tr>
        <tr>
            <td>5940</td>
            <td>孙悟空</td>
            <td>男</td>
            <td>13938475687</td>
        </tr>
        <tr>
            <td>6841</td>
            <td>龟仙人</td>
            <td>男</td>
            <td>18501029504</td>
        </tr>
    </table>
</div>
</body>
```

## 2.5 组件

### 2.5.1 导航条

#### 导航条的作用

​	导航条是在您的应用或网站中作为导航页头的响应式基础组件。它们在移动设备上可以折叠（并且可开可关），且在视口（viewport）宽度增加时逐渐变为水平展开模式。

#### 导航条的组成

​	整个导航条就是一个nav标签

| **导航条的类样式**                  | **描述**                 |
| ---------------------------- | ---------------------- |
| **navbar navbar-default**    | 默认导航条的样式               |
| **navbar-header**            | 显示第一项有一个开关，在移动设备上显示得更好 |
| **collapse navbar-collapse** | 用于折叠的菜单项：各种链接，表单，其它内容  |
| **dropdown**                 | 下拉菜单                   |
| **navbar-left**              | 向左对齐，navbar-right向右对齐  |
| **navbar-inverse**           | 反转颜色，变成黑色              |

#### 导航条的案例

##### 案例需求

​	做出如下图所示的导航条

##### 案例效果

![13-导航条](image/13-导航条.png)

##### 案例分析

1. 代码从上到下依次是导航容器
2. 首选项和汉堡按钮
3. 可折叠的菜单，表单或其它的内容

##### 案例代码

```html
<div class="container">
<h3>导航条</h3>
	<nav class="navbar navbar-inverse">
	 <!--nav下有一个100%宽度div-->
	  <div class="container-fluid">
		<!-- 显示第一项有一个开关，在移动设备上显示得更好 -->
		<div class="navbar-header">
			<!--
			所有data开头的属性，不建议删除，表示有事件发生的属性，这里表示按钮可以折叠
			aria 开头属性是给残障人士使用
			-->
		  <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                  data-target="#bs-example-navbar-collapse-1">
             <!-- 使用字体图标 -->
			<span class="glyphicon glyphicon-th-list"></span>
		  </button>
          <!-- 最前面的商标按钮，最大的一个 -->
		  <a class="navbar-brand" href="#">传智播客</a>
		</div>
	
		<!-- 各种链接，表单，其它内容 -->
		<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
		  <ul class="nav navbar-nav">
             <!-- active表示当前选项激活的样式 -->
			<li class="active"><a href="#">JavaEE</a></li>
			<li><a href="#">Android</a></li>
			<!--
				下拉菜单；role属性: 起到标签的说明作用
			-->
			<li class="dropdown">
			  <a href="#" class="dropdown-toggle" data-toggle="dropdown">
                请选择 <span class="caret"></span>
              </a>
			  <ul class="dropdown-menu">
				<li><a href="#">Action</a></li>
				<li><a href="#">Another action</a></li>
				<li><a href="#">Something else here</a></li>
			   <!-- divider菜单中水平线-->
				<li role="separator" class="divider"></li>
				<li><a href="#">Separated link</a></li>
				<li role="separator" class="divider"></li>
				<li><a href="#">One more separated link</a></li>
			  </ul>
			</li>
		  </ul>
		  
		 <!-- 表单：搜索和提交按钮-->
		  <form class="navbar-form navbar-right">
			<div class="form-group">
			  <input type="text" class="form-control" placeholder="搜索关键字">
			</div>
			<button type="submit" class="btn btn-default">
              <span class="glyphicon glyphicon-search">搜索</span>
             </button>
		  </form>
		</div>
	  </div>
	</nav>    	
</div>
```

### 2.5.2 分页组件

#### 分页的作用

为您的网站或应用提供带有展示页码的分页组件

#### 默认分页

##### 分页效果

![131-分页](image/131-分页.png)

##### 分页代码

```html
<div class="container">
<!--分页容器是一个nav-->
<nav>
    <ul class="pagination">
        <li>
            <a href="#">
                <!--向前的双箭头-->
                <span>&laquo;</span>
            </a>
        </li>
        <!--每个li代表其中的一项-->
        <li><a href="#">1</a></li>
        <li><a href="#">2</a></li>
        <li><a href="#">3</a></li>
        <li><a href="#">4</a></li>
        <li><a href="#">5</a></li>
        <li>
            <a href="#">
                <!--向后的双箭头-->
                <span>&raquo;</span>
            </a>
        </li>
    </ul>
</nav>
</div>
```

#### 禁用和激活状态

​	链接在不同情况下可以定制。你可以给不能点击的链接添加 `.disabled` 类、给当前页添加 `.active` 类。下面的按钮中，<<这个按钮不能点击，数字1被激活，数字5不能点。

![132-分页](image/132-分页.png)

##### 案例代码

```html
<nav>
    <ul class="pagination">
        <!--这个按钮不能点-->
        <li class="disabled">
            <a href="#" >
                <span>&laquo;</span>
            </a>
        </li>
        <!--这一项当前被激活-->
        <li class="active"><a href="#">1</a></li>
        <li><a href="#">2</a></li>
        <li><a href="#">3</a></li>
        <li><a href="#">4</a></li>
        <!--这一项看起来是链接，但不能点-->
        <li>
            <span>5</span>
        </li>
        <li>
            <a href="#">
                <!--向后的双箭头-->
                <span>&raquo;</span>
            </a>
        </li>
    </ul>
</nav>
```

## 2.6 javascript插件

​	所有的javascript插件必须要使用到jQuery框架

### 2.6.1 模态框

#### 什么是模态框

​	以弹出对话框的形式出现，出现在其它网页元素之上，在当前对话框没有关闭之前，其它元素不能操作。

#### 对话框的组成

​	包含了模态框的头、体和一组放置于底部的按钮。**默认是不可见的**，点击下面的按钮即可通过 JavaScript 启动一个模态框。

#### 打开对话框的属性

| 按钮上的属性                      | 描述                 |
| --------------------------- | ------------------ |
| data-toggle="modal"         | 打开模态框的功能           |
| data-target="#id值" 或 =".类名" | 指定需要打开的id或类名来打开模态框 |
| data-dismiss="modal"        | 关闭模态框的功能           |

```html
<button class="btn btn-primary" data-toggle="modal" data-target="#myModal">打开模态框</button>
<button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
```
#### 案例需求：

​	点击页面上的蓝色按钮，打开一个模态框。点模态框上的关闭按钮或右上角的x，关闭模态框。

#### 案例效果：

![135-模态框](image/135-模态框.png)

#### 案例代码：

```html
<div class="container">
    <h3>模态框</h3>
    <!--data-toggle="modal"属性表示打开模态框的开关-->
    <!--data-target属性用于指定需要打开的模态框的id-->
    <!--data-dismiss="modal" 表示关闭模态框-->
    <button class="btn btn-primary" data-toggle="modal" data-target="#myModal">
      打开模态框
    </button>
    <div class="modal fade" id="myModal">
        <!--这是一个模态对话框-->
        <div class="modal-dialog">
            <!--模态框的内容开始-->
            <div class="modal-content">
                <!--模态框的头部-->
                <div class="modal-header">
                    <!--点按钮关闭：data-dismiss="modal"-->
                    <button type="button" class="close" data-dismiss="modal">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title">我是标题</h4>
                </div>
                <!--模态框的主体-->
                <div class="modal-body">
                    <p>模态框的内容&hellip;</p>
                </div>
                <!--模态框的脚部-->
                <div class="modal-footer">
                    <!--底部的关闭按钮-->
                    <button type="button" class="btn btn-default" data-dismiss="modal">
                      关闭
                   </button>
                    <!--底部的提交按钮-->
                    <button type="button" class="btn btn-primary">保存</button>
                </div>
            </div><!-- /.modal-content -->
        </div><!-- /.modal-dialog -->
    </div><!-- /.modal -->
</div>
```

### 2.6.2 轮播图

​	这个组件用于轮播显示一组图片，类似于旋转木马，页面加载就自动运行，也可以通过点击左右的两个箭头向左或向右翻页。

![14-轮播图](image/14-轮播图.png)

#### 组成的类样式名

| **类样式名字**               | **描述**     |
| ----------------------- | ---------- |
| **carousel slide**      | 轮播图最外面容器   |
| **carousel-indicators** | 指示器：中间的小圆点 |
| **carousel-inner**      | 内部放所有图片的容器 |
| **carousel-caption**    | 图片标题       |
| **carousel-control**    | 下面的控制按钮    |

#### 相关的属性

| 属性名               | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| data-ride="carousel" | 指定当前是一个轮播图，加载时自动运行                         |
| data-interval="2000" | 间隔的时间间隔，单位是毫秒                                   |
| data-target="#myPic" | 指定轮播图div容器的id值                                      |
| data-pause=“hover”   | 表示当鼠标悬停在轮播图的时候，停止轮播。如果设置为"null"则鼠标悬停不影响轮播图 |

#### 案例需求：

​	参考文档，实现上面的轮播图的效果

#### 案例代码：

```html
<!-- data-ride 默认5秒以后滚动, data-interval 指定滚动的时间 -->
<div id="myPic" class="carousel slide" data-ride="carousel" data-interval="2000">
  <!-- 指示器，中间的小圆点 -->
  <ol class="carousel-indicators">
    <li data-target="#myPic" data-slide-to="0" class="active"></li>
    <li data-target="#myPic" data-slide-to="1"></li>
    <li data-target="#myPic" data-slide-to="2"></li>
  </ol>

  <!-- 图片容器 -->
  <div class="carousel-inner" role="listbox">
    <div class="item active">
      <img src="img/01.jpg">
      <div class="carousel-caption">
        iPhone X
      </div>
    </div>

    <div class="item">
      <img src="img/02.jpg">
      <div class="carousel-caption">
        Mac Book Pro
      </div>
    </div>

    <div class="item">
      <img src="img/03.jpg">
      <div class="carousel-caption">
        iWatch
      </div>
    </div>
  </div>

  <!-- 左右按钮 -->
  <a class="left carousel-control" href="#myPic" role="button" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left"></span>
  </a>
  <a class="right carousel-control" href="#myPic" role="button" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right"></span>
  </a>
</div>
```

# 第3章 综合案例：完成黑马旅游的网站首页

## 3.1 案例需求：

​	使用前面学习过的HTML、CSS和Bootstrap制作黑马旅游的首页

## 3.2 案例效果：

![原型](image/原型.png)

## 3.3 案例分析：

### 3.3.1 导入框架

1. 导入bootstrap框架和jQuery框架
2. 创建一个index.css文件在css文件夹，我们写的类样式都放在这个文件中。

```html
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>黑马旅游网</title>
    <!--导入bootstrap框架-->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <link href="css/index.css" rel="stylesheet">
    <script src="js/jquery-3.2.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
</head>
```

### 3.3.2 全局CSS样式

1. 所有元素的内外边距设置成0
2. a:link的颜色为黑色，没有下划线，a:hover颜色为orangered，没有下划线
3. 创建一个类样式名，设置上内边距为10px，用于调整一些行间隔

```css
* {
	margin: 0px;
	padding: 0px;
}

a:link {
	color: black;
	text-decoration: none;
}

a:hover {
	color: orangered;
	text-decoration: none;
}

.padd-top {
	padding-top: 10px;
}
```

### 3.3.3 整体布局

​	头部使用header标签，脚部使用footer标签，头和脚两个的类容器都使用container-fluid占100%，主体部分使用div标签，类容器使用container的固定大小。

```html
<body>
    <!--分成三部分：头、主体、脚部-->
    <!--头和脚本占100%的宽度，主体使用固定大小的容器-->
    <header class="container-fluid">
    </header>
    <div class="container">
    </div>
    <footer class="container-fluid">
    </footer>
</body>
```

### 3.3.4 第一行：做成响应式的图片

```html
<!--第一行，一张响应式的图片-->
<div class="row">
    <img src="images/top_banner.jpg" class="img-responsive"/>
</div>
```

![top_banner](image/top_banner.jpg)

### 3.3.5 第二行：Logo所在行

```
第二行分成三格，分别占4-5-3列
```

![row-2](image/row-2-7276832642.png)

#### Logo

直接在这一格中输入图片即可，设置text-center类，居中对齐

#### 搜索框

.search_input 文本框：宽400，高36，左浮动，边框2，颜色#ffc900，左内边距10，外轮廓outline设置为0

.search_button 点击按钮，是一个链接：左浮动，宽90，高36，背景色#ffc900，行高36，文本居中

```css
/* 搜索框和按钮 */
.search_input {
	width: 400px;
	height: 36px;
	border: 2px solid #ffc900;
	padding-left: 10px;
	float: left;
	outline: 0px;
}

.search-button {
	float: left;
	width: 90px;
	height: 36px;
	background-color: #ffc900;
	line-height: 36px;
	text-align: center;
}
```

#### 电话

直接插入图片即可

```html
<!--第二行分成三格，分别占4-5-3列, text-center表示内容居中，section表示区域，功能与div相同 -->
<div class="row padd-top">
    <!--左边是logo图片-->
    <section class="col-md-4 text-center">
        <img src="images/logo.jpg">
    </section>
    
    <!--中间是一个文本框和搜索按钮-->
    <section class="col-md-5 padd-top">
        <!-- 文本框与按钮在同一行 -->
        <input type="text" placeholder="请输入路线名称" class="search_input"/>
        <a href="#" class="search-button"><i class="glyphicon glyphicon-search"></i> 搜 索</a>
    </section>
    <section class="col-md-3">
        <!--右边是电话图片和文字-->
        <img src="images/hotel_tel.png">
    </section>
</div>
```

### 3.3.6 导航条

使用bootstrap的组件完成导航条的功能，只保留中间的ul-li即可，其它项目都删除。

将底部的外边距设置为0。

![17-nav](image/17-nav.png)

```html
<!--第三行：导航条，使用bootstrap的组件 -->
<nav class="navbar navbar-default" style="margin-bottom: 0">
    <div class="container-fluid">
        <!-- 导航条上的各项-->
        <div class="collapse navbar-collapse">
            <ul class="nav navbar-nav">
                <li class="active"><a href="">首页</a></li>
                <li><a href="javascript:;">门票</a></li>
                <li><a href="javascript:;">酒店</a></li>
                <li><a href="javascript:;">机票+酒店</a></li>
                <li><a href="javascript:;">香港车票</a></li>
                <li><a href="javascript:;">出境游</a></li>
                <li><a href="javascript:;">国内游</a></li>
                <li><a href="javascript:;">港澳游</a></li>
                <li><a href="javascript:;">抱团定制</a></li>
                <li><a href="javascript:;">全球自由行</a></li>
                <li><a href="javascript:;">旅游攻略</a></li>
                <li><a href="javascript:;">租车</a></li>
                <li><a href="javascript:;">国际电话卡</a></li>
                <li><a href="javascript:;">商业保险</a></li>
                <li><a href="javascript:;">本地导游</a></li>
            </ul>
        </div>
    </div>
</nav>
```

### 3.3.7 轮播图

在header中，使用轮播图，设置轮播时间为2000

![150-轮播图](image/150-轮播图.png)

```html
<div class="row">
    <!-- 轮播图由三部分组成：指示器，图片，左右点击的按钮 -->
    <div id="carousel-example-generic" class="carousel slide" data-ride="carousel" data-interval="2000">
        <!-- 指示器，下面的白色小圆点 -->
        <ol class="carousel-indicators">
            <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
            <li data-target="#carousel-example-generic" data-slide-to="1"></li>
            <li data-target="#carousel-example-generic" data-slide-to="2"></li>
        </ol>

        <!--轮播的图片 -->
        <div class="carousel-inner" role="listbox">
            <div class="item active">
                <img src="images/banner_1.jpg">
            </div>
            <div class="item">
                <img src="images/banner_2.jpg">
            </div>
            <div class="item">
                <img src="images/banner_3.jpg">
            </div>
        </div>

        <!-- 控制按钮 -->
        <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
            <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
        </a>
        <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
            <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
        </a>
    </div>
</div>
```

### 3.3.8 黑马精选

创建一个自定义的样式jx_top，用于row内部的div样式

外上边距15px, 底部边框2px 实线 #ffc900做为下面的线，宽度100%，高度35px，行高35px，外下边距5px

图标文件：icon_5.jpg

![152-icon](image/152-icon.png)

```css
/* 下面的小标题和图标 */
.jx_top {
   margin-top: 15px;
   border-bottom: 2px solid #ffc900;  /* 下面的线*/
   width: 100%;
   height: 35px;
   line-height: 35px;
   margin-bottom: 5px;
}
```
```html
<!--jx_top是自定义的类-->
<div class="row">
    <div class="jx_top">
        <img src="images/icon_5.jpg">
        <span>黑马精选</span>
    </div>
</div>
```

### 3.3.9 黑马精选缩略图

使用bootstrap的组件==> 缩略图 ==> 自定义内容

每个格子占3列。使用p标签加文字，使用span标签加价格，文字使用红色，一共4格。

![160-黑马精选图](image/160-黑马精选图.png)

```html
<!--使用组件 ==> 缩略图-->
<div class="row">
    <div class="col-md-3">
        <div class="thumbnail">
            <!--图片和文字-->
            <img src="images/jiangxuan_4.jpg">
            <div class="caption">
                <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                <span style="color: red">网付价: ￥899</span>
            </div>
        </div>
    </div>
</div>
```

### 3.3.10 国内游

与上面的黑马精选相同，只是图标和文字换了，图标换成icon_6.jpg

![161-国内游图标](image/161-国内游图标.png)

```html
<!--国内游标题-->
<div class="row">
    <div class="jx_top">
        <img src="images/icon_6.jpg">
        <span>国内游</span>
    </div>
</div>
```

### 3.3.11 国内游缩略图

每成2部分，左边4列，右边8列。左边使用图片guonei_1.jpg，右边每个小格子占4列，一共6个缩略图。

![国内游](image/国内游.png)

```html
<!--国内游缩略图-->
<!--每成2部分，左边4列，右边8列-->
<div class="row">
	<div class="col-md-4">
		<img src="images/guonei_1.jpg">
	</div>

	<div class="col-md-8">
		<!--每个缩略图占4格-->
		<div class="col-md-4">
             <!-- 这里只写了一个缩略图，其它的复制即可 -->
			<div class="thumbnail">
				<!--图片和文字-->
				<img src="images/jiangxuan_4.jpg">
				<div class="caption">
				<p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
				<span style="color: red">网付价: ￥899</span>
				</div>
			</div>
          
		</div>
	</div>
</div>
```

### 3.3.12 境外游

与上面的国内游的制作方式相同，只是换一下图标和图片即可。左边的图片文件名：jiangwai_1.jpg

![境外游](image/境外游.png)

### 3.3.13 脚部位置

使用container-fluid容器

第一行：使用一张图片footer_service.jpg，并设置类名 class="img-responsive"，响应式图片

第二行：自定义样式，写一行文字。样式名company：字体大小12，背景色#ffc900，文字居中对齐，高度与行高为40px

![底部](image/底部.png)

## 3.4 案例代码：

### 3.4.1完整的html

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>黑马旅游网</title>
    <!--导入bootstrap框架-->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <link href="css/index.css" rel="stylesheet">
    <script src="js/jquery-3.2.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
</head>
<body>
<!--分成三部分：头、主体、脚部-->
<!--头和脚本占100%的宽度，主体使用固定大小的容器-->
<header class="container-fluid">
    <!--第一行，一张响应式的图片-->
    <div class="row">
        <img src="images/top_banner.jpg" class="img-responsive"/>
    </div>
    <!--第二行分成三格，分别占4-5-3列, text-center表示内容居中，section表示区域，功能与div相同 -->
    <div class="row padd-top">
        <!--左边是logo图片-->
        <section class="col-md-4 text-center">
            <img src="images/logo.jpg">
        </section>
        <!--中间是一个文本框和搜索按钮-->
        <section class="col-md-5 padd-top">
            <!-- 文本框与按钮在同一行 -->
            <input type="text" placeholder="请输入路线名称" class="search_input"/>
            <a href="#" class="search-button"><i class="glyphicon glyphicon-search"></i> 搜 索</a>
        </section>
        <section class="col-md-3">
            <!--右边是电话图片和文字-->
            <img src="images/hotel_tel.png">
        </section>
    </div>
    <!--第三行：导航条，使用bootstrap的组件 -->
    <nav class="navbar navbar-default" style="margin-bottom: 0;">
        <div class="container-fluid">
            <!-- 导航条上的各项-->
            <div class="collapse navbar-collapse">
                <ul class="nav navbar-nav">
                    <li class="active"><a href="">首页</a></li>
                    <li><a href="#">门票</a></li>
                    <li><a href="#">酒店</a></li>
                    <li><a href="#">机票+酒店</a></li>
                    <li><a href="#">香港车票</a></li>
                    <li><a href="#">出境游</a></li>
                    <li><a href="#">国内游</a></li>
                    <li><a href="#">港澳游</a></li>
                    <li><a href="#">抱团定制</a></li>
                    <li><a href="#">全球自由行</a></li>
                    <li><a href="#">旅游攻略</a></li>
                    <li><a href="#">租车</a></li>
                    <li><a href="#">国际电话卡</a></li>
                    <li><a href="#">商业保险</a></li>
                    <li><a href="#">本地导游</a></li>
                </ul>
            </div>
        </div>
    </nav>

    <!--第四行：轮播图-->
    <div class="row">
        <!-- 轮播图由三部分组成：指示器，图片，左右点击的按钮 -->
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel" data-interval="2000">
            <!-- 指示器，下面的白色小圆点 -->
            <ol class="carousel-indicators">
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!--轮播的图片 -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <img src="images/banner_1.jpg">
                </div>
                <div class="item">
                    <img src="images/banner_2.jpg">
                </div>
                <div class="item">
                    <img src="images/banner_3.jpg">
                </div>
            </div>

            <!-- 控制按钮 -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
            </a>
        </div>
    </div>
</header>
<!--主体部分-->
<div class="container">
    <!-- 黑马精选标题 -->
    <!--jx_top是自定义的类-->
    <div class="row">
        <div class="jx_top">
            <img src="images/icon_5.jpg">
            <span>黑马精选</span>
        </div>
    </div>

    <!-- 黑马精选缩略图 -->
    <!--使用组件 ==> 缩略图-->
    <div class="row">
        <div class="col-md-3">
            <div class="thumbnail">
                <!--图片和文字-->
                <img src="images/jiangxuan_1.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                    <span style="color: red">网付价: ￥899</span>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <!--图片和文字-->
                <img src="images/jiangxuan_1.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                    <span style="color: red">网付价: ￥899</span>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <!--图片和文字-->
                <img src="images/jiangxuan_1.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                    <span style="color: red">网付价: ￥899</span>
                </div>
            </div>
        </div>
        <div class="col-md-3">
            <div class="thumbnail">
                <!--图片和文字-->
                <img src="images/jiangxuan_1.jpg">
                <div class="caption">
                    <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                    <span style="color: red">网付价: ￥899</span>
                </div>
            </div>
        </div>
    </div>

    <!--国内游标题-->
    <div class="row">
        <div class="jx_top">
            <img src="images/icon_6.jpg">
            <span>国内游</span>
        </div>
    </div>

    <!--国内游缩略图-->
    <!--每成2部分，左边4列，右边8列-->
    <div class="row">
        <div class="col-md-4">
            <img src="images/guonei_1.jpg">
        </div>

        <div class="col-md-8">
            <!--每个缩略图占4格-->
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_4.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_4.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_4.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_4.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_4.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_4.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!--境外游，与上面的相同，换图片和文字即可-->
    <div class="row">
        <div class="jx_top">
            <img src="images/icon_7.jpg">
            <span>境外游</span>
        </div>
    </div>

    <!--境外游缩略图-->
    <!--每成2部分，左边4列，右边8列-->
    <div class="row">
        <div class="col-md-4">
            <img src="images/jiangwai_1.jpg"/>
        </div>

        <div class="col-md-8">
            <!--每个缩略图占4格-->
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_3.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_3.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_3.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_3.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_3.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="thumbnail">
                    <!--图片和文字-->
                    <img src="images/jiangxuan_3.jpg">
                    <div class="caption">
                        <p>上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                        <span style="color: red">网付价: ￥899</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<footer class="container-fluid">
    <!--直接指定为一张图片-->
    <div class="row">
        <img src="images/footer_service.png" class="img-responsive">
    </div>
    <!--company是自定义样式-->
    <div class="row company">
        江苏传智播客教育科技股份有限公司 版权所有Copyright 2006-2018, All Rights Reserved 苏ICP备16007882
    </div>
</footer>
</body>
</html>
```

### 3.4.2完整的index.css

```css
* {
	margin: 0px;
	padding: 0px;
}

a:link {
	color: black;
	text-decoration: none;
}

a:hover {
	color: orangered;
	text-decoration: none;
}

.padd-top {
	padding-top: 10px;
}

/* 搜索框和按钮 */
.search_input {
	width: 400px;
	height: 36px;
	border: 2px solid #ffc900;
	padding-left: 10px;
	float: left;
	outline: 0px;
}

.search-button {
	float: left;
	width: 90px;
	height: 36px;
	background-color: #ffc900;
	line-height: 36px;
	text-align: center;
}

/* 下面的小标题和图标 */
.jx_top {
	margin-top: 15px;
	border-bottom: 2px solid #ffc900;  /* 下面的线*/
	width: 100%;
	height: 35px;
	line-height: 35px;
	margin-bottom: 5px;
}

/*最下面一行的文字*/
.company {
	font-size: 12px;
	background-color: #ffc900;
	text-align: center;
	height: 40px;
	line-height: 40px;
}
```



