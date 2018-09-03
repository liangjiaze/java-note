# 连接池
## 学习目标
1. 能够理解连接池解决现状问题的原理
2. 能够使用C3P0连接池
3. 能够编写C3P0连接池工具类
4. 能够使用DRUID连接池
5. 能够使用JdbcTemplate执行SQL语句
6. 能够理解JdbcTemplate的原理


# 第1章 自定义连接池
## 1.1 连接池概念
​	我们现实生活中每日三餐。我们并不会吃一餐饭就将碗丢掉，而是吃完饭后将碗放到碗柜中，下一餐接着使用。目的是重复利用碗，我们的数据库连接也可以重复使用，可以减少数据库连接的创建次数。提高数据库连接对象的使用率。

**连接池的概念**: 连接池是创建和管理数据库连接的缓冲池技术。连接池就是一个容器，连接池中保存了一些数据库连接，这些连接是可以重复使用的。

## 1.2 没有连接池的现状
1. 之前JDBC访问数据库的步骤：
   **创建数据库连接** →**运行SQL语句**→**关闭连接**
   每次数据库访问执行这样重复的动作
   ![](imgs\连接池01.png)

2. **每次创建数据库连接的问题**
   * 获取数据库连接需要消耗比较多的资源，而每次操作都要重新获取新的连接对象，执行一次操作就把连接关闭，而数据库创建连接通常需要消耗相对较多的资源，创建时间也较长。这样数据库连接对象的使用率低。
   * 假设网站一天10万访问量，数据库服务器就需要创建10万次连接，极大的浪费数据库的资源，并且极易造成数据库服务器内存溢出


## 1.3 连接池解决现状问题的原理
1. 连接池在初始化的时候会默认存放n个连接对象并存放到一个容器(LinkedList)中

2. 当需要的连接的时候，如果连接池中有连接就直接从连接池中获取，如果连接池中没有则新创建连接，如果连接数大于最大连接数时，如果再有请求需要获取连接则将其添加到等待队列

3. 对于新创建的连接，一般不会直接销毁，它将被放到连接池中等待重复使用,只有当空闲超时后被释放。

4. 对于原本就在连接池中的连接对象，用完之后直接放回连接池中

5. 连接池中的连接数在空闲时会逐渐趋近于最小连接数,连接池在满载时的连接数会接近最大连接数。

   ​
   ![](imgs\连接池02.png)

**数据库连接池相关API**
​        Java为数据库连接池提供了公共的接口: `javax.sql.DataSource`，各个厂商需要让自己的连接池实现这个接口。这样应用程序可以方便的切换不同厂商的连接池。

数据源(连接池)接口：`javax.sql.DataSource`中的方法
```java
Connection getConnection() 
从连接池中取出一个连接。
```
常见的连接池: `C3P0`、`Druid`、`DBCP`等

## 1.4自定义连接池的要求

1. 在连接池初始化的时候默认存放n个连接对象
2. 当需要连接时，如果连接池中有连接则直接从连接池中获取，没有则新创建
3. 原本就在连接池中的连接，用完之后直接还回连接池中。从连接池的头部获取连接，从连接池的尾部归还连接
4. 对于新创建的连接，用完之后直接销毁。

### 1.4.1自定义连接池第一个版本

* 1. 定义一个MyThreadPool类,该类有一个成员变量pool是LinkedList类型，用于存放连接对象的容器。

* 2. 该类有两个方法，分别用于获取连接对象和归还连接对象

  * 2.1.  获取连接的方法:当pool的长度大于零时从pool中获取(调用removeFirst()方法),当pool的长度小于等于零的时候，创建一个Connection对象
  * 2.2.  归还连接的方法:调用pool的addLast()方法

* 3. 该版本的缺点:如果连接不是原本就在连接池中的连接，也会归还到连接池中去，造成了连接池的体积越来越庞大，内存占用越来越多。

* 4. 解决方法:是原本就在连接池中的连接就归还到连接池中，是新创建的连接就直接销毁。

```
public class MyConnectionPool {
	private LinkedList<Connection> pool = new LinkedList<>();
	private String password = "123";
	private String user = "root";
	private String url = "jdbc:mysql://localhost:3306/day09_1";
	static{
		//注册驱动
		try {
			Class.forName("com.mysql.jdbc.Driver");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	public MyConnectionPool() throws SQLException{
		//初始化的时候，往容器中存放五个连接对象
		for(int i=0;i<5;i++){
			Connection conn = DriverManager.getConnection(url, user, password);
			pool.add(conn);
		}
	}
	public Connection getConnectionFromPool() throws SQLException{
		//从pool的头部拿连接
		//如果池子中有连接，就直接从池子中取
		Connection connection = null;
		if (pool.size() > 0) {
			connection = pool.removeFirst();
		}else {
			//池子中没有连接了
			//先等待，等待超时之后，就新创建连接
			connection = DriverManager.getConnection(url, user, password);
		}
		return connection;
	}
	public int getCount(){
		return pool.size();
	}
	public void addBack(Connection conn){
		//如果是原本就在池子中的，就归还到池子中
		//如果是新创建的连接，就直接销毁
		pool.addLast(conn);
	}
}
```



### 1.4.2自定义连接池第二个版本

* 1. 自定义一个MyConnection类实现Connection接口,初始化连接池对象的时候往容器中存入n个MyConnection的对象。而新创建Connection的时候，则创建MySQL驱动中实现类的对象，这样就能区分拿个连接是新创建的或者是原本就在连接池中的了。
* 2. 在归还连接的时候，使用instance of 判断如果该对象是MyConnection类型则归还到连接池中，否则直接销毁。
* 3. 该版本缺点:初始化存放在连接池中MyConnection对象的方法都是空实现
* 4. 解决方法:将MySQL驱动中实现类的对象传入到MyConnection类中，去帮助MyConnection类实现方法

```
/**
 * 优化:区分原本就在池子中的连接和新创建的连接
 *
 */
public class MyConnectionPool2 {
	private LinkedList<Connection> pool = new LinkedList<>();
	private String password = "123";
	private String user = "root";
	private String url = "jdbc:mysql://localhost:3306/day09_1";
	static{
		//注册驱动
		try {
			Class.forName("com.mysql.jdbc.Driver");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	public MyConnectionPool2() throws SQLException{
		//初始化的时候，往容器中存放五个连接对象------>我们自定义的MyConnection的对象
		for(int i=0;i<5;i++){
			Connection connection = DriverManager.getConnection(url, user, password);
			Connection conn = new MyConnection(connection);
			pool.add(conn);
		}
	}
	public Connection getConnectionFromPool() throws SQLException{
		//从pool的头部拿连接
		//如果池子中有连接，就直接从池子中取
		Connection connection = null;
		if (pool.size() > 0) {
			connection = pool.removeFirst();
		}else {
			//池子中没有连接了
			//先等待，等待超时之后，就新创建连接
			connection = DriverManager.getConnection(url, user, password);
		}
		return connection;
	}
	public int getCount(){
		return pool.size();
	}
	public void addBack(Connection conn) throws SQLException{
		//如果是原本就在池子中的，就归还到池子中
		//如果是新创建的连接，就直接销毁
		//判断是原本就在池子中的还是新创建的
		if (conn instanceof MyConnection) {
			//原本就在池子中的
			pool.addLast(conn);
		}else {
			//新创建的，就直接销毁
			conn.close();
		}
	}
}
```

```
public class MyConnection implements Connection{
	@Override
	public void close() throws SQLException {
		
	}
	public MyConnection() {
	}
	@Override
	public <T> T unwrap(Class<T> iface) throws SQLException {
		return null;
	}

	@Override
	public boolean isWrapperFor(Class<?> iface) throws SQLException {
		return false;
	}

	@Override
	public Statement createStatement() throws SQLException {
		return null;
	}

	@Override
	public PreparedStatement prepareStatement(String sql) throws SQLException {
		return null;
	}

	......所有方法都是空实现
}
```



### 1.4.3自定义连接池第三个版本

* 1. 从MyConnection类的构造函数中传入MySQL驱动中Connection的实现类的对象。
* 2. MyConnection类中需要实现的方法都由传入的connection对象来进行完成。
* 3. 该版本的缺点:1.代码不够漂亮2.没有遵循sun公司的规则，即没有实现DataSource接口

```
public class MyConnection implements Connection{
	private Connection conn;
	public MyConnection(Connection conn) {
		this.conn = conn;
	}
	public MyConnection() {
	}
	@Override
	public <T> T unwrap(Class<T> iface) throws SQLException {
		return null;
	}

	@Override
	public boolean isWrapperFor(Class<?> iface) throws SQLException {
		return false;
	}

	@Override
	public Statement createStatement() throws SQLException {
		
		return conn.createStatement();
	}

	@Override
	public PreparedStatement prepareStatement(String sql) throws SQLException {
		return conn.prepareStatement(sql);
	}

	@Override
	public CallableStatement prepareCall(String sql) throws SQLException {
		return conn.prepareCall(sql);
	}
	......所有方法都由传入的connection对象实现
}
```



### 1.4.4自定义连接池第四个版本

1.1.1. 让连接池类实现DataSource接口，并实现所有方法

1.1.2. 重写MyConnection类的close()方法，让其对象调用close()方法的时候完成将该对象归还到连接池中去。

```
public class MyConnectionPool4 implements DataSource{
	private LinkedList<Connection> pool = new LinkedList<>();
	private String password = "123";
	private String user = "root";
	private String url = "jdbc:mysql://localhost:3306/day09_1";
	static{
		//注册驱动
		try {
			Class.forName("com.mysql.jdbc.Driver");
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	public MyConnectionPool4() throws SQLException{
		//初始化的时候，往容器中存放五个连接对象------>我们自定义的MyConnection的对象
		for(int i=0;i<5;i++){
			Connection connection = DriverManager.getConnection(url, user, password);
			Connection conn = new MyConnection(connection,pool);
			pool.add(conn);
		}
	}
	@Override
	public Connection getConnection() throws SQLException {
		//从pool的头部拿连接
		//如果池子中有连接，就直接从池子中取
		Connection connection = null;
		if (pool.size() > 0) {
			connection = pool.removeFirst();
		}else {
			//池子中没有连接了
			//先等待，等待超时之后，就新创建连接
			connection = DriverManager.getConnection(url, user, password);
		}
		return connection;
	}
	......其它方法都空实现
}
```

```
public class MyConnection implements Connection{
	private Connection conn;
	private LinkedList<Connection> pool;
	public MyConnection(Connection conn,LinkedList<Connection> pool) {
		this.conn = conn;
		this.pool = pool;
	}
	public MyConnection(Connection conn) {
		this.conn = conn;
	}
	@Override
	public void close() throws SQLException {
		//重写这个close()方法，让其在调用这个close()方法的时候，将当前这个调用的连接对象归还到池子中
		pool.addLast(this);
	}
	public MyConnection() {
	}
	@Override
	public <T> T unwrap(Class<T> iface) throws SQLException {
		return null;
	}

	@Override
	public boolean isWrapperFor(Class<?> iface) throws SQLException {
		return false;
	}

	@Override
	public Statement createStatement() throws SQLException {
		
		return conn.createStatement();
	}

	@Override
	public PreparedStatement prepareStatement(String sql) throws SQLException {
		return conn.prepareStatement(sql);
	}

	@Override
	public CallableStatement prepareCall(String sql) throws SQLException {
		return conn.prepareCall(sql);
	}
	......除了close()方法之外，其它方法都用传入的connection对象实现
}
```

## 1.5代理模式

### 1.5.1代理模式的概念

​    ProxyPattern（即：代理模式），23种常用的面向对象软件的设计模式之一

​    代理模式的定义：为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户和目标对象之间起到中介的作用。

​    意义：为某个对象提供一个代理，以控制对这个对象的访问。 代理类和委托类有共同的父类或父接口，这样在任何使用委托类对象的地方都可以用代理对象替代。也可以增强一个类中的某个方法.或者对程序进行扩展在调用核心方法之前做前置处理，调用完核心方法之后做后置处理。

### 1.5.2代理模式的组成部分

* 1. 代理接口:代理类和被委托类(被代理类)共同实现的接口
* 2. 委托类:被代理的类,用于处理核心业务
* 3. 代理类::负责对请求的预处理、过滤、将请求分派给委托类处理、以及委托类执行完请求后的后续处理。

### 1.5.3代理模式的分类

#### 1.5.3.1静态代理

* 概念:由程序员创建或工具生成代理类的源码，再编译代理类。所谓静态也就是在程序运行前就已经存在代理类的字节码文件，代理类和委托类的关系在运行前就确定了。
* 缺点:
  * 代理对象的一个接口只服务于一种类型的对象，如果要代理的方法很多，势必要为每一种方法都进行代理，静态代理在程序规模稍大时就无法胜任了。
  * 如果接口增加一个方法，除了所有委托类需要实现这个方法外，所有代理类也需要实现此方法。增加了代码维护的复杂度。

#### 1.5.3.2动态代理

* 概念:动态代理与静态代理模式原理是一样的，只是动态代理类的源码是在程序运行期间由JVM根据反射等机制动态的生成，所以不存在代理类的字节码文件。代理者和委托者的关系是在程序运行时确定。

* 与动态代理相关的API

  * java.lang.reflect.Proxy:这是 Java 动态代理机制生成的所有动态代理类的父类，它提供了一组静态方法来为一组接口动态地生成代理对象。

  ```
  Proxy.newProxyInstance(loader,interfaces,invacationHandler)
  ```

  * java.lang.reflect.InvocationHandler,这是调用处理器接口，它自定义了一个 invoke 方法，用于集中处理在动态代理类对象上的方法调用，通常在该方法中实现对委托类的代理访问。每次生成动态代理类对象时都要指定一个对应的调用处理器对象。

```
public class MyInvocationHandler implements InvocationHandler{
	//缺少了委托者对象
	private Object object;//委托者
	
	public MyInvocationHandler(Object object) {
		super();
		this.object = object;
	}
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		//每次代理者调用方法都会触发invoke()方法
		//proxy参数代表代理者本身,method参数代表代理者当前调用的方法,args代表代理者调用方法时传入的参数
		//invoke()方法的返回值就是代理者调用的方法的返回值
	}
}
```



# 第二章C3P0连接池

## 2.1 C3P0连接池简介
C3P0地址：https://sourceforge.net/projects/c3p0/?source=navbar
C3P0是一个开源的连接池。Hibernate框架，默认推荐使用C3P0作为连接池实现。
C3P0的jar包：`c3p0-0.9.1.2.jar`![](imgs\连接池04.png)

## 2.2 常用的配置参数解释
**常用的配置参数：**
| 参数              | 说明       |
| --------------- | -------- |
| initialPoolSize | 初始连接数    |
| maxPoolSize     | 最大连接数    |
| checkoutTimeout | 最大等待时间   |
| maxIdleTime     | 最大空闲回收时间 |

`初始连接数`：刚创建好连接池的时候准备的连接数量
`最大连接数`：连接池中最多可以放多少个连接
`最大等待时间`：连接池中没有连接时最长等待时间
`最大空闲回收时间`：连接池中的空闲连接多久没有使用就会回收

完整参数
![](imgs\连接池03.png)

## 2.3C3P0连接池基本使用
### 2.3.1 C3P0配置文件
​	我们看到要使用C3P0连接池，需要设置一些参数。那么这些参数怎么设置最为方便呢？使用配置文件方式。

**配置文件的要求：**
1. 文件名：c3p0-config.xml
2. 放在源代码即src目录下
3. 配置方式一：使用默认配置（default-config）
4. 配置方式二：使用命名配置（named-config）

配置文件c3p0-config.xml
```xml
<c3p0-config>
  <!-- 使用默认的配置读取连接池对象 -->
  <default-config>
  	<!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/day25</property>
    <property name="user">root</property>
    <property name="password">root</property>
    
    <!-- 连接池参数 -->
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">10</property>
    <property name="checkoutTimeout">2000</property>
    <property name="maxIdleTime">1000</property>
  </default-config>

  <named-config name="itheimac3p0"> 
    <!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/day25</property>
    <property name="user">root</property>
    <property name="password">root</property>
    
    <!-- 连接池参数 -->
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">15</property>
    <property name="checkoutTimeout">2000</property>
    <property name="maxIdleTime">1000</property>
  </named-config>
</c3p0-config>
```

### 2.3.2 API介绍
`com.mchange.v2.c3p0.ComboPooledDataSource`类表示C3P0的连接池对象，常用2种创建连接池的方式：`1.无参构造，使用默认配置`，`2.有参构造，使用命名配置`
1. ```java
   public ComboPooledDataSource()
   无参构造使用默认配置（使用xml中default-config标签中对应的参数）
   ```

2. ```java
   public ComboPooledDataSource(String configName)
   有参构造使用命名配置（configName：xml中配置的名称，使用xml中named-config标签中对应的参数）
   ```

3. ```java
   public Connection getConnection() throws SQLException
   从连接池中取出一个连接
   ```

### 2.3.3 使用步骤
1. 导入jar包`c3p0-0.9.1.2.jar`
2. 编写`c3p0-config.xml`配置文件，配置对应参数
3. 将配置文件放在src目录下
4. 创建连接池对象`ComboPooledDataSource`，使用默认配置或命名配置
5. 从连接池中获取连接对象
6. 使用连接对象操作数据库
7. 关闭资源

### 2.3.4 注意事项
C3P0配置文件名称必须为`c3p0-config.xml`
C3P0命名配置可以有多个

### 2.3.5 案例代码
1. 准备数据
   ```sql
   CREATE TABLE student (
      id INT PRIMARY KEY AUTO_INCREMENT,
      NAME VARCHAR(20),
      age INT,
      score DOUBLE DEFAULT 0.0
   );
   ```

2. 配置文件
   ```xml
    <c3p0-config>
      <!-- 使用默认的配置读取连接池对象 -->
      <default-config>
        <!--  连接参数 -->
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/day25</property>
        <property name="user">root</property>
        <property name="password">root</property>

        <!-- 连接池参数 -->
        <property name="initialPoolSize">5</property>
        <property name="maxPoolSize">10</property>
        <property name="checkoutTimeout">2000</property>
        <property name="maxIdleTime">1000</property>
      </default-config>

      <named-config name="itheimac3p0"> 
        <!--  连接参数 -->
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/day25</property>
        <property name="user">root</property>
        <property name="password">root</property>

        <!-- 连接池参数 -->
        <property name="initialPoolSize">5</property>
        <property name="maxPoolSize">15</property>
        <property name="checkoutTimeout">2000</property>
        <property name="maxIdleTime">1000</property>
      </named-config>
    </c3p0-config>
   ```

3. java代码
   ```java
    public class Demo01 {
        public static void main(String[] args) throws Exception {
            // 方式一: 使用默认配置（default-config）
            // new ComboPooledDataSource();
    //		 ComboPooledDataSource ds = new ComboPooledDataSource();

            // 方式二： 使用命名配置（named-config：配置名）
            // new ComboPooledDataSource("配置名");
            ComboPooledDataSource ds = new ComboPooledDataSource("itheimac3p0");

    //		for (int i = 0; i < 10; i++) {
    //			Connection conn = ds.getConnection();
    //			System.out.println(conn);
    //		}

            // 从连接池中取出连接
            Connection conn = ds.getConnection();

            // 执行SQL语句
            String sql = "INSERT INTO student VALUES (NULL, ?, ?, ?);";
            PreparedStatement pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, "张三");
            pstmt.setInt(2, 25);
            pstmt.setDouble(3, 99.5);

            int i = pstmt.executeUpdate();
            System.out.println("影响的行数： " + i);
            pstmt.close();
            conn.close(); // 将连接还回连接池中
        }
    }
   ```

### 2.3.6 案例效果
1. 正常获取连接池中连接

![](imgs\连接池06.png)

1. 获取连接池中连接超时

![](imgs\连接池07.png)

1. 使用连接池中的连接往数据库添加数据

![](imgs\连接池05.png)

### 2.3.7 总结
配置文件名称必须为：`c3p0-config.xml`，将配置文件放在src目录下
使用配置文件方式好处：只需要单独修改配置文件，不用修改代码
多个配置的好处：
1. 可以连接不同的数据库：db1,db2
2. 可以使用不同的连接池参数：maxPoolSize
3. 可以连接不同厂商的数据库：Oracle或MySQL


# 第三章 DRUID连接池
## 3.1 DRUID简介
​	Druid是阿里巴巴开发的号称为监控而生的数据库连接池，Druid是目前最好的数据库连接池。在功能、性能、扩展性方面，都超过其他数据库连接池，同时加入了日志监控，可以很好的监控DB池连接和SQL的执行情况。Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。Druid地址：https://github.com/alibaba/druid 
DRUID连接池使用的jar包：`druid-1.0.9.jar`![](imgs\连接池08.png)

## 3.2 DRUID常用的配置参数
**常用的配置参数：**
| 参数              | 说明                                       |
| --------------- | ---------------------------------------- |
| jdbcUrl         | 连接数据库的url：mysql : jdbc:mysql://localhost:3306/druid2 |
| username        | 数据库的用户名                                  |
| password        | 数据库的密码                                   |
| driverClassName | 驱动类名。根据url自动识别，这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName(建议配置下) |
| initialSize     | 初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时 |
| maxActive       | 最大连接池数量                                  |
| maxIdle         | 已经不再使用，配置了也没效果                           |
| minIdle         | 最小连接池数量                                  |
| maxWait         | 获取连接时最大等待时间，单位毫秒。                        |


## 3.3 DRUID连接池基本使用
### 3.3.1 API介绍
`com.alibaba.druid.pool.DruidDataSourceFactory`类有创建连接池的方法
```java
public static DataSource createDataSource(Properties properties)
创建一个连接池，连接池的参数使用properties中的数据
```

我们可以看到DRUID连接池在创建的时候需要一个Properties对象来设置参数，所以我们使用properties文件来保存对应的参数。
DRUID连接池的配置文件名称随便，建议放到src目录下面方便加载。
`druid.properties`文件内容：
```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/day25
username=root
password=root
initialSize=5
maxActive=10
maxWait=3000
maxIdle=6
minIdle=3
```

### 3.3.2 使用步骤
1. 在src目录下创建一个properties文件，并设置对应参数
2. 加载properties文件的内容到Properties对象中
3. 创建DRUID连接池，使用配置文件中的参数
4. 从DRUID连接池中取出连接
5. 执行SQL语句
6. 关闭资源

### 3.3.3 案例代码
1. 在src目录下新建一个DRUID配置文件，命名为：druid.properties，内容如下
```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/day25
username=root
password=root
initialSize=5
maxActive=10
maxWait=3000
maxIdle=6
minIdle=3
```

java代码
```java
public class Demo02 {
	public static void main(String[] args) throws Exception {
		// 加载配置文件中的配置参数
		InputStream is = Demo03.class.getResourceAsStream("/druid.properties");
		Properties pp = new Properties();
		pp.load(is);
		
		// 创建连接池，使用配置文件中的参数
		DataSource ds = DruidDataSourceFactory.createDataSource(pp);
		
//		for (int i = 0; i < 10; i++) {
//			Connection conn = ds.getConnection();
//			System.out.println(conn);
//		}
		
		// 从连接池中取出连接
		Connection conn = ds.getConnection();
		
		// 执行SQL语句
		String sql = "INSERT INTO student VALUES (NULL, ?, ?, ?);";
		PreparedStatement pstmt = conn.prepareStatement(sql);
		pstmt.setString(1, "王五");
		pstmt.setInt(2, 35);
		pstmt.setDouble(3, 88.5);
		
		int i = pstmt.executeUpdate();
		System.out.println("影响的行数： " + i);
		
		// 执行查询
		sql = "SELECT * FROM student;";
		ResultSet rs = pstmt.executeQuery(sql);
		
		while (rs.next()) {
			int id = rs.getInt("id");
			String name = rs.getString("name");
			int age = rs.getInt("age");
			double score = rs.getDouble("score");
			System.out.println("id: " + id + " ,name: " + name + " ,age = " + age + " ,score = " + score);
		}
		
		pstmt.close();
		conn.close(); // 将连接还回连接池中
	}
}
```

### 3.3.4 案例效果
1. 正常获取连接池中的连接
   ![](imgs\连接池09.png)
2. 获取连接池中的连接超时
   ![](imgs\连接池10.png)
3. 使用DRUID连接池中的连接操作数据库
   ![](imgs\连接池11.png)

### 3.3.5 总结
​	DRUID连接池根据Properties对象中的数据作为连接池参数去创建连接池，我们自己定义properties类型的配置文件，名称自己取，也可以放到其他路径，建议放到src目录下方便加载。
​	不管是C3P0连接池，还是DRUID连接池，配置大致都可以分为2种：`1.连接数据库的参数`，`2.连接池的参数`，这2种配置大致参数作用都相同，只是参数名称可能不一样。


## 3.4 Jdbc工具类
​	我们每次操作数据库都需要创建连接池，获取连接，关闭资源，都是重复的代码。我们可以将创建连接池和获取连接池的代码放到一个工具类中，简化代码。

**Jdbc工具类步骤：**
1. 声明静态数据源成员变量
2. 创建连接池对象
3. 定义公有的得到数据源的方法
4. 定义得到连接对象的方法
5. 定义关闭资源的方法

**案例代码**
JdbcUtils.java
```java
public class JdbcUtils {
	// 1.	声明静态数据源成员变量
	private static DataSource ds;

	// 2. 创建连接池对象
	static {
		// 加载配置文件中的数据
		InputStream is = JdbcUtils.class.getResourceAsStream("/druid.properties");
		Properties pp = new Properties();
		try {
			pp.load(is);
			// 创建连接池，使用配置文件中的参数
			ds = DruidDataSourceFactory.createDataSource(pp);
		} catch (IOException e) {
			e.printStackTrace();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	// 3. 定义公有的得到数据源的方法
	public static DataSource getDataSource() {
		return ds;
	}

	// 4. 定义得到连接对象的方法
	public static Connection getConnection() throws SQLException {
		return ds.getConnection();
	}

	// 5.定义关闭资源的方法
	public static void close(Connection conn, Statement stmt, ResultSet rs) {
		if (rs != null) {
			try {
				rs.close();
			} catch (SQLException e) {}
		}

		if (stmt != null) {
			try {
				stmt.close();
			} catch (SQLException e) {}
		}

		if (conn != null) {
			try {
				conn.close();
			} catch (SQLException e) {}
		}
	}

	// 6.重载关闭方法
	public static void close(Connection conn, Statement stmt) {
		close(conn, stmt, null);
	}
}
```

测试类代码
```java
public class Demo03 {
	public static void main(String[] args) throws Exception {
		// 拿到连接
		Connection conn = JdbcUtils.getConnection();

		// 执行sql语句
		String sql = "INSERT INTO student VALUES (NULL, ?, ?, ?);";
		PreparedStatement pstmt = conn.prepareStatement(sql);
		pstmt.setString(1, "李四");
		pstmt.setInt(2, 30);
		pstmt.setDouble(3, 50);
		int i = pstmt.executeUpdate();
		System.out.println("影响的函数: " + i);

		// 关闭资源
		JdbcUtils.close(conn, pstmt);
	}
}
```

> 小结：使用Jdbc工具类后可以简化代码，我们只需要写SQL去执行。
