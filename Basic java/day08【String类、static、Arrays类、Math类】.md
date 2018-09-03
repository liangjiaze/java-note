# day08【String类、static关键字、Arrays类、Math类】

## 今日内容

*  String类
*  static关键字


*  Arrays类


*  Math类


## 教学目标

- [ ] 能够使用String类的构造方法创建字符串对象
- [ ] 能够明确String类的构造方法创建对象,和直接赋值创建字符串对象的区别

- [ ] 能够使用文档查询String类的判断方法

- [ ] 能够使用文档查询String类的获取方法

- [ ] 能够使用文档查询String类的转换方法

- [ ] 能够理解static关键字

- [ ] 能够写出静态代码块的格式

- [ ] 能够使用Arrays类操作数组

- [ ] 能够使用Math类进行数学运算

# 第一章 String类

## 1.1 String类概述

### 概述

`java.lang.String` 类**代表字符串**。Java程序中所有的字符串文字（例如`"abc"` ）都可以被看作是实现此类的实例(实例就是对象),即告诉我,**"abc"是String类的一个对象!!!**,是对象就可以.来调用方法,实现不同的功能,比如下面:

类 `String` 中包括用于检查各个字符串的方法，比如用于**比较**字符串，**搜索**字符串，**提取**子字符串以及创建具有翻译为**大写**或**小写**的所有字符的字符串的副本等。

### 特点//以后再记,选择题,面试题

1.  字符串不变：字符串的值在创建后不能被更改。

```java
String s1 = "abc";//"abc"对象,s1对象名,存的是这个"abc"对象的地址值
s1 += "d";//s1=s1+"d";//"abcd"是一个新的对象,s1对象名,存的"abcd"对象的地址值
System.out.println(s1); // "abcd",本身是地址值,默认重写toString()的结果
// 内存中有"abc"，"abcd"两个对象，s1从指向"abc"，改变指向，指向了"abcd"。
```

2. 因为String对象是不可变的，所以它们可以被共享。

```java
String s1 = "abc";//"abc"是常量存储在常量池内存块,常量池没有这个常量会帮你创建,有了就使用同一个(共享)
String s2 = "abc";//String类,类型,引用数据类型
String s3 = new String("abc");//"";//new出来东西在堆内存,跟常量池不是同一个地方,地址值不一样
// 内存中只有一个"abc"对象被创建，同时被s1和s2共享。
System.out.println(s1==s2);//true,==号对于引用数据类型比较的是地址值,基本数据类比较的才是值
System.out.println(s1==s3);//false,==号对于引用数据类型比较的是地址值,基本数据类比较的才是值
```

3. `"abc"` 等效于 `char[] data={ 'a' , 'b' , 'c' }`。

```java
例如： 
String str = "abc";//想告诉你,"abc"也是对象而已

相当于： 
char data[] = {'a', 'b', 'c'};     
String str = new String(data);
// String底层是靠字符数组实现的。
```
## 1.2 使用步骤

* 查看类

  * `java.lang.String` ：此类不需要导包。 

* 查看构造方法

  * `public String() ` ：通过无参构造方法创建一个空字符串对象,结果跟""是一样的意思
  * `public String(char[] value) ` ：通过构造方法传入字符数组,把字符数组变成字符串对象
  * `public String(byte[] bytes) ` ：通过构造方法传入字节数组,把字节数组变成字符串对象


```java
public class Test01 {
    public static void main(String[] args) {
        // 无参构造
        String str = new String();//""
        System.out.println(str);//空字符串,字符串里面没有内容,即"",只是打印出来没有双引号

        // 通过字符数组构造
        char[] chars = {'a', 'b', 'c'};
        String str2 = new String(chars);
        System.out.println(str2);//abc

        // 通过字节数组构造
        byte[] bytes = {97, 98, 99};
        String str3 = new String(bytes);
        System.out.println(str3);//abc,97对应的字符是a,根据码表的对应关系来确定,98是b,99是c
        
        //String str = "abc";//"abc"是String类的一个对象!!,str对象名.调用方法,功能
    }
}
```

## 1.3 常用方法

### 判断功能的方法

* `public boolean equals (Object anObject) ` ：判断调用方法的字符串,跟传入方法的字符串对象内容是否相等,在严格区分大小写的情况下,内容相等返回true,否则返回false

* ` public boolean equalsIgnoreCase (String anotherString) ` ：判断调用方法的字符串,跟传入方法的字符串对象内容是否相等,在忽略大小写的情况下,内容相等返回true,否则返回false

  ```java
  public class String_Demo01 {
      public static void main(String[] args) {
          // 创建字符串对象
          String s1 = "hello";//s1对象名.调用方法
          String s2 = "hello";
          String s3 = "HELLO";
          String s4 = new String("hellO");
  
          // boolean equals(Object obj):比较字符串的内容是否相等
          System.out.println(s1.equals(s2)); // true
          System.out.println(s1.equals(s3)); // false
          System.out.println("-----------");
  
          //boolean equalsIgnoreCase(String str):比较字符串的内容是否相等,忽略大小写
          System.out.println(s1.equalsIgnoreCase(s2)); // true
          System.out.println(s1.equalsIgnoreCase(s3)); // true
          System.out.println(s1.equalsIgnoreCase(s4)); // true
          System.out.println("-----------");
      }
  }
  ```

> Object 是” 对象类”的意思,作为参数类型,表示任意对象都可以传递到方法中,即万物皆对象!!!

### 获取功能的方法

- `public int length () ` ：得到调用方法的字符串的长度,字符串由字符构成,即得到字符串里面字符的个数
- `public String concat (String str)` ：把调用方法的字符串跟传入方法的字符串,串成一个新的字符串,+
- ` public char charAt (int index) ` ：得到调用方法的字符串,传入的索引对应的字符,比如传入0索引得到第一个字符,其他以此类推
- `public int indexOf (String str) ` ：得到传入方法的字符串,第一次出现在调用方法的字符串的位置的索引,找不到返回-1表示
- `public int lastIndexOf(String str) ` ：得到传入方法的字符串,最后一次出现在调用方法的字符串的位置的索引,找不到返回-1表示
- ` public String substring (int beginIndex) ` ：截取字符串,从调用方法的字符串传入的开始索引一直截取到末尾,截取,从哪里开始到哪里结束,只告诉我开始索引,没有告诉我结束索引,那默认就截取到字符串的末尾
- ` public String substring (int beginIndex, int endIndex) ` ：截取字符串,从开始索引截取到结束索引-1的那一段,即含头不含尾,或者巧记为神龙见首不见尾

```java
public class String_Demo02 {//temp
      public static void main(String[] args) {
         //创建字符串对象
        String ss = "helloworld";

        // int length():获取字符串的长度，其实也就是字符个数
        System.out.println(ss.length());//10
        System.out.println("--------");

        // String concat (String str):将将指定的字符串连接到该字符串的末尾.
        String s = "helloworld";
        String s2 = s.concat("**hello itheima");
        System.out.println(s2);// helloworld**hello itheima

        // char charAt(int index):获取指定索引处的字符
        System.out.println(s.charAt(0));//h
        System.out.println(s.charAt(1));//e
        System.out.println("--------");

        // int indexOf(String str):获取str在字符串对象中第一次出现的索引,没有返回-1
        System.out.println(s.indexOf("l"));//2
        System.out.println(s.indexOf("owo"));//4
        System.out.println(s.indexOf("ak"));//-1
        System.out.println("--------");

        // String substring(int start):从start开始截取字符串到字符串结尾
        System.out.println(s.substring(0));//helloworld
        System.out.println(s.substring(5));//world
        System.out.println("--------");

        // String substring(int start,int end):从start到end截取字符串。含start，不含end。
        System.out.println(s.substring(0, s.length()));//helloworld
        System.out.println(s.substring(3,8));//lowor,3到7,神龙见首不见尾
      }
}
```

### 转换功能的方法

- `public char[] toCharArray () ` ：把调用方法的字符串转换成字符数组 
- ` public byte[] getBytes () ` ：把调用方法的字符串转换成字节数组
- `public String replace (CharSequence target, CharSequence replacement) ` ：替换调用方法的字符串,用后面的东西替换字符串里面出现的前面的东西,**默认替换所有**,即replace sth with sth;产生新的字符串,原来的字符串没有改变,所以要定义一个新的变量接收方法的结果,CharSequence 字符序列,字符串也是字符序列

```java
public class String_Demo03 {
    public static void main(String[] args) {
        //创建字符串对象
        String s = "abcde";

        // char[] toCharArray():把字符串转换为字符数组
        char[] chs = s.toCharArray();
        for(int x = 0; x < chs.length; x++) {
            System.out.println(chs[x]);//a...
        }
        System.out.println("-----------");

        // byte[] getBytes ():把字符串转换为字节数组
        byte[] bytes = s.getBytes();
        for(int x = 0; x < bytes.length; x++) {
            System.out.println(bytes[x]);//97...
        }
        System.out.println("-----------");

        // 替换字母it为大写IT
        String str = "itcast itheima";
        String replace = str.replace("it", "IT");
        System.out.println(replace); // ITcast ITheima,替换所有
        System.out.println("-----------");
    }
}
```

> CharSequence 是一个接口，也是一种引用类型。作为参数类型，可以把String对象传递到方法中。

### 分割功能的方法

* `public String[] split(String regex)` ：将此字符串按照给定的regex（规则）拆分(切割)为字符串数组

  ```java
  public class String_Demo03 {//注意切割点要用"\\."
    public static void main(String[] args) {
        String s = "aa#bb#cc";//切割,吃掉了,按照什么规则
        String[] arr = s.split("#");
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);//aa bb cc
        }
        
        //创建字符串对象
        String s = "aa|bb|cc";
        String[] strArray = s.split("\\|"); // ["aa","bb","cc"]
        for(int x = 0; x < strArray.length; x++) {
            System.out.println(strArray[x]); // aa bb cc
        }
        
        String s = "aa.bb.cc";//切割,吃掉了,按照什么规则注意切割点要用"\\."
        String[] arr = s.split("\\.");//.在正则表达式里面任意字符,不是我认为的.,转义\\.
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);//aa bb cc
        }
  	  
    }
  }
  ```

## 1.4 String类的练习

### 拼接字符串

定义一个方法，把数组{1,2,3}按照指定个格式拼接成一个字符串。格式参照如下：[word1#word2#word3]。

```java
public class StringTest1 {
    public static void main(String[] args) {
        //定义一个int类型的数组
        int[] arr = {1, 2, 3};

        //调用方法
        String s = arrayToString(arr);

        //输出结果
        System.out.println("s:" + s);//[word1#word2#word3]
    }

    /*
     * 写方法实现把数组中的元素按照指定的格式拼接成一个字符串
     * 两个明确：
     * 返回值类型：String
     * 参数列表：int[] arr
     */
    public static String arrayToString(int[] arr) {
        // 创建字符串s
        String s = "[";

        // 遍历数组，并拼接字符串
        for (int x = 0; x < arr.length; x++) {
            if (x == arr.length - 1) {
                s = s.concat(arr[x] + "]");//s+=....
            } else {
                s = s.concat(arr[x] + "#");
            }
      
        }

        return s;
    }
}
```

### 统计字符个数

键盘录入一个字符，统计字符串中大小写字母及数字字符个数//YMwoaini666

```java
public class StringTest2 {
    public static void main(String[] args) {
        //键盘录入一个字符串数据
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个字符串数据：");
        String s = sc.nextLine();//

        //定义三个统计变量，初始化值都是0
        int bigCount = 0;
        int smallCount = 0;
        int numberCount = 0;

        //遍历字符串，得到每一个字符
        char[] arr = line.toCharArray();
        for (int i = 0; i < arr.length; i++) {
            char c = arr[i];//c每一个字符
            if (c>='a'&&c<='z'){
                small++;//加1,有一个就加1,统计个数
            }else if (c>='A'&&c<='Z'){
                big++;
            }else if (c>='0'&&c<='9'){
                num++;
            }else {
                System.out.println("其他字符");
            }
        }
        
        for(int x=0; x<s.length(); x++) {
            char ch = s.charAt(x);//x,每一个字符
            //拿字符进行判断
            if(ch>='A'&&ch<='Z') {
                bigCount++;
            }else if(ch>='a'&&ch<='z') {
                smallCount++;
            }else if(ch>='0'&&ch<='9') {
                numberCount++;
            }else {
                System.out.println("该字符"+ch+"非法");
            }
        }

        //输出结果
        System.out.println("大写字符："+bigCount+"个");
        System.out.println("小写字符："+smallCount+"个");
        System.out.println("数字字符："+numberCount+"个");
    }
}
```



# 第二章 static关键字,静态共享的意思,用其修饰的东西属于类,随着类的加载而加载(字节码一个一次),优先于对象存在,给类的所有对象共享,所以除了类名.还可以使用对象名.来调用(使用拿到)

## 2.1 概述

关于 `static` 关键字的使用，它可以用来修饰的成员变量和成员方法，被修饰的成员是**属于类**的，而不是单单是属于某个对象的。也就是说，既然属于类,除了用以前的对象名.来调用,还多了用类名.来调用的方式

## 2.2 定义和使用格式  

### 静态变量,又称为类变量

当 `static` 修饰成员变量时，该变量称为**类变量**。该类的每个对象都**共享**同一个类变量的值。任何对象都可以更改该类变量的值，但也可以在不创建该类的对象的情况下对类变量进行操作。

* **类变量**：使用 static关键字修饰的成员变量。

定义格式：

```java
static 数据类型 变量名； 
```

```java
public class Test11 {
    public static void main(String[] args) {
        //static关键字,静态共享的意思,用其修饰的东西属于类,随着类的加载而加载(字节码一个一次),
        //优先于对象存在,给类的所有对象共享,
        //所以除了类名.还可以使用对象名.来调用(使用拿到)

        Person p = new Person();
        p.name = "林志玲";
        p.country = "中国";//台湾是中国的
        System.out.println(p.name);
        System.out.println(p.country);//对象名.来调用(使用拿到)

        Person p2 = new Person();
        p2.name = "苍井空";
        //p2.country = "中国";//日本也是中国的省,共享的
        System.out.println(p2.name);
        System.out.println(p2.country);//给类的所有对象共享,

        System.out.println(Person.country);//中国,属于类的,通过类名.调用,类加载的时候已经存在了,存在就可以使用,随着类的加载而加载,优先于对象存在

    }
}

class Person {//类,类型,模型,模板,具体对象
    String name;//属于对象,并且每一个对象都有自己独有的一份
    static String country;//null,中国是共享,大家都用同一份,所有对象,共享
}
```



### 静态方法,又称为类方法

当`static` 修饰成员方法时，该方法称为**类方法** 。静态方法在声明中有`static` ，建议使用类名来调用，而不需要创建类的对象。调用方式非常简单。

* **类方法**：使用 static关键字修饰的成员方法，习惯称为**静态方法**。

定义格式：

```java
修饰符 static 返回值类型 方法名 (参数列表){ 
	// 执行语句 
}
```

举例：在Student类中定义静态方法

```java
public static void showNum() {
    System.out.println("num:" +  numberOfStudent);
}
```

* **静态方法调用的注意事项：**
  * 静态方法可以直接访问类变量和静态方法。
  * 静态方法**不能直接访问**普通成员变量或成员方法。反之，成员方法可以直接访问类变量或静态方法。
  * 静态方法中，不能使用**this**关键字。

> 小贴士：简记,除非创建对象,否则静态方法里面只能访问静态成员(变量和方法),分清楚什么东西属于类,什么东西属于对象即可理解上面的注意事项!!!

### 调用格式

被static修饰的成员可以并且建议通过**类名直接访问**。虽然也可以通过对象名访问静态成员，原因即多个对象均属于一个类，共享使用同一个静态成员，但是不建议，会出现警告信息。

格式：

```java  
// 访问类变量
类名.类变量名；

// 调用静态方法
类名.静态方法名(参数)； 
```

调用演示，代码如下：

```java
public class StuDemo2 {
    public static void main(String[] args) {      
        // 访问类变量
        System.out.println(Student.numberOfStudent);
        // 调用静态方法
        Student.showNum();
    }
}
```



## 2.3 静态原理图解	

`static` 修饰的内容：

* 是随着类的加载而加载的，且只加载一次。
* 存储于一块固定的内存区域（静态区），所以，可以直接被类名调用。
* 它优先于对象存在，所以，可以被所有对象所共享。

![](img/1.jpg)



## 2.4 静态代码块	

* **静态代码块**：定义在成员位置，即类中方法外,使用static修饰的代码块{ }。
  * 位置：类中方法外。
  * 执行：随着类的加载而执行且执行一次，优先于main方法和构造方法的执行。//记忆

格式：

```java
public class ClassName{
    static {
        // 执行语句 
    }
}
```

作用：给类变量进行初始化赋值。用法演示，代码如下：

```java
public class Game {
    public static int number;
    public static ArrayList<String> list;

    static {
        // 给类变量赋值
        number = 2;
        list = new ArrayList<String>();
        // 添加元素到集合中
        list.add("张三");
        list.add("李四");
    }
}
```

> 小贴士：
>
> static 关键字，可以修饰变量、方法和代码块。在使用的过程中，其主要目的还是想在不创建对象的情况下，去调用方法。下面将介绍两个工具类，来体现static 方法的便利。



# 第三章 Arrays类

## 3.1 概述

`java.util.Arrays`  此类包含用来操作数组的各种方法，比如排序和搜索等。其所有方法均为静态方法，调用起来非常简单。

## 3.2 操作数组的方法

* `public static String toString(int[] a) ` ：返回指定数组内容的字符串表示形式。

```java
public static void main(String[] args) {
    // 定义int 数组
    int[] arr  =  {2,34,35,4,657,8,69,9};
    // 打印数组,输出地址值
    System.out.println(arr); // [I@2ac1fdc4

    // 数组内容转为字符串,注意!是特殊格式的字符串跟打印ArrayList集合对象名的效果一样
    String s = Arrays.toString(arr);
    // 打印字符串,输出内容
    System.out.println(s); // [2, 34, 35, 4, 657, 8, 69, 9]
}
```

* `public static void sort(int[] a)` ：对指定的 int 型数组按数字升序进行排序。

```java
public static void main(String[] args) {
    // 定义int 数组
    int[] arr  =  {24, 7, 5, 48, 4, 46, 35, 11, 6, 2};
    System.out.println("排序前:"+ Arrays.toString(arr)); // 排序前:[24, 7, 5, 48, 4, 46, 35, 11, 6, 2]
    // 升序排序,按照整数从小到大排序
    Arrays.sort(arr);
    System.out.println("排序后:"+ Arrays.toString(arr));// 排序后:[2, 4, 5, 6, 7, 11, 24, 35, 46, 48]
}
```

## 3.3 练习

请使用`Arrays` 相关的API，将一个字符串中的所有字符升序排列，并倒序打印。

```java
public class ArraysTest {
    public static void main(String[] args) {
        // 定义随机的字符串
        String line = "ysKUreaytWTRHsgFdSAoidq";
        // 转换为字符数组
        char[] chars = line.toCharArray();

        // 升序排序
        Arrays.sort(chars);//a,b....
      
        //System.out.println(Arrays.toString(chars));//[A, F, H, K, R, S, T, U, W, a, d, d, e, g, i, o, q, r, s, s, t, y, y]//A65,a97
        
        // 反向遍历打印,chars.for
        for (int i =  chars.length-1; i >= 0 ; i--) {
            System.out.print(chars[i]+" "); // y y t s s r q o i g e d d a W U T S R K H F A 
        }
    }
}
```



#  第四章 Math类

## 4.1 概述

`java.lang.Math` 类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数。类似这样的工具类，其所有方法均为静态方法，并且不会创建对象，调用起来非常简单。

## 4.2 基本运算的方法

* `public static double abs(double a) ` ：返回 double 值的绝对值。 

```java
double d1 = Math.abs(-5); //d1的值为5.0
double d2 = Math.abs(5); //d2的值为5.0
```

* `public static double ceil(double a)` ：返回大于等于参数的最小的整数,最后把结果变成小数

```java
double d1 = Math.ceil(3.3); //d1的值为 4.0,天花板,向上取整,两个整数小,大,取大的那个整数
double d2 = Math.ceil(-3.3); //d2的值为 -3.0,不知道定义什么变量接收,.var自动生成方法的返回值
double d3 = Math.ceil(5.1); //d3的值为 6.0
```

* `public static double floor(double a) ` ：返回小于等于参数最大的整数,最后把结果变成小数

```java
double d1 = Math.floor(3.3); //d1的值为3.0//地板
double d2 = Math.floor(-3.3); //d2的值为-4.0
double d3 = Math.floor(5.1); //d3的值为 5.0
```

* `public static long round(double a)` ：返回最接近参数的 long。(相当于四舍五入方法)  

```java
long d1 = Math.round(5.5); //d1的值为6
long d2 = Math.round(5.4); //d2的值为5
```



## 4.3 练习 

请使用`Math` 相关的API，计算在 `-10.8`  到`5.9`  之间，绝对值大于`6`  或者小于`2.1` 的整数有多少个？

```java
public class Test14 {
    public static void main(String[] args) {
        // 定义最小值
        double min = -10.8;
        // 定义最大值
        double max = 5.9;
        // 定义变量计数
        int count = 0;

        // 范围内循环
        for (double i = Math.ceil(min); i < max; i++) {//Math.ceil(min),向上取整得到小数-10.0,不断加1减少精度丢失
            // 获取绝对值并判断
            if (Math.abs(i) > 6 || Math.abs(i) < 2.1) {
                // 计数
                count++;
                //System.out.println((int)i);//把带有0的小数强制为整数输出
            }
        }

        System.out.println("个数为: " + count + " 个");
    }
}
```

