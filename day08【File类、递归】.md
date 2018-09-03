# day08【File类、递归】

## 主要内容

*  File类
*  递归


## 教学目标

- [ ] 能够说出File对象的创建方式
- [ ] 能够说出File类获取名称的方法名称
- [ ] 能够说出File类获取绝对路径的方法名称
- [ ] 能够说出File类获取文件大小的方法名称
- [ ] 能够说出File类判断是否是文件的方法名称
- [ ] 能够说出File类判断是否是文件夹的方法名称
- [ ] 能够辨别相对路径和绝对路径
- [ ] 能够遍历文件夹
- [ ] 能够解释递归的含义
- [ ] 能够使用递归的方式计算5的阶乘
- [ ] 能够说出使用递归会内存溢出隐患的原因

# 第一章 File类,代表文件或者文件夹的路径

路径:代表文件或者文件夹的地址,比如d:\\a.png;

## 1.1 概述

`java.io.File` 类是文件和目录(文件夹)路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作。

## 1.2 构造方法

* `public File(String pathname) ` ：通过将给定的**路径名字符串**转换为抽象路径名来创建新的 File实例。  
* `public File(String parent, String child) ` ：从**父路径名字符串和子路径名字符串**创建新的 File实例。
* `public File(File parent, String child)` ：从**父抽象路径名和子路径名字符串**创建新的 File实例。  


* 构造举例，代码如下：

```java
// 文件路径名
String pathname = "D:\\aaa.txt";
File file1 = new File(pathname); 

// 文件路径名
String pathname2 = "D:\\aaa\\bbb.txt";
File file2 = new File(pathname2); 

// 通过父路径和子路径字符串
 String parent = "d:\\aaa";
 String child = "bbb.txt";
 File file3 = new File(parent, child);

// 通过父级File对象和子路径字符串
File parentDir = new File("d:\\aaa");
String child = "bbb.txt";
File file4 = new File(parentDir, child);

//把字符串路径封装成File类型的路径,就可以调用File类的方法,做更多的事情,上面三个构造方法都是一样的路径,
//具体使用哪个看需求,但是一般使用最简单的那个
```



## 1.3 常用方法

### 获取功能的方法

* `public String getAbsolutePath() ` ：返回此File的绝对路径名字符串。带有盘符,比如D:\\a.png;

* ` public String getPath() ` ：返回此File类构造方法传入的路径名字符串。 

* `public String getName()`  ：返回由此File表示的文件或文件夹的名称。  

* `public long length()`  ：返回由此File表示的文件的长度,大小,不能获取文件夹的大小 

  方法演示，代码如下：

  ```java
  public class FileGet {
      public static void main(String[] args) {
          File f = new File("d:/aaa/bbb.java");     
          System.out.println("文件绝对路径:"+f.getAbsolutePath());
          System.out.println("文件构造路径:"+f.getPath());
          System.out.println("文件名称:"+f.getName());
          System.out.println("文件长度:"+f.length()+"字节");
  
          File f2 = new File("d:/aaa");     
          System.out.println("目录绝对路径:"+f2.getAbsolutePath());
          System.out.println("目录构造路径:"+f2.getPath());
          System.out.println("目录名称:"+f2.getName());
          System.out.println("目录长度:"+f2.length());
      }
  }
  
  输出结果：
  文件绝对路径:d:\aaa\bbb.java
  文件构造路径:d:\aaa\bbb.java
  文件名称:bbb.java
  文件长度:636字节
  
  目录绝对路径:d:\aaa
  目录构造路径:d:\aaa
  目录名称:aaa
  目录长度:4096
  ```

> API中说明：length()，表示文件的长度。但是File对象表示目录，则返回值未指定。即文件夹不能它来求长度
>
> 

### 绝对路径和相对路径

* **绝对路径**：从盘符开始的路径，这是一个完整的路径。
* **相对路径**：相对于某个东西而言的路径，这是一个便捷的路径，开发中经常使用。注意idea相对的是工作空间而不是工作空间下写的模块(也就是你在eclipse里面认为的项目),而eclipse相对的才是我们写的项目!!!

```java
public class FilePath {
    public static void main(String[] args) {
      	// D盘下的bbb.java文件
        File f = new File("D:\\bbb.java");
        System.out.println(f.getAbsolutePath());
      	
		// 项目下的bbb.java文件
        File f2 = new File("bbb.java");//在idea相对的是工作空间,我们写的项目的那个文件要加上项目名
        System.out.println(f2.getAbsolutePath());
    }
}

输出结果：
D:\bbb.java
D:\idea_project_test4\bbb.java//结果验证了相对路径,在idea相对的是工作空间
```



### 判断功能的方法

- `public boolean exists()` ：此File表示的文件或文件夹路径是否实际存在。
- `public boolean isDirectory()` ：此File表示的是否为文件夹目录。
- `public boolean isFile()` ：此File表示的是否为文件。


方法演示，代码如下：

```java
public class FileIs {
    public static void main(String[] args) {
        File f = new File("d:\\aaa\\bbb.java");
        File f2 = new File("d:\\aaa");
        
      	// 判断是否存在
        System.out.println("d:\\aaa\\bbb.java 是否存在:"+f.exists());
        System.out.println("d:\\aaa 是否存在:"+f2.exists());
        
      	// 判断是文件还是目录
        System.out.println("d:\\aaa 文件?:"+f2.isFile());
        System.out.println("d:\\aaa 目录?:"+f2.isDirectory());
    }
}

输出结果：
d:\aaa\bbb.java 是否存在:true
d:\aaa 是否存在:true
d:\aaa 文件?:false
d:\aaa 目录?:true
```



### 创建删除功能的方法,不存在就创建,存在不创建

- `public boolean createNewFile()` ：当且仅当具有该名称的文件不存在时，才创建一个新的空文件。 
- `public boolean delete()` ：删除文件或文件夹 ,删除不走回收站,直接删除文件夹要求是空文件夹
- `public boolean mkdir()` ：创建单级文件夹,单级,单个文件夹,不管路径多么复杂
- `public boolean mkdirs()` ：创建多级文件夹,多个文件夹,不管路径是否存在,s复数,多个

方法演示，代码如下：

```java
public class FileCreateDelete {
    public static void main(String[] args) throws IOException {
        // 文件的创建
        File f = new File("aaa.txt");
        System.out.println("是否存在:"+f.exists()); // false
        System.out.println("是否创建:"+f.createNewFile()); // true
        System.out.println("是否存在:"+f.exists()); // true
		
     	// 目录的创建
      	File f2= new File("newDir");	
        System.out.println("是否存在:"+f2.exists());// false
        System.out.println("是否创建:"+f2.mkdir());	// true
        System.out.println("是否存在:"+f2.exists());// true

		// 创建多级目录
      	File f3= new File("newDira\\newDirb");
        System.out.println(f3.mkdir());// false
        File f4= new File("newDira\\newDirb");
        System.out.println(f4.mkdirs());// true
      
      	// 文件的删除
       	System.out.println(f.delete());// true
      
      	// 目录的删除
        System.out.println(f2.delete());// true
        System.out.println(f4.delete());// false
    }
}
```



## 1.4 高级获取功能,文件夹遍历,调用方法的对象要求是文件夹路径

* 简单,高级获取功能,值得是打开文件夹,要么得到下面东西的名字,要么得到路径,都存到对应数组里面去了
* `public String[] list()` ：列出,打开调用方法的文件夹,得到下面东西的名字,存到数组里


* `public File[] listFiles()` ：列出,打开调用方法的文件夹,得到下面东西的File类型的路径,存到数组里

  注意,调用list或者listFiles方法的File对象，表示的必须是实际存在的文件夹，否则返回null，无法进行遍历

```java
public class FileFor {
    public static void main(String[] args) {
        File dir = new File("d:\\java_code");
      
      	//打开文件夹得到下面东西名字,多个,存到数组里面
		String[] names = dir.list();
		for(String name : names){
			System.out.println(name);
		}
        
        //打开文件夹得到下面东西路径,File类型路径,File对象,File方法,更加方便
        File[] files = dir.listFiles();
        for (File file : files) {
            System.out.println(file);
        }
    }
}
```



# 第二章 递归

## 2.1 概述

* **递归**：指在当前方法内,方法自己调用自己的这种现象,前提是这个方法也要给调用起来
* 自己调用自己,别人如何调用你,你就怎么自己调用自己!!!

```java
public static void main(String[] args) {//自己调用自己,别人如何调用你,你就怎么自己调用自己!!!
  	//System.out.println("main");
	//main(args);//main方法给别人(java虚拟机jvm)调用,模仿别人调用方式
    
    method();//别人main方法是这么调用我的,我自己调用自己也这么写,这么模仿
}

public static void method(){
    System.out.println("method");
    method();//自己调用自己,别人如何调用你,你就怎么自己调用自己!!!
}

```

**注意事项:**

**1.递归一定要有一个出口,否则容易出现栈内存溢出错误**StackOverflowError

**2.递归调用次数不能过多,否则容易出现栈内存溢出错误**StackOverflowError

**3.构造方法不能递归调用,,否则容易出现栈内存溢出错误**StackOverflowError



## 2.2 递归累和,解决递归问题,找到出口,找到规律,代入公式即可  

### 计算1 ~ n的和

**分析**：num的累和 = num + (num-1)的累和，所以可以把累和的操作定义成一个方法，递归调用。

**出口**:1的累和等于1//

**规律**:n的累和等于n+(n-1的累和),比如1的累和是1,2的累和是2+1,3的累和是3+2的累和,即3的累和是3+ 2+1;

**实现代码**：

```java
public class DiGuiDemo {
	public static void main(String[] args) {
		int sum = getSum(5);//5的累和用我写的方法即公式是这么表示的,其他累和的表示我也会了
		System.out.println(sum);	
	}
  	
	public static int getSum(int n) {//写一个方法,用来求1到n的和,n的累和
         //出口:1的累和等于1,即getSum(1)==1;
		if(n == 1){
			return 1;
		}
      	 
         //规律:n的累和等于n+(n-1的累和),即getSum(n)==n+getSum(n-1);
		return n + getSum(n-1);
	}
}
```

### 代码执行图解,每调用一次方法就会进入栈内存,栈内存有限,消耗过多,溢出

![](img/day08_01_递归累和.jpg)

> 小贴士：如图,递归一定要有个出口，保证递归能够停止下来，而且次数不要太多，否则会发生栈内存溢出。



## 2.3 递归求阶乘,解决递归问题,找到出口,找到规律,代入公式即可

* **阶乘**：所有小于及等于该数的正整数的积。!读作阶乘

```java
n的阶乘：n! = n * (n-1) *...* 3 * 2 * 1 ,如5!=5*4*3*2*1=120;5!=5*4!
```

**分析**：这与累和类似,只不过换成了乘法运算，学员可以自己练习，需要注意阶乘值符合int类型的范围。

```
推理得出：n! = n * (n-1)!
出口:1!=1
规律:n! = n * (n-1)!
```

**代码实现**：

```java
public class DiGuiDemo {
    public static void main(String[] args) {
        int value = getValue(5);//5的阶乘用我写的方法即公式是这么表示的,其他阶乘的表示我也会了
        System.out.println(value);
    }
    
    public static int getValue(int n) {//写一个方法,用来求n的阶乘,公式
      	//出口:1!=1,即getValue(1)==1;
        if (n == 1) {
            return 1;
        }
      	
        //规律:n! = n * (n-1)!,即getValue(n)==n*getValue(n-1);
        return n * getValue(n - 1);
    }
}

//------------------------11235813,斐波那契数列,不死神兔问题递归解决-------------------------------
public class Test11 {
    public static void main(String[] args) {
        int i = f(8);//第12项的结果
        System.out.println(i);//11235813
    }

    //11235813,斐波那契数列,不死神兔
    public static int f(int n){//写一个方法,用来求第n项的结果,f(n)
        //出口:第一项是1f(1)=1,第二项也是1,f(2)=1;
        if (n==1||n==2){
            return 1;
        }

        //规律:任意第三项等于前面两项之和,f(n)=f(n-1)+f(n-2);
        return f(n-1)+f(n-2);
    }
}
```



## 2.4 递归打印一个文件夹下面所有东西的绝对路径

**总结**:打开文件夹的递归都是这样的:打开文件夹,如果是文件就干嘛,如果是文件夹就递归!!!

**分析**：打开要打开的文件夹,判断如果是文件就打印绝对路径,否则是文件夹也打印路径,但是文件夹还要继续递归

**代码实现**：

```java  
import java.io.File;

public class Test10 {
    public static void main(String[] args) {
        //打开要打开的文件夹,判断如果是文件就打印绝对路径,否则是文件夹也打印路径,但是文件夹还要继续递归
        //把字符串路径,文件夹路径,封装成File类型路径调用list...方法打开文件夹
        File dir = new File("E:\\");

        method(dir);
    }

    public static void method(File dir){//打印一个文件夹下面所有东西的绝对路径
        //dir:C:\Users\ywj\Desktop\深圳黑马Java基础38期

        //打开要打开的文件夹,
        File[] arr = dir.listFiles();//null,打开文件夹下面东西的路径,dir特别的人,打开文件夹的奴隶
        if (arr!=null){
            for (File file : arr) {//arr=null,arr.length;
                if (file.isFile()){
                    //判断如果是文件就打印绝对路径
                    System.out.println(file.getAbsolutePath());
                }else {
                    //否则是文件夹也打印路径,但是文件夹还要继续递归
                    System.out.println(file.getAbsolutePath());
                    //file文件夹,还要继续打开,然后判断,重复之前的动作,方法已经做了,自己调用自己,递归
                    method(file);
                }
            }
        }

    }

}
```



# 第三章 综合案例	

## 3.1 文件搜索	

搜索`D:\aaa` 目录中的`.java` 文件。

**分析**：打开要打开的文件夹,判断如果是文件并且文件名后缀以.java结尾,就打印文件路径,否则是文件夹就继续打开重复方法之前的判断动作,自己调用自己,递归

**代码实现**：

```java
import java.io.File;

public class Test11 {
    public static void main(String[] args) {
        File dir = new File("C:\\Users\\ywj\\Desktop\\深圳黑马Java基础38期");

        method(dir);
    }

    //分析：打开要打开的文件夹,判断如果是文件并且文件名后缀以.java结尾,就打印文件路径,否则是文件夹就继续打开重复方法之前的判断动作,自己调用自己,递归
    public static void method(File dir){//写一个方法,打开一个文件夹,得到所有.java结尾的文件,打印绝对路径
        File[] arr = dir.listFiles();//dir,人,打开文件夹的

        if (arr!=null){
            for (File file : arr) {
                if (file.isFile()&&file.getName().endsWith(".java")){
                    System.out.println(file.getAbsolutePath());
                }else if (file.isDirectory()){
                    //否则是文件夹就继续打开重复方法之前的判断动作,自己调用自己,递归
                    method(file);
                }
            }
        }
    }
}
```



## 3.2 路径过滤器优化

`java.io.FileFilter`是一个接口，是File的过滤器。 该接口的对象可以传递给File类的`listFiles(FileFilter)` 作为参数， 接口中只有一个方法。

`boolean accept(File pathname)  ` ：测试pathname是否应该包含在当前File目录中，符合则返回true。

**分析**：

1. 接口作为参数，需要传递子类对象，重写其中方法。我们选择匿名内部类方式，比较简单。
2. `accept`方法，参数为File，表示当前File下所有的子文件和子目录。保留住则返回true，全部过滤掉则返回false。保留规则：
   1. 要么是.java文件。
   2. 要么是目录，用于继续遍历。
3. 通过过滤器的作用，`listFiles(FileFilter)`返回的数组元素中，子文件对象都是符合条件的，可以直接打印。
4. 代码简单使用

~~~java
import java.io.File;
import java.io.FileFilter;

public class Test12 {
    public static void main(String[] args) {
        File dir = new File("C:\\Users\\ywj\\Desktop\\深圳黑马Java基础38期");

        File[] arr = dir.listFiles(new FileFilter() {//路径过滤器
            @Override
            public boolean accept(File pathname) {//过滤
//                return true;//返回false,对应数组没有值,返回true才有
                  //特殊true,满足一定条件,是文件并且以.txt结尾的路径才存到数组
//                System.out.println(pathname);//打开文件下面东西的路径
                return pathname.isFile()&&pathname.getName().endsWith(".txt");//true
            }
        });

        for (File file : arr) {
            System.out.println(file);//C:\Users\ywj\Desktop\深圳黑马Java基础38期\新建文本文档.txt
            System.out.println(file.getName());//新建文本文档.txt
        }

    }
}
~~~



**代码实现：**

```java
import java.io.File;
import java.io.FileFilter;

public class Test13 {
    public static void main(String[] args) {
        File dir = new File("C:\\Users\\ywj\\Desktop\\深圳黑马Java基础38期");

        method(dir);
    }

    //打开要打开的文件夹,判断如果是文件并且文件名后缀以.java结尾,就打印文件路径,否则是文件夹就继续打开重复方法之前的判断动作,自己调用自己,递归
    public static  void method( File dir){//打印一个文件夹下面所有后缀以.java结尾的绝对路径,过滤器,提高效率
        File[] arr = dir.listFiles(new FileFilter() {//dir,人,打开文件夹过滤的
            @Override
            public boolean accept(File pathname) {//pathname打开文件夹下面东西路径
//                System.out.println(pathname);//C:\Users\ywj\Desktop\深圳黑马Java基础38期\day13
                return pathname.isFile()&&pathname.getName().endsWith("java")||pathname.isDirectory();//是文件夹也存到数组,之后要打开递归
            }
        });

        if (arr!=null){
            for (File file : arr) {
                if (file.isFile()){
                    System.out.println(file.getAbsolutePath());
                }else {
                    //是文件夹,就要进行打开然后过滤判断,这些东西,上面已经做了,方法自己调用自己,递归
                    method(file);
                }
            }
        }

    }
}
```



## 3.3 Lambda优化

**分析：**`FileFilter`是只有一个方法的接口，因此可以用lambda表达式简写。

lambda标准格式：

```java
()->{ }
```

**代码实现：**

```java
public static void printDir3(File dir) {
  	// lambda的改写
    File[] files = dir.listFiles(f ->{//lambda省略格式
      	return f.getName().endsWith(".java") || f.isDirectory(); 
    });
  	
	// 循环打印
    for (File file : files) {
        if (file.isFile()) {
            System.out.println("文件名:" + file.getAbsolutePath());
      	} else {
        	printDir3(file);
      	}
    }
}
//总结:打开文件夹的递归都是这样的:打开文件夹,如果是文件就干嘛,如果是文件夹就递归!!!

//========================================课堂代码示例=========================================//
import java.io.File;

public class Test13 {
    public static void main(String[] args) {
        File dir = new File("C:\\Users\\ywj\\Desktop\\深圳黑马Java基础38期");

        method(dir);
    }

    //打开要打开的文件夹,判断如果是文件并且文件名后缀以.java结尾,就打印文件路径,否则是文件夹就继续打开重复方法之前的判断动作,自己调用自己,递归
    public static  void method( File dir){//打印一个文件夹下面所有后缀以.java结尾的绝对路径,过滤器,提高效率
//        File[] arr = dir.listFiles(new FileFilter() {//dir,人,打开文件夹过滤的
//            @Override
//            public boolean accept(File pathname) {//pathname打开文件夹下面东西路径
////                System.out.println(pathname);//C:\Users\ywj\Desktop\深圳黑马Java基础38期\day13
//                return pathname.isFile()&&pathname.getName().endsWith("java")||pathname.isDirectory();//是文件夹也存到数组,之后要打开递归
//            }
//        });

        //lambda接口只有一个抽象方法,函数式接口,函数式编程,用的lambda表达式,参数传递执行方法体,标准格式
//        File[] arr = dir.listFiles((File pathname)->{return pathname.isFile()&&pathname.getName().endsWith("java")||pathname.isDirectory();});//标准格式

        File[] arr = dir.listFiles( pathname-> pathname.isFile()&&pathname.getName().endsWith("java")||pathname.isDirectory());//省略格式

        if (arr!=null){
            for (File file : arr) {
                if (file.isFile()){
                    System.out.println(file.getAbsolutePath());
                }else {
                    //是文件夹,就要进行打开然后过滤判断,这些东西,上面已经做了,方法自己调用自己,递归
                    method(file);
                }
            }
        }

    }
}
```

