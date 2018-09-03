# HTML5讲义

## 学习目标

1. 能够使用idea创建html文档

2. 能够使用h1~h6、hr、p、br 等与文本有关的标签

3. 能够使用有序列表ul-li和无序列表ol-li显示列表内容

4. 能够使用块标签div与内联标签span

5. 能够使用图片img标签把图片显示在页面中

6. 能够使用超链接a标签跳转到一个新页面

7. 能够使用table、tr、td标签定义表格

8. **能够制作黑马旅游网的首页**

## 案例介绍

​	通过今天的学习要做出如下网页的：黑马旅游网的首页，因为今天学习的内容有限，随着后期知识点的深入，我们还会进一步完善这个网页。

![首页原型](image/首页原型.png)


# 第1章 HTML概述

## 1.1 HTML概念

### 1.1.1 什么是HTML

​	HTML：Hyper Text Markup Language，中文是超文本标记语言。

​	它通过标记符号来标记要显示的网页中的各个部分。网页文件本身是一种文本文件，通过在文本文件中添加标记符，可以告诉浏览器如何显示其中的内容（如：文字如何处理，画面如何安排，图片如何显示等）。浏览器按顺序阅读网页文件，然后根据标记符解释和显示其标记的内容，对书写出错的标记将不指出其错误，且不停止其解释执行过程。

1、 超文本：网页文件本身是一个文本文件，而超文本指的是网页中包含的图片、超链接、音频、视频等内容超出了文本的范畴。

2、 标记语言：一种将文本以特殊的标签进行标识的语言。使用标记语言编写的文件中都是一些纯文本，而文本文件本身不具备行为能力，只是用来被读取的，例如xml、html、xhtml(xml与html的结合物)等，这些都属于标记语言。而编程语言是指，语言中包含了很多if else for print等具有逻辑性和行为能力的指令。例如C、Java、PHP等。

### 1.1.2 HTML5

​	2014年10月29日，万维网联盟(W3C)泪流满面地宣布，经过几乎8年的艰辛努力，HTML5标准规范终于最终制定完成了，并已公开发布，这是一次重大的革新。
	HTML5将会取代1999年制定的HTML 4.01、XHTML 1.0标准，以期能在互联网应用迅速发展的时候，使网络标准达到符合当代的网络需求，为桌面和移动平台带来无缝衔接的丰富内容。
	目前支持Html5的浏览器包括Firefox（火狐浏览器），IE9及其更高版本，Chrome（谷歌浏览器），Safari，Opera等。

![02-浏览器](image/02-浏览器.png)

## 1.2 开发HTML

### 1.2.1 使用记事本创建html

​	HTML是一个文本文件，使用记事本就可以开发。

```html
<html>
	<head>
		<title>这是标题</title>
	</head>
	<body>
		这里是正文
	</body>
</html>
```
### 1.2.2 以下是HTML的基本结构

| **标签名**        | **作用**                                   |
| -------------- | ---------------------------------------- |
| **html**       | 根元素是html标签                               |
| **head**       | 网页的头部                                    |
| **body**       | 网页的主体，网页的标签主要写在主体中  bgcolor属性：设置网页背景色    |
| **\<!--注释-->** | 网页中的说明性文字，功能与Java中的注释一样，可以同时用于单行和多行注释<br />浏览器查看时，不显示，右键查看源码可以看到。<br />注释标签不能嵌套。 |

### 1.2.3 使用IntelliJ IDEA创建html

​	使用“记事本”开发效率低，现阶段使用idea做为开发工具，开发步骤如下：

1. 创建静态Web工程

![HeiMa_094242](image/HeiMa_094242.png)

2. 指定工程名和保存位置

   ![HeiMa_094325](image/HeiMa_094325.png)

3. 创建HTML文件，选择html5的版本

   ![HeiMa_095010](image/HeiMa_095010.png)

4. 创建HTML文件

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>我是第一个网页</title>
   </head>
   <body>
       世界，你好！
   </body>
   </html>
   ```

5. 点右上角一排浏览器按钮运行，idea会使用内置的服务器在指定的浏览器上运行

![](image/HM_0131_095622.png)

6. 在浏览器上运行的结果

   ![](image/HM_0131_095901.png)

### 1.2.4 项目结构：

​	一个网页项目建议按如下目录创建结构

![项目结构](image/项目结构.png)

# 第2章 HTML的标签

## 2.1 HTML标签的分类(了解)

### 2.1.1 按是否有主体分

| **有否有主体**  | **格式**                          |
| ---------- | ------------------------------- |
| **有主体标签**  | 标签内部有文本或其它的标签，如：  \<h1>内容\</h1> |
| **没有主体标签** | 没有主体内容，如：\<br/>                 |

### 2.1.2 按是否换行

| **标签分类** | **特点**                                   |
| -------- | ---------------------------------------- |
| **块标签**  | 块级元素在浏览器显示时，通常会以新行来开始（和结束）。<br />每个标签单独占一行，自己换行。例子：\<h1>, \<p>, \<ul>, \<table> |
| **内联标签** | 标签本身没有换行的功能，下一个标签出现在上一个标签的后面。例子：\<b>, \<td>, \<a>, \<img> |

## 2.2 语义化标签(了解)

### 2.2.1 概念

​	语义化就是你看到某个标签就知道它是干什么的，见名知义。语义化的主要优点如下：

1. 提升浏览器对网页的可访问性；

2. 有利于百度等搜索引擎的优化；

3. 对于开发者来说，结构更加清晰，利于文档的维护；

### 2.2.2 常用的语义化标签

| 语义化标签 | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| \<head>    | 用于网页的页眉，页眉通常用于设置网站标志、主导航、全站链接以及搜索框。 |
| \<nav>     | 用于页面的导航，仅对文档中重要的链接群使用。                 |
| \<main>    | 页面主要内容，一个页面只能使用一次。                         |
| \<section> | 具有相似主题的一组内容，比如网站的主页可以分成介绍、新闻条目、联系信息等条块。 |
| \<footer>  | 用于网页的页脚，只有当父级是body时，才是整个页面的页脚。     |

## 2.3 文本标签

### 2.3.1 文本的标签介绍

| **标签名**   | **标签描述**                                 | **常用属性**                                 |
| --------- | ---------------------------------------- | ---------------------------------------- |
| **h1~h6** | 用于创建标题。一共有6级标题，1级标题最大，6级标题最小，标题标签默认字体加粗。 | align：标题对齐的方式  取值：center居中、right 右对齐、left 左对齐 |
| **hr**    | 绘制一条水平线，没有主体的标签。                         | width: 宽度  <br />size: 粗细  <br />color: 颜色 |
| **font**  | 设置字体，大小，颜色。如果本地没有这个字体，使用默认的字体            | color：颜色  <br />size：大小  <br />face：字体   |
| **b**     | 对字体加粗标签                                  |                                          |
| **i**     | 设置为斜体字                                   |                                          |
| **br**    | 换行，没有主体的标签                               |                                          |
| **p**     | 代表一个自然段，段落标记  段与段之间有间隔，没有首行缩进。           | title：如果鼠标移动到段落上，会出现提示文字                 |

### 2.3.2 文本标签的代码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>文本标签</title>
</head>
<body>
<h1>第1级标题</h1>
<h2>第2级标题</h2>
<h3 align="center">第3级标题</h3>
<h4 align="right">第4级标题</h4>
<h5>第5级标题</h5>
<h6>第6级标题</h6>
<hr width="300" size="6" color="red"/>
普通文本
<hr/>
<font color="blue" size="6" face="楷体">孙悟空</font><br/>
<font color="blue" size="6" face="楷体"><b>孙悟空</b></font><br/>
<font color="blue" size="6" face="楷体"><b><i>孙悟空</i></b></font><br/>
<hr/>
<p title="我是第一自然段">
    World Wide Web Consortium万维网联盟创建于1994年，是Web技术领域最具权威和影响力的国际中立性技术标准机构。到目前为止，W3C已发布了200多项影响深远的Web技术标准及实施指南。
</p>
</body>
</html>
```
-----

运行效果：

![HM_0131_115424](image/HM_0131_115424.png)

### 2.3.3 案例：制作黑马公司介绍

#### 案例需求：

​	使用已经学习的文本标签，制作如下的公司介绍的页面。

​	*注：因为没有学习CSS样式，本案例中为了实现效果，还是会用到部分已经不推荐使用的HTML4标签，等明天学习完CSS，则那些标签可以不再使用了。*

#### 案例效果：

![HM_0131_110417](image/HM_0131_110417.png)

#### 案例分析：

1. 整个页面使用语义标签进行布局，上面使用header，中间使用main，下面使用footer
2. “公司介绍”，需要使用标题标签完成 。例如：\<h3>
3. 两条橙色的线使用水平线完成
4. “中关村黑马程序员训练营” 需要使用字体标签完成 <font>
5. “传智播客” 需要斜体\<i> 和 粗体\<b> 组合完成
6. 这个文档被划分成4个段落，每一个段落之间有定义的间隔，需要使用段落标签\<p>完成，如果要缩进目前可以使用两个全角空格。后面还有其它实现方式。
7. 第2行与第3行是一个普通的换行，需要使用\<br/>完成
8. 最下面的页脚使用2号字体，灰色，居中。

#### 案例代码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>文本标签</title>
</head>
<body>
<header>
    <h1>公司简介</h1>
    <hr size="2" color="orange"/>
</header>
<main>
    <p>
 　　<font color="red">“中关村黑马程序员训练营”</font>是由<b><i>传智播客</i></b>联合中关村软件园、CSDN，并委托传智播客进行教学实施的软件开发高端培训机构，致力于服务各大软件企业，解决当前软件开发技术飞速发展，而企业招不到优秀人才的困扰。<br/>
目前，“中关村黑马程序员训练营”已成长为行业“学员质量好、课程内容深、企业满意”的移动开发高端训练基地，并被评为中关村软件园重点扶持人才企业。
    </p>
    <p> 　　黑马程序员的学员多为大学毕业后，有理想、有梦想，想从事IT行业，而没有环境和机遇改变自己命运的年轻人。黑马程序员的学员筛选制度，远比现在90%以上的企业招聘流程更为严格。任何一名学员想成功入学“黑马程序员”，必须经历长达2个月的面试流程，这些流程中不仅包括严格的技术测试、自学能力测试，还包括性格测试、压力测试、品德测试等等测试。毫不夸张地说，黑马程序员训练营所有学员都是精挑细选出来的。百里挑一的残酷筛选制度确保学员质量，并降低企业的用人风险。
    </p>
    <p>　　中关村黑马程序员训练营不仅着重培养学员的基础理论知识，更注重培养项目实施管理能力，并密切关注技术革新，不断引入先进的技术，研发更新技术课程，确保学员进入企业后不仅能独立从事开发工作，更能给企业带来新的技术体系和理念。
    </p>
    <p>　一直以来，黑马程序员以技术视角关注IT产业发展，以深度分享推进产业技术成长，致力于弘扬技术创新，倡导分享、开放和协作，努力打造高质量的IT人才服务平台。
    </p>
</main>
<footer>
    <hr size="2" color="orange"/>
    <font size="2" color="gray">
        <center>江苏传智播客教育科技股份有限公司<br/>
        版权所有Copyright &copy; 2006-2018, All Rights Reserved 苏ICP备16007882</center>
    </font>
</footer>
</body>
</html>
```

## 2.4 列表标签

​	列表标签用于显示一列数据，在后期的导航条菜单中也使用比较多。

| **列表标签**  | **属性**                                   | **说明**                      |
| --------- | ---------------------------------------- | --------------------------- |
| **ol-li** | type属性<br />1  默认使用数字序列 <br />a,A  使用字母序列<br />i,I    罗马数字 | 有序列表  ol 代表列表容器  li 列表中的每一项 |
| **ul-li** | type属性 <br />disc   ●  默认  <br />circle ○  <br />square ■ | 无序列表  ul 列表容器  li 列表中每一项    |

#### 案例需求：

​	制作如图所示的菜单列表，左边列是有序列表，右边列是无序列表

#### 案例效果：

![HM_0131_160000](image/HM_0131_160000.png)

#### 案例代码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>列表标签</title>
</head>
<body>
<h3>晚餐菜单</h3>
<ol type="1">
    <li>红烧肉</li>
    <li>番茄炒牛肉</li>
    <li>地沟油薯条</li>
    <li>口水鸡</li>
</ol>
<h3>晚餐菜单</h3>
<ol type="A">
    <li>红烧肉</li>
    <li>番茄炒牛肉</li>
    <li>地沟油薯条</li>
    <li>口水鸡</li>
</ol>
<h3>晚餐菜单</h3>
<ol type="I">
    <li>红烧肉</li>
    <li>番茄炒牛肉</li>
    <li>地沟油薯条</li>
    <li>口水鸡</li>
</ol>
<hr />
<h3>晚餐菜单</h3>
<ul>
    <li>红烧肉</li>
    <li>番茄炒牛肉</li>
    <li>地沟油薯条</li>
    <li>口水鸡</li>
</ul>
<h3>晚餐菜单</h3>
<ul type="circle">
    <li>红烧肉</li>
    <li>番茄炒牛肉</li>
    <li>地沟油薯条</li>
    <li>口水鸡</li>
</ul>
<h3>晚餐菜单</h3>
<ul type="square">
    <li>红烧肉</li>
    <li>番茄炒牛肉</li>
    <li>地沟油薯条</li>
    <li>口水鸡</li>
</ul>
</body>
</html>
```

## 2.5 块标签与内联标签(重要)

### 2.5.1 \<div> 和 \<span>的作用：

​	可以通过\<div> 和 \<span> 将网页在逻辑上分成不同的部分，两个标签本身没有具体的含义。

### 2.5.2 div标签

​	 \<div> 元素是块级元素，浏览器会在其前后显示折行。它是可用于组合其他 HTML 元素的容器。
\<div> 元素是英文division，起到分割的意思。如果与 CSS 一同使用，\<div> 元素可用于对大的内容块设置样式属性。\<div> 元素的一个常见的用途是文档布局。

### 2.5.3 span标签

​	\<span> 元素是内联元素，可用作文本的容器。当与 CSS 一同使用时，\<span> 元素可用于为部分文本设置样式属性。

### 2.5.4 div和span案例：

#### 案例需求：

​	通过代码认识div和span的功能和特点

#### 案例效果：

![HM_0131_162032](image/HM_0131_162032.png)

#### 案例代码：

```html
<span style="color: red; background-color: yellow;">World Wide Web Consortium</span>万维网联盟创建于1994年<br />
<div style="color: red; background-color: yellow;">World Wide Web Consortium</div>万维网联盟创建于1994年
```

## 2.6 实体字符(了解)

​	当页面上需要使用一些特殊符号的时候怎么办呢？可以使用实体字符

![实体字符](image/实体字符.png)

### 2.6.1 实体字符的格式：

​	前面以&符号开头，后面以;分号结尾

### 2.6.2 案例代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>实体字符</title>
</head>
<body>
<body>
<font size="6"> 
    小于 &lt; &nbsp; 大于&gt; 日元&yen; &nbsp; 与符号&amp; &nbsp;
    双引号&quot; &nbsp; 版权&copy; 
</font>&nbsp;
<font color="red" size="6">红心&hearts;</font>&nbsp;
<font color="red" size="6">方块&diamond;</font>&nbsp;
<font size="6">梅花&clubs;</font>
</body>
</body>
</html>
```

### 2.6.3 常用的实体字符表

![实体字符表](image/实体字符表.png)

## 2.7 图像标签(重点)

### 2.7.1 图像标签的作用

​	用于在网页中显示各种图片

### 2.7.2 基本语法

​	

<img src="图片URL"/> 它是无主体的标签

### 2.7.3 常用属性

| **属性名**    | **作用**                  |
| ---------- | ----------------------- |
| **src**    | 图片的地址，也可以是其它网站图片        |
| **width**  | 设置图片的宽度，如果只设置宽度，高度会等比缩放 |
| **height** | 设置图片的高度                 |
| **alt**    | 如果图片丢失，使用这个文字代替         |
| **title**  | 鼠标移上去出现提示信息             |

### 2.7.4 图像使用的案例

#### 案例效果：

-----

![HM_0131_171026](image/HM_0131_171026.png)

#### 案例代码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>图像案例</title>
</head>
<body>
<h3>我是女生</h3>
<img src="img/girl.jpg" width="300"/>
<h3>我是男生</h3>
<img src="img/boy.png" width="300" title="我是男生"/>
<h3>程序猿 </h3>
<img alt="我是猴子" width="300" src="http://www.itcast.cn/files/image/201801/20180102155918729.jpg">
</body>
</html>
```

#### 相对路径和绝对路径

##### 相对路径(很重要)

相对的是当前文件，如果目标文件与当前文件在同一个目录下，那么直接写目标文件的文件名。如果目标文件在当前文件的上一级目录，则先使用"../"找到上一级目录。



##### 绝对路径

就是该文件在服务器上的全路径。



## 2.8 超链接标签(重要)

### 2.8.1 基本作用

	让一个文本可以点击，点击文字以后跳转到其它的网页。可以是同一个服务器中的网页，也可以是其它的服务器。
### 2.8.2 语法

	<a href="跳转地址URL">文字</a>
### 2.8.3 常用属性：

- href ： 要跳转到的URL地址
- title：鼠标移上去以后显示提示信息
- target取值：设置网页打开的方式
  - _self: 默认，在当前的窗口中打开页面
  - _blank: 在新的窗口或标签页面中打开页面

### 2.8.4 调用发邮件客户端(了解) 

#### 作用：

​	打开发邮件的客户端，给指定邮箱发邮件。

#### 语法：

	<a href="mailto:邮箱地址">给我发邮件</a>
### 2.8.5 案例代码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>a标签</title>
</head>
<body>
<!--本项目资源(就是在本网站内进行跳转)-->
<a href="liqinzhao.html" target="_blank">李清照</a>
<!--跳转到其它项目资源-->
<a href="http://www.itcast.cn" title="跳转传智官网">传智播客</a>
<a href="mailto: zhangsan@163.com">给张三发邮件</a>
</body>
</html>
```

## 2.9 表格标签(重要)

### 2.9.1 表格的作用

1. 用于显示大量的数据
2. 可以进行网页布局，但目前这种方式使用比较少(现在一般都是使用div进行布局)


### 2.9.2 表格的结构标签

| **标签名**  | **作用**                                                     |
| ----------- | ------------------------------------------------------------ |
| **table**   | 表格容器                                                     |
| **tr**      | 一行                                                         |
| **th**      | 表格的列标题：加粗，居中                                     |
| **td**      | 一列                                                         |
| **caption** | 表格的标题                                                   |
| **thead**   | 不显示任何的内容，在逻辑上将表格分成不同的部分。表示表格的头部 |
| **tbody**   | 表示表格的主体，无论我们的代码中有没有tbody，在浏览器中都是存在的 |
| **tfoot**   | 表示表格的脚部                                               |

### 2.9.3 常用的表格属性

| **属性名**         | **作用**          |
| --------------- | --------------- |
| **width**       | 表格宽度            |
| **border**      | 表格外边框粗细         |
| **align**       | 可以用在td，tr，table |
| **rowspan**     | 跨几行             |
| **colspan**     | 跨几列             |
| **cellspacing** | 设置单元格之间的间隔      |
| **cellpadding** | 设置单元格边框与文字之间距离  |

### 2.9.4 没有边框的表格

#### 案例效果：

![没有边框的表格](image/没有边框的表格.png)

#### 案例代码：

```html
<h3>这个表格没有边框</h3>
<!--table表示表格的容器-->
<table>
	<!--tr表示一行-->
	<tr>
		<!--表示一列-->
		<td>100</td>
		<td>200</td>
		<td>300</td>
	</tr>
	<tr>
		<!--表示一列-->
		<td>400</td>
		<td>500</td>
		<td>600</td>
	</tr>
</table>
```

### 2.9.5 有表头的表格

#### 案例效果：

![有表头的表格](image/有表头的表格.png)

#### 案例代码：

```html
<!-- 指定边框的粗细，指定表格的宽度 -->
<table border="2" width="500">
	<tr>
		<!-- 表头 -->
		<th>姓名</th>
		<th>电话</th>
		<th>电话</th>
	</tr>
	<tr>
		<td>Bill Gates</td>
		<td>2934-729374</td>
		<td>09348234-23</td>
	</tr>
</table>
```

### 2.9.6 跨列的表格

#### 案例效果

![HM_0131_175454](image/HM_0131_175454.png)

#### 案例代码

```html
<h3>有标题的表格</h3>
<table border="5" width="300">
    <!--表格的标题-->
    <caption>我是标题</caption>
    <tr>
        <td>100</td>
        <td>200</td>
        <td>300</td>
    </tr>
    <tr>
        <td>400</td>
        <td>500</td>
        <td>600</td>
    </tr>
    <tr>
        <!--跨3列-->
        <td colspan="3">
            999
        </td>
    </tr>
</table>
```

### 2.9.7 案例：学生成绩表

#### 案例需求：

​	制作如图所示的学生成绩表

#### 案例效果：

![HM_0131_180104](image/HM_0131_180104.png)

#### 案例分析：

1. 表格有标题，使用caption标签
2. 使用thead、tbody、tfoot对表格进行分块
3. 第一行有表格列标题，使用th标签
4. 表格的每一行都居中，使用tr标签的align属性居中对齐
5. 成绩和总成绩分别有跨行和跨列的要求
6. 使用属性要设置表格之间的间隔
7. 使用属性设置文本与表格边框之间的距离

#### 案例代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>学生成绩表</title>
</head>
<body>
<!--边框粗细为2，宽500，网页居中对齐，单元格间隔0，文字与边框间隔8-->
<table border="2" width="500" align="center" cellspacing="0" cellpadding="8">
    <!--标题-->
    <caption>学生成绩表</caption>
    <!--表格头部-->
    <thead>
    <tr>
        <!--表格的列标题-->
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>成绩</th>
    </tr>
    </thead>
    <!--表格主体-->
    <tbody>
    <!--内容居中对齐-->
    <tr align="center">
        <td>100</td>
        <td>潘金莲</td>
        <td>女</td>
        <td>80</td>
    </tr>
    <tr align="center">
        <td>200</td>
        <td>武大狼</td>
        <td>男</td>
        <!--跨2行-->
        <td rowspan="2">90</td>
    </tr>
    <tr align="center">
        <td>300</td>
        <td align="center">红太狼</td>
        <td>女</td>
    </tr>
    </tbody>
    <!--表格脚部-->
    <tfoot>
    <tr align="center">
        <td>总成绩</td>
        <!--跨3列-->
        <td colspan="3">900</td>
    </tr>
    </tfoot>
</table>
</body>
</html>
```



# 第3章 案例：黑马旅游首页

### 3.1 案例需求

​	通过今天学习的HTML知识做出黑马旅游网的首页，因为还有一些知识没有学习，用到个别淘汰的标签，效果上也会有一些出入。

### 3.2 案例效果

![HM_0201_085710](image/HM_0201_085710.png)

### 3.3 案例分析

​	整个网页布局最外面可以做成一个9行1列的表格，然后使用表格嵌套的方式实现。

#### 3.3.1 整个布局

1. 创建最外面的表格，边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%
2. 上面的4行，设置为thead
3. 中间部分设置为tbody
4. 最后2行，设置为tfoot

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>黑马旅游网</title>
</head>
<body>
<table border="0" width="100%" cellpadding="0" cellspacing="0">
    <!-- 头部 -->
    <thead>
    <tr>
        <td></td>
    </tr>
    <tr>
        <td></td>
    </tr>
    <tr>
        <td></td>
    </tr>
    <tr>
        <td></td>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td></td>
    </tr>
  	</tbody>
  	<tfoot>
    <tr>
        <td></td>
    </tr>
    <tr>
        <td></td>
    </tr>
  </tfoot>
</table>
</body>
</html>
```

#### 3.3.2 最上面的横幅图片

##### 案例效果

![top_banner](image/top_banner.jpg)

##### 案例分析

​	直接在里面插入一张图片top_banner.jpg即可，图片的宽度使用100%

##### 案例代码

```html
<!--第1行:横幅图片-->
<tr>
    <td>
        <img src="img/top_banner.jpg" width="100%"/>
    </td>
</tr>
```

#### 3.3.3 Logo所在行

##### 案例效果

![HM_0131_210124](image/HM_0131_210124.png)

##### 案例分析

1. 使用表格嵌套，插入一个1行3列的表格。边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%。
2. 表格只有1行，全部居中
3. 第2列的图片宽度设置为500

##### 案例代码

```html
<!-- 第2行:表格嵌套,插入1行3列的表格 -->
<tr>
    <td>
        <table border="0" width="100%" cellpadding="0" cellspacing="0">
            <!--这一行全部居中-->
            <tr align="center">
                <!--第1列是logo-->
                <td>
                    <img src="img/logo.jpg">
                </td>
                <!--第2列是搜索框,宽度500-->
                <td>
                    <img src="img/search.png" width="500">
                </td>
                <!--第3列是电话-->
                <td>
                    <img src="img/hotel_tel.png">
                </td>
            </tr>
        </table>
    </td>
</tr>
```

#### 3.3.4 导航条

##### 案例效果

![HM_0131_211005](image/HM_0131_211005.png)

##### 案例分析

1. 插入一个1行10列的表格
2. 边框、单元格间隔、单元格与内容的间隔都设置成0，宽度100%，背景色设置#ffc900。
3. 在每个单元格中输入一个菜单项，每个菜单项上可以加上a标签，单元格的高度设置为45
4. tr对齐居中，则表格中的文字居中
5. 上一级的td对齐居中，则表格整个位置居中。

##### 案例代码

```html
<!--第3行:导航条 -->
<tr>
    <td align="center">
        <!--1行10列的表格,宽100%-->
        <table border="0" width="100%" cellpadding="0" cellspacing="0"
               bgcolor="#ffc900">
            <tr align="center">
                <td height="45">
                  <a href="#">首页</a>
                </td>
                <td>
                    门票
                </td>
                <td>
                    酒店
                </td>
                <td>
                    香港车票
                </td>
                <td>
                    出境游
                </td>
                <td>
                    国内游
                </td>
                <td>
                    港澳游
                </td>
                <td>
                    抱团定制
                </td>
                <td>
                    全球自由行
                </td>
                <td>
                    收藏排行榜
                </td>
            </tr>
        </table>
    </td>
</tr>
```

#### 3.3.5 轮播图

##### 案例效果

![HM_0131_212033](image/HM_0131_212033.png)

##### 案例分析

​	因为没有学习相关的知识，这里只要插入一张图片即可，宽度为100%

##### 案例代码

```html
<!--第4行:轮播图-->
<tr>
    <td>
        <img src="img/banner_3.jpg" width="100%">
    </td>
</tr>
```

#### 3.3.6 旅游产品部分

##### 案例效果

![HM_0201_070047](image/HM_0201_070047.png)

##### 案例分析

1. "黑马精选"、"国内游"、"境外游"三个部分嵌套一个6行4列的表格，边框、内边距、单元格间距都为0，表格宽度为95%，表格设置居中。
2. 其中1，3，5行中的四个单元格合并成一个，并在td中添加图标icon_5.jpg、icon_6.jpg、icon_7.jpg和文字，下面使用hr添加水平线，粗细2，颜色为：#ffc900，td单元格的高度为80
3. 第2行嵌每个单元格插入1张图片和文字。文字使用p，添加样式字体大小12px。价格使用font标签设置为红色。
4. 第4行第一列的两个单元格合并成一个单元格并插入1张图片guonei_1.jpg，宽度30%，右边1每个单元格插入1张图片和文字。
5. 第6行与第4行的制作方法相同，左边1列的图片名为jiangwai_1.jpg，修改文字和图片即可

##### 案例代码

```html
<table align="center" border="0" width="95%" cellpadding="0" cellspacing="0">
<!-- 黑马精选 -->
<tr>
<td height="80">
	<img src="img/icon_5.jpg">
	黑马精选
	<hr color="#ffc900" size="2"/>
</td>
</tr>
<!-- 1行4列的表格 -->
<tr>
<td>
	<table width="100%" cellspacing="0" cellpadding="0">
		<tr>
			<td>
				<!-- 图片和文字 -->
				<img src="img/jiangxuan_1.jpg">
				<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
				<span style="color:red">&yen;899</span>
			</td>
			<td>
				<!-- 图片和文字 -->
				<img src="img/jiangxuan_1.jpg">
				<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
				<span style="color:red">&yen;899</span>
			</td>
			<td>
				<!-- 图片和文字 -->
				<img src="img/jiangxuan_1.jpg">
				<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
				<span style="color:red">&yen;899</span>
			</td>
			<td>
				<!-- 图片和文字 -->
				<img src="img/jiangxuan_1.jpg">
				<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
				<span style="color:red">&yen;899</span>
			</td>
		</tr>
	</table>
</td>
</tr>
<!-- 国内游 -->
<tr>
<td height="80">
	<img src="img/icon_6.jpg">
	国内游
	<hr color="#ffc900" size="2"/>
</td>
</tr>
<!-- 1行2列,左边占30% -->
<tr>
<td>
	<table width="100%" cellspacing="0" cellpadding="0">
		<tr>
			<td>
				<img src="img/guonei_1.jpg">
			</td>
			<td>
				<!-- 嵌套2列3列的表格 -->
				<table width="100%" cellspacing="0" cellpadding="0">
					<tr>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
					</tr>
					<tr>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size: 12px;">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
					</tr>
				</table>
			</td>
		</tr>
	</table>
</td>
</tr>
<!--境外游-->
<tr>
<td height="80">
	<img src="img/icon_7.jpg">
	境外游
	<hr color="#ffc900" size="2"/>
</td>
</tr>
<tr>
<td>
	<table width="100%" cellspacing="0" cellpadding="0">
		<tr>
			<td>
				<img src="img/jiangwai_1.jpg">
			</td>
			<td>
				<!-- 嵌套2列3列的表格 -->
				<table width="100%" cellspacing="0" cellpadding="0">
					<tr>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
					</tr>
					<tr>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size: 12px;">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
						<td>
							<!-- 图片和文字 -->
							<img src="img/jiangxuan_2.jpg">
							<p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
							<span style="color:red">&yen;699</span>
						</td>
					</tr>
				</table>
			</td>
		</tr>
	</table>
</td>
</tr>
</table>
```

#### 3.3.7 页面脚部

##### 案例效果

![HM_0201_083819](image/HM_0201_083819.png)

##### 案例分析

1. 第1行插入图片footer_service.png，宽度100%
2. 第2行输入文字，背景色#ffc900，内容居中，大小2，颜色用灰色

##### 案例代码

```html
<tr>
    <td>
        <img src="img/footer_service.png" width="100%">
    </td>
</tr>
<tr>
    <td bgcolor="#ffc900" align="center" height="45">
        <font color="gray" size="2">江苏传智播客教育科技股份有限公司 版权所有Copyright 2006-2018, All Rights Reserved 苏ICP备16007882</font>
    </td>
</tr>
```

### 3.4 完整案例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>黑马旅游网</title>
</head>
<body>
<table border="0" width="100%" cellpadding="0" cellspacing="0">
<thead>
<!--第1行:横幅图片-->
<tr>
    <td>
        <img src="img/top_banner.jpg" width="100%"/>
    </td>
</tr>
<!-- 第2行:表格嵌套,插入1行3列的表格 -->
<tr>
    <td>
        <table border="0" width="100%" cellpadding="0" cellspacing="0">
            <!--这一行全部居中-->
            <tr align="center">
                <!--第1列是logo-->
                <td>
                    <img src="img/logo.jpg">
                </td>
                <!--第2列是搜索框,宽度500-->
                <td>
                    <img src="img/search.png" width="500">
                </td>
                <!--第3列是电话-->
                <td>
                    <img src="img/hotel_tel.png">
                </td>
            </tr>
        </table>
    </td>
</tr>
<!--第3行:导航条 -->
<tr>
    <td align="center">
        <!--1行10列的表格,宽100%-->
        <table border="0" width="100%" cellpadding="0" cellspacing="0" bgcolor="#ffc900">
            <tr align="center">
                <td height="45">
                    <a href="#">首页</a>
                </td>
                <td>
                    门票
                </td>
                <td>
                    酒店
                </td>
                <td>
                    香港车票
                </td>
                <td>
                    出境游
                </td>
                <td>
                    国内游
                </td>
                <td>
                    港澳游
                </td>
                <td>
                    抱团定制
                </td>
                <td>
                    全球自由行
                </td>
                <td>
                    收藏排行榜
                </td>
            </tr>
        </table>
    </td>
</tr>
<!--第4行:轮播图-->
<tr>
    <td>
        <img src="img/banner_3.jpg" width="100%">
    </td>
</tr>
</thead>
<tbody>
<!--第5行:旅游产品,6行1列的表格-->
<tr>
    <td>
        <table align="center" border="0" width="95%" cellpadding="0" cellspacing="0">
            <!-- 黑马精选 -->
            <tr>
                <td height="80">
                    <img src="img/icon_5.jpg">
                    黑马精选
                    <hr color="#ffc900" size="2"/>
                </td>
            </tr>
            <!-- 1行4列的表格 -->
            <tr>
                <td>
                    <table width="100%" cellspacing="0" cellpadding="0">
                        <tr>
                            <td>
                                <!-- 图片和文字 -->
                                <img src="img/jiangxuan_1.jpg">
                                <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                <span style="color:red">&yen;899</span>
                            </td>
                            <td>
                                <!-- 图片和文字 -->
                                <img src="img/jiangxuan_1.jpg">
                                <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                <span style="color:red">&yen;899</span>
                            </td>
                            <td>
                                <!-- 图片和文字 -->
                                <img src="img/jiangxuan_1.jpg">
                                <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                <span style="color:red">&yen;899</span>
                            </td>
                            <td>
                                <!-- 图片和文字 -->
                                <img src="img/jiangxuan_1.jpg">
                                <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                <span style="color:red">&yen;899</span>
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
            <!-- 国内游 -->
            <tr>
                <td height="80">
                    <img src="img/icon_6.jpg">
                    国内游
                    <hr color="#ffc900" size="2"/>
                </td>
            </tr>
            <!-- 1行2列,左边占30% -->
            <tr>
                <td>
                    <table width="100%" cellspacing="0" cellpadding="0">
                        <tr>
                            <td>
                                <img src="img/guonei_1.jpg">
                            </td>
                            <td>
                                <!-- 嵌套2列3列的表格 -->
                                <table width="100%" cellspacing="0" cellpadding="0">
                                    <tr>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_2.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_2.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_2.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_2.jpg">
                                            <p style="font-size: 12px;">
                                                上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_2.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_2.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                    </tr>
                                </table>
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
            <!--境外游-->
            <tr>
                <td height="80">
                    <img src="img/icon_7.jpg">
                    境外游
                    <hr color="#ffc900" size="2"/>
                </td>
            </tr>
            <tr>
                <td>
                    <table width="100%" cellspacing="0" cellpadding="0">
                        <tr>
                            <td>
                                <img src="img/jiangwai_1.jpg">
                            </td>
                            <td>
                                <!-- 嵌套2列3列的表格 -->
                                <table width="100%" cellspacing="0" cellpadding="0">
                                    <tr>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_3.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_3.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_3.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_3.jpg">
                                            <p style="font-size: 12px;">
                                                上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_3.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                        <td>
                                            <!-- 图片和文字 -->
                                            <img src="img/jiangxuan_3.jpg">
                                            <p style="font-size:12px">上海直飞三亚5天4晚自由行(春节预售+亲子/蜜月/休闲游首选+豪华酒店任选+接送机)</p>
                                            <span style="color:red">&yen;699</span>
                                        </td>
                                    </tr>
                                </table>
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
        </table>
    </td>
</tr>
</tbody>
<tfoot>
<!-- 倒数第2行插入图片 -->
<tr>
    <td>
        <img src="img/footer_service.png" width="100%">
    </td>
</tr>
<tr>
    <td bgcolor="#ffc900" align="center" height="45">
        <font color="gray" size="2">江苏传智播客教育科技股份有限公司 版权所有Copyright 2006-2018, All Rights Reserved
            苏ICP备16007882</font>
    </td>
</tr>
</tfoot>
</table>
</body>
</html>
```
