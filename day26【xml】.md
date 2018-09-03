# XML&DOM4J&动态代理

# 学习目标

1. 能够说出XML的作用(掌握)
2. 了解XML的组成元素(了解)
3. 能够说出有哪些XML约束技术(了解)
4. 能够说出解析XML文档DOM方式原理(了解)
5. 能够使用DOM4J解析XML(重点掌握)
6. 能够使用xpath解析XML(重点掌握)
7. 掌握动态代理的使用(掌握)

# 第1章 XML

## 1.1 XML介绍

### 1.1.1 什么是XML

- XML 指可扩展标记语言（**EXtensible Markup Language**）
- XML 是一种**标记语言**，很类似 HTML，HTML文件也是XML文档
- XML 的设计宗旨是**传输数据和数据存储(最主要)**，而非显示数据
- XML 标签没有被预定义。您需要**自行定义标签**。
- XML 被设计为具有**自我描述性(就是易于阅读)**。
- XML 是 **W3C 的推荐标准**

  W3C在1988年2月发布1.0版本，2004年2月又发布1.1版本，单因为1.1版本不能向下兼容1.0版本，所以1.1没有人用。同时，在2004年2月W3C又发布了1.0版本的第三版。我们要学习的还是1.0版本。

### 1.1.2 XML 与 HTML 的主要差异

- XML 不是 HTML 的替代。
- XML 和 HTML 为不同的目的而设计。
- XML 被设计为传输和存储数据，其焦点是数据的内容。
- HTML 被设计用来显示数据，其焦点是数据的外观。
- HTML 旨在显示信息，而 XML 旨在传输信息。

### 1.1.3 XML文件案例编写person.xml文件

#### 1.1.3.1 需求

​	编写xml文档，用于描述人员信息，person代表一个人员，id是人员的属性代表人员编号。人员信息包括age年龄、name姓名、sex性别信息。

#### 1.1.3.2 效果

使用浏览器运行person.xml文件效果如下

![](img/06.png)

#### 1.1.3.3 实现步骤

步骤1：使用idea开发工具，选择当前项目鼠标右键新建“”，如图

![](img/03.png)

![](img/04.png)

步骤2：编写文件person.xml文件，内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<person id="110">
	<age>18</age>		<!--年龄-->
	<name>张三</name>	  <!--姓名-->
	<sex/>				<!--性别-->
</person>
```

步骤3：如图点击浏览器运行

![](img/05.png)

步骤4：浏览器运行效果如下

![](img/06.png)

## 1.2 XML作用

XML在企业开发中主要有两种应用场景：

1）XML可以作为数据交换的载体，也就是说使用XML格式进行数据的传输。(在客户端与服务器之间进行数据传递)XML 数据是以纯文本格式的文件数据（以.xml扩展名的文件），因此提供了一种独立于软件和硬件的数据，可用于不同软件或系统之间传输数据。(了解)

2）XML也可以作为配置文件，例如后面框架阶段我们学习的Spring框架的配置都是通过XML进行配置的（企业开发中经常使用的）

例如：spring框架的 ApplicationContext.xml配置文件。

## 1.3 XML的组成元素

XML文件数据一共由7个组成元素构成，分别为文档声明、 元素element、 属性、注释、转义字符、字符区、处理指令。

### 1.3.1 文档声明(必须写在第一行)

```xml
<?xml version="1.0" encoding="utf-8" ?>
```

1. 文档声明必须为<?xml开头，以？>结束；
2. 文档声明必须从文档的0行0列位置开始；
3. 文档声明只有三个属性：
   - version：指定XML文档版本。必须属性，这里一般选择1.0；
   - enconding：指定当前文档的编码，可选属性，默认值是utf-8；
   - standalone：指定文档独立性。可选属性，默认值为no，表示当前文档不是独立的文档，会依赖外部文件；如果为no表示当前文档是独立的文档

### 1.3.2 元素element(就是标签)

```xml
<person>
```

1. 元素是XML文档中最重要的组成部分；
2. 普通元素的结构由开始标签、元素体、结束标签组成。例如：``<name>张三</name>``
3. 元素体：元素体可以是元素，也可以是文本，例如：``<person><name>张三</name></person>``
4. 空元素：空元素只有标签，而没有结束标签，但元素必须自己闭合，例如：``<sex/>``
5. 元素命名
   - 区分大小写
   - 不能使用空格，不能使用冒号
   - 不建议以XML、xml、Xml开头
   - 标签名不能使用数字开头
6. 格式化良好的XML文档，必须只有一个根元素。

### 1.3.3 属性

```xml
<person id="110">
```

1. 属性是元素的一部分，它必须出现在元素的开始标签中
2. 属性的定义格式：属性名=“属性值”，其中属性值必须使用单引或双引号括起来
3. 一个元素可以有0~N个属性，但一个元素中不能出现同名属性
4. 属性名不能使用空格、冒号等特殊字符，且必须以字母开头

### 1.3.4 注释

```xml
<!--注释内容-->
```

XML的注释与HTML相同，既以``<!--``开始，``-->``结束。

### 1.3.5 转义字符

​	XML中的转义字符与HTML一样。因为很多符号已经被文档结构所使用，所以在元素体或属性值中想使用这些符号就必须使用转义字符（也叫实体字符），例如：">"、"<"、"'"、"""、"&"。

| 字符 | 预定义的转义字符 | 说明 |
| :--: | :--------------: | :--: |
|  <   |     ``&lt;``     | 小于 |
|  >   |    `` &gt;``     | 大于 |
|  &   |    `` &amp;``    | 和号 |

*注意：严格地讲，在 XML 中仅有字符 "<"和"&" 是非法的。省略号、引号和大于号是合法的，但是把它们替换为实体引用是个好的习惯。*

转义字符应用示例：

​	假如您在 XML 文档中放置了一个类似 "<" 字符，那么这个文档会产生一个错误，这是因为解析器会把它解释为新元素的开始。因此你不能这样写：

```xml
<message>if salary < 1000 then</message>
```

为了避免此类错误，需要把字符 "<" 替换为实体引用，就像这样：

```xml
<message>if salary &lt; 1000 then</message>
```

### 1.3.6 字符区(了解)

```
<![CDATA[if (a < b && a < 0) then]]>
```

1.  CDATA 指的是不应由 XML 解析器进行解析的文本数据（Unparsed Character Data）
2.  字符区格式：``<![CDATA[文本数据]]>``，CDATA 部分由 "<![CDATA[" 开始，由 "]]>" 结束；
3.  当大量的转义字符出现在xml文档中时，会使XML文档的可读性大幅度降低。这时如果使用CDATA段就会好一些。

*注意：*

​	CDATA 部分不能包含字符串 "]]>"。也不允许嵌套的 CDATA 部分。

​	标记 CDATA 部分结尾的 "]]>" 不能包含空格或折行。

### 1.3.7 处理指令(了解)

```xml
<?xml-stylesheet type="text/css" href="a.css"?>
在XML文档中可以使用xml-stylesheet指令，通知XML解析引擎，应用css文件显示xml文档内容
```

1. 处理指令必须以“<?”作为开头，以“?>”作为结尾，XML声明语句就是最常见的一种处理指令。 
2. 处理指令，简称PI （processing instruction），处理指令用来指挥解析引擎如何解析XML文档内容。
3. xml-stylesheet指令应用样式需要使用浏览器浏览才可以看到效果。

处理指令示例：

现有person.xml文档内容如下：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<person>
  <age>20</age><!--年龄-->
  <name>张三</name><!--姓名-->
  <sex/>
  <description><![CDATA[我非常喜欢阅读，阅读过<<java编程思想>>、<<大型企业网站架构>>]]>	</description><!--描述-->
</person>
```

使用浏览器浏览效果如下：

![](img/01.png)

现在有样式文件a.css文件内容如下：

```css
description{
    color: blue;
    font-size: 20px;
}/*操作<description>元素样式字体为蓝色，字体字号大小为20px*/
```

操作person.xml文档应用a.css样式文件：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<?xml-stylesheet type="text/css" href="a.css" ?>
<person>
    <age>20</age><!--年龄-->
    <name>张三</name><!--姓名-->
    <sex/>
    <description><![CDATA[我非常喜欢阅读，阅读过<<java编程思想>>、<<大型企业网站架构>>]]></description><!--描述-->
</person>
```

使用浏览器浏览效果如下：

![](img/02.png)

## 1.4 XML文件的约束(了解)

常见的xml约束：DTD、Schema

### 1.4.2 dtd约束

#### 1.4.2.1 概念

​	DTD是文档类型定义（Document Type Definition）。DTD 可以定义在 XML 文档中出现的元素、这些元素出现的次序、它们如何相互嵌套以及XML文档结构的其它详细信息。

#### 1.4.2.2 约束体验

体验效果说明：当编写xml文档时不符合指定dtd约束时，进行提示xml编写错误，如下图：

![](img/07.png)

体验步骤:

步骤1：新建bookshelf.dtd文件，选择项目鼠标右键“NEW”-->"File",文件名为“bookshelf.dtd”

步骤2：bookshelf.dtd文件内容如下

```dtd
<!ELEMENT 书架 (书+)>
        <!ELEMENT 书 (书名,作者,售价)><!--约束元素书的子元素必须为书名、作者、售价-->
        <!ELEMENT 书名 (#PCDATA)>
        <!ELEMENT 作者 (#PCDATA)>
        <!ELEMENT 售价 (#PCDATA)>
        ]>
```

步骤三：新建books.xml，代码如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE 书架 SYSTEM "bookshelf.dtd"><!--指定使用bookshelf.dtd文件约束当前xml文档-->
<书架>
    <书>
        <书名>JavaWeb开发教程</书名>
        <作者>张孝祥</作者>
        <售价>100.00元</售价>
    </书>
    <书>
        <书名>三国演义</书名>
        <作者>罗贯中</作者>
        <售价>100.00元</售价>
        <测试>hello</测试><!--不符合约束，书的子元素必须为书名、作者、售价-->
    </书>
</书架>
```

步骤四：idea开发工具books.xml的dtd约束验证不通过的效果如下

![](img/09.png)

#### 1.4.2.3 dtd学习要求

​	在企业实际开发中，我们很少自己编写DTD约束文档，通常情况下通过框架提供的DTD约束文档编写对应的XML文档。所以这一知识点的要求是可以根据DTD约束文档内容编写XML文档。

#### 1.4.2.4 语法

#####14.2.4.1 文档声明

1. 内部DTD，在XML文档内部嵌入DTD，只对当前XML有效。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE 根元素 [...//具体语法]><!--内部DTD-->
   <根元素>
   </根元素>
   ```

2. 外部DTD—本地DTD，DTD文档在本地系统上，企业内部自己项目使用。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE 根元素 SYSTEM "bookshelf.dtd"><!--外部本地DTD-->
   <根元素>
   </根元素>
   ```

3. 外部DTD—公共DTD，DTD文档在网络上，一般都有框架提供

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">
   <web-app>
   </web-app>
   ```

##### 14.2.4.2 元素声明

1. 约束元素的嵌套层级

   语法

   ```
   <!ELEMENT 父标签 （子标签1，子标签2，…）>
   ```

   代码

   ```dtd
   <!ELEMENT 书架 (书+)>  <!--约束根元素是"书架"，"书架"子元素为"书"，“+”为数量词，请看下面介绍-->
   <!ELEMENT 书 (书名,作者,售价)><!--约束"书"子元素依次为“书名”、“作者”、“售价”，“+”书元素至少1次-->
   ```

2. 约束元素体里面的数据

   语法

   ```
   <!ELEMENT 标签名字 标签类型>
   ```

   标签类型

   | 标签类型 | 代码写法  | 说明                 |
   | -------- | --------- | -------------------- |
   | PCDATA   | (#PCDATA) | 被解释的字符串数据   |
   | EMPTY    | EMPTY     | 即空元素，例如\<hr/> |
   | ANY      | ANY       | 即任意类型           |

   代码

   ```dtd
   <!ELEMENT 书名 (#PCDATA)>  	<!--"书名"元素体为字符串数据-->
   <!ELEMENT 作者 (#PCDATA)>		<!--"作者"元素体为字符串数据-->
   <!ELEMENT 售价 (#PCDATA)>		<!--"售价"元素体为字符串数据-->
   <!ELEMENT 出版日期 ANY>		   <!--"出版日期"元素体为任意类型-->
   <!ELEMENT 版本号 EMPTY>		<!--"版本号"元素体为空元素-->
   ```

3. 数量词

   | 数量词符号 | 含义                         |
   | ---------- | ---------------------------- |
   | +          | 表示元素可以出现至少1个      |
   | ?          | 表示元素可以是0或1个         |
   | ,          | 表示元素需要按照顺序显示     |
   | \|         | 表示元素需要选择其中的某一个 |
   | *          | 表示元素可以出现0到多个      |

##### 14.2.4.3 属性声明

语法

```dtd
<!ATTLIST 标签名称 
		属性名称1 属性类型1 属性说明1
		属性名称2 属性类型2 属性说明2
		…
>
```

属性类型

| 属性类型       | 含义                                       |
| ---------- | ---------------------------------------- |
| CDATA      | 代表属性是文本字符串， eg:<!ATTLIST 属性名 CDATA 属性说明> |
| ID         | 代码该属性值唯一，不能以数字开头， eg:<!ATTLIST 属性名 ID 属性说明> |
| ENUMERATED | 代表属性值在指定范围内进行枚举 Eg:<!ATTLIST属性名 (社科类\|工程类\|教育类) "社科类"> "社科类"是默认值，属性如果不设置默认值就是"社科类" |

属性说明

| 属性说明      | 含义                                       |
| --------- | ---------------------------------------- |
| #REQUIRED | 代表属性是必须有的                                |
| #IMPLIED  | 代表属性可有可无                                 |
| #FIXED    | 代表属性为固定值，实现方式：book_info CDATA #FIXED "固定值" |

代码

```dtd
<!ATTLIST 书									<!--设置"书"元素的的属性列表-->
		id ID #REQUIRED						 <!--"id"属性值为必须有-->
		编号 CDATA #IMPLIED				    <!--"编号"属性可有可无-->
		出版社 (清华|北大|传智播客) "传智播客"   <!--"出版社"属性值是枚举值，默认为“传智播客”-->
		type CDATA #FIXED "IT"                <!--"type"属性为文本字符串并且固定值为"IT"-->
>
```

### 1.4.3 schema约束

#### 1.4.3.1 概念

Schema 语言也可作为 XSD（XML Schema Definition）。

Schema 比DTD强大，是DTD代替者。

Schema 本身也是XML文档，单Schema文档扩展名为xsd，而不是xml。

Schema 功能更强大，数据类型约束更完善。

#### 1.4.3.1 约束体验

体验效果说明：体验schema约束XML文档中对元素体数据类型的约束。效果如下：

![](img/12.png)

DTD约束无法对具体数据类型进行约束,所以开发工具没有任何错误提示，如下效果：

![](img/11.png)

实现步骤

步骤1：新建schema约束文件bookshelf.xsd，对售价约束数据类型，代码如下

```scheme
<?xml version="1.0" encoding="UTF-8" ?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://www.itcast.cn"
           elementFormDefault="qualified">
        <xs:element name='书架' >
                <xs:complexType>
                        <xs:sequence maxOccurs='unbounded' >
                                <xs:element name='书' >
                                        <xs:complexType>
                                                <xs:sequence>
                                                     <xs:element name='书名' type='xs:string' />
                                                     <xs:element name='作者' type='xs:string' />
                                                     <xs:element name='售价' type='xs:double' />
                                                </xs:sequence>
                                        </xs:complexType>
                                </xs:element>
                        </xs:sequence>
                </xs:complexType>
        </xs:element>
</xs:schema>

```

步骤2：新建books2.xml使用schema约束文件bookshelf.xsd，代码如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<书架
xmlns="http://www.itcast.cn"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.itheima.cn bookshelf.xsd"
><!--指定schema文档约束当前XML文档-->
    <书>
        <书名>JavaScript网页开发</书名>
        <作者>张孝祥</作者>
        <售价>abc</售价>
    </书>
</书架>
```

步骤3：开发工具提示效果

![](img/12.png)

#### 1.4.3.1 schema学习要求

​	虽然schema功能比dtd强大，但是编写要比DTD复杂，同样以后我们在企业开发中也很少会自己编写schema文件。本知识点大家可以根据拓展资料进行自行学习，提供大家的自学能力。



​	xml编写与约束内容已经完成了，根据xml的作用我们了解到，无论是xml作为配置文件还是数据传输，我们的程序都要获取xml文档中的数据以便我们进行具体的业务操作，接下来我们就要学习XML解析技术DOM4J。

# 第2章XML的解析

## 2.1 XML解析

### 2.1.1 解析概述

​	当将数据存储在XML后，我们就希望通过程序获取XML的内容。如果我们使用Java基础所学的IO知识是可以完成的，不过你学要非常繁琐的操作才可以完成，且开发中会遇到不同问题（只读、读写）。人们为不同问题提供不同的解析方式，使用不同的解析器进行解析，方便开发人员操作XML。

### 2.1.2 解析方式和解析器

- 开发中比较常见的解析方式有三种，如下：

  1. DOM：要求解析器把整个XML文档装载到内存，并解析成一个Document对象

     a）优点：元素与元素之间保留结构关系，故可以进行增删改查操作。

     b）缺点：XML文档过大，可能出现内存溢出

  2. SAX：是一种速度更快，更有效的方法。她逐行扫描文档，一边扫描一边解析。并以事件驱动的方式进行具体解析，每执行一行，都触发对应的事件。（了解）

     a）优点：处理速度快，可以处理大文件

     b）缺点：只能读，逐行后将释放资源，解析操作繁琐。

  3. PULL：Android内置的XML解析方式，类似SAX。（了解）

- 解析器，就是根据不同的解析方式提供具体实现。有的解析器操作过于繁琐，为了方便开发人员，有提供易于操作的解析开发包

  ![](img/13.png)

- 常见的解析开发包

  - JAXP：sun公司提供支持DOM和SAX开发包
  - Dom4j：比较简单的的解析开发包(今天学这个)
  - JDom：与Dom4j类似
  - Jsoup：功能强大DOM方式的XML解析开发包，尤其对HTML解析更加方便。

## 2.1 DOM4J的基本概念

​	document for java  民间组织开发的， 支持dom

## 2.2 DOM4J作用

1. 解析XML文档并操作和管理XML元素、属性、元素体等数据。

## 2.3 DOM4J的基本使用

### 2.3.1 DOM解析原理及结构模型

- 解析原理

  XML DOM 和 HTML DOM一样，XML DOM 将整个XML文档加载到内存，生成一个DOM树，并获得一个Document对象，通过Document对象就可以对DOM进行操作。以下面books.xml文档为例。

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <books>
      <book id="0001">
          <name>JavaWeb开发教程</name>
          <author>张孝祥</author>
          <sale>100.00元</sale>
      </book>
      <book id="0002">
          <name>三国演义</name>
          <author>罗贯中</author>
          <sale>100.00元</sale>
      </book>
  </books>
  ```

- 结构模型

  ![](img/15.png)

  DOM中的核心概念就是节点，在XML文档中的元素、属性、文本，在DOM中都是节点！所有的节点都封装到了Document对象中。

### 2.3.3 DOM4J的使用

- **关键是获取标签，只要标签获取到了，那么通过attribute()方法能获取属性对象，通过getText()方法能获取标签中的文本，通过getName()方法获取标签名**

  **接下来的关键是：怎么获取标签。有两种方法:迭代器,elements(),element(“标签名”)**

  **步骤：**

  - 1.导入jar包 dom4j.jar
  - 2.创建解析器
    SAXReader reader = new SAXReader();
  - 3.解析xml 获得document对象 
    Document document = reader.read(url);
    	获取根节点
    	Element root = documen.getRootElement()

  **关于DOM4J解析的总结：**

  1.获取标签

  ​	1.1迭代器（比较麻烦）

  ​	1.2 elements()获取所有子标签     elements("标签名")获取指定标签名的子标签

  2.获取文本

  ​	getText()

  3.获取属性

  ​	attributes()获取所有属性   获取的是Attribute对象的集合

  ​	attribute("属性名")获取对应属性名的一个属性    获取的是Attribute对象
  ​	使用attribute对象获取属性值  getValue()

  

  

  ### 2.3.4 XPATH的使用

  - 定义了一种规则。

  - 使用的方法：

    - selectSingleNode():

    - selectNodes():

      **步骤：**

  - 1、注意：要导包 jaxen...jar

  - 2、创建解析器
    SAXReader reader = new SAXReader();

  - 3、解析xml 获得document对象 
    Document document = reader.read(url);

什么时候使用DOM4J:要遍历所有标签的时候使用

什么时候使用XPATH:查找某一个或者某一类标签的时候使用

## 第3章动态代理

### 3.1 动态代理的概述

动态代理它可以直接给某一个目标对象(委托对象)生成一个代理对象，而不需要代理类存在。

动态代理与静态代理模式原理是一样的，只是动态代理类的源码是在程序运行期间由JVM根据反射等机制动态的生成，所以不存在代理类的字节码文件。代理者和委托者的关系是在程序运行时确定。

### 3.2 动态代理的API

- 1.java.lang.reflect.Proxy:这是 Java 动态代理机制生成的所有动态代理类的父类，它提供了一组静态方法来为一组接口动态地生成代理对象。 
  - static Object newProxyInstance(ClassLoader loader, Class[] interfaces, InvocationHandler h),该方法用于为指定类加载器、一组接口(委托类实现的所有接口)及调用处理器生成动态代理类实例
- 2.java.lang.reflect.InvocationHandler,这是调用处理器接口，它自定义了一个 invoke 方法，用于集中处理在动态代理类对象上的方法调用，通常在该方法中实现对委托类的代理访问。每次生成动态代理类对象时都要指定一个对应的调用处理器对象。 
  - Object invoke(Object proxy, Method method, Object[] args)，该方法负责集中处理动态代理类上的所有方法调用。第一个参数既是代理类实例，第二个参数是被调用的方法对象
- 3.java.lang.ClassLoader 
  这是类装载器类，负责将类的字节码装载到 Java 虚拟机（JVM）中并为其定义类对象，然后该类才能被使用。Proxy 静态方法生成动态代理类同样需要通过类装载器来进行装载才能使用，它与普通类的唯一区别就是其字节码是由 JVM 在运行时动态生成的而非预存在于任何一个 .class 文件中。 
  每次生成动态代理类对象时都需要指定一个类装载器对象 

###3.3动态代理的使用步骤 

- 1.定义一个类实现InvocationHandler接口
- 2.创建上述实现类的对象
- 3.使用Proxy类调用newProxyInstance(ClassLoader loader, Class[] interfaces, InvocationHandler h),并将上一步创建的对象传入该方法中

### 3.4 动态代理的示例代码

- 第一步:定义一个代理接口

```
package com.itheima.dynamicproxy;

public interface KindWomen {
	public void happyWithMan();
	public double collect(double money);
}
```

- 第二步:写两个委托类实现上述接口

  ```
  public class JinlianPan implements KindWomen {
  	@Override
  	public void happyWithMan() {
  		System.out.println("金莲正在happy。。。");
  	}
  
  	@Override  
  	public double collect(double money) {
  		return money;
  	}
  
  }
  ```

- 第三步:定义一个类实现InvocationHandler接口

  ```
  /**
   * 调用处理者，在这里统一处理动态代理类 对象的方法调用。
   */
  public class InvocationHandlerImp implements InvocationHandler {
  	private Object target;
  	public InvocationHandlerImp(Object target) {
  		super();
  		this.target = target;
  	}
  	//该方法就是处理动态代理类对象对象额度方法调用
  	@Override
  	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
  		//此处的三个参数:第一个proxy指的是生成的动态代理对象
  		//第二个参数method是被调用的方法
  		//第三个参数是调用该方法传入的参数
  		String methodName = method.getName();
  		if ("happyWithMan".equals(methodName)) {
  			//如果方法名是happyWithMan，则先调用openHours()方法
  			openHours();
  			//再调用委托对象的happyWithMan()方法
  			method.invoke(target, args);
  			return null;
  		}else if ("collect".equals(methodName)) {
  			//如果方法是collect(),则扣除40%的手续费
  			double money = (double) method.invoke(target, args);
  			System.out.println("平台收取40%的手续费，共计:"+money*0.4+"元");
  			return money*0.6;
  		}
  		return method.invoke(target, args);
  	}
  	public void openHours(){
  		System.out.println("以做衣服的名义把两人安排到已开好的房间里。。。");
  	}
  }
  ```

- 第四步:在调用者中，使用Proxy.newProxyInstance()方法创建代理对象

  ```
  public class MenqingXi {
  
  	public static void main(String[] args) {
  		JinlianPan pan = new JinlianPan();
  		//通过代理调用金莲的happyWithMan()方法
  		InvocationHandlerImp imp = new InvocationHandlerImp(pan);
  		//调用newProxyInstance()方法动态创建代理对象，代理对象类必定跟委托类实现了相同的接口
  		//该方法第一个参数是类加载器,第二个参数是委托类实现的所有接口(使用委托类的字节码文件对象.getInterfaces()方法获取)
  		KindWomen women = (KindWomen) Proxy.newProxyInstance(MenqingXi.class.getClassLoader(), JinlianPan.class.getInterfaces(), imp);
  		//代理对象调用任意方法，都会调用到InvocationHandler的实现类对象的invoke方法
  		women.happyWithMan();
  		double money = women.collect(500);
  		System.out.println("金莲最终拿到"+money+"元");
  	}
  
  }
  ```

  

