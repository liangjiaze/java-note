# 第一章JDBC元数据(了解)

## 2.1元数据的概述

包含了数据库、表、列的定义信息，关于数据库的整体综合信息。

## 2.2元数据的分类

#### 2.2.1DataBaseMetaData,数据库元数据

* getURL()：返回一个String类对象，代表数据库的URL。
* getUserName()：返回连接当前数据库管理系统的用户名。
* getDriverName()：返回驱动驱动程序的名称。

#### 2.2.2 ParameterMetaData,参数元数据

- getParameterCount()
  获得指定参数的个数

- getParameterType(int param)
  获得指定参数的sql类型

  > 并不是所有数据库都支持，使用MySQL时，这个方法只会返回varchar的int值（即14）

- getParameterTypeName(int param)  --- 参数类型名称

getParameterType异常处理

```
  Parameter metadata not available for the given statement
```

解决方法：url后面拼接参数

```
  ?generateSimpleParameterMetadata=true
```

demo：

```
  public void demo2() throws SQLException {
      Connection conn = JDBCUtils.getConnection();
      String sql = "select * from users where id = ? and password=?";
      PreparedStatement stmt = conn.prepareStatement(sql);
      // 通过ParameterMetaData 获得 ？ 相关信息
      ParameterMetaData parameterMetaData = stmt.getParameterMetaData();
      // 获得个数
      int count = parameterMetaData.getParameterCount();
      System.out.println(count);

      for (int i = 1; i <= count; i++) {
          // 该方法并不是所有数据库都支持 --- MySQL不支持（所有返回类型都是varchar）
          int type = parameterMetaData.getParameterType(i);
          System.out.println(type);
          System.out.println(parameterMetaData.getParameterTypeName(i));
      }
  }
```

#### 2.2.3ResultSetMetaData,结果集元数据

- getColumnCount()
  返回resultset对象的列数
- getColumnName(int column)
  获得指定列的名称
- getColumnTypeName(int column)
  获得指定列的类型

demo：

```
  public void demo3() throws SQLException {
      Connection conn = JDBCUtils.getConnection();
      String sql = "select * from users";
      PreparedStatement stmt = conn.prepareStatement(sql);

      ResultSet rs = stmt.executeQuery();
      // 获得结果集元数据
      ResultSetMetaData resultSetMetaData = rs.getMetaData();

      int count = resultSetMetaData.getColumnCount();
      // 打印table 第一行
      for (int i = 1; i <= count; i++) {
          System.out.print(resultSetMetaData.getColumnName(i) + "\t");
      }
      System.out.println();

      // 打印每列类型
      for (int i = 1; i <= count; i++) {
          System.out.print(resultSetMetaData.getColumnTypeName(i) + "\t");
      }
      System.out.println();

      // 打印table数据
      while (rs.next()) {
          for (int i = 1; i <= count; i++) {
              System.out.print(rs.getObject(i) + "\t");
          }
          System.out.println();
      }
  }
```

## 2.3使用元数据抽取JDBC工具类(增、删、改)

```
public class JDBCUtils {
	private DataSource dataSource;
	public JDBCUtils(DataSource dataSource) {
		super();
		this.dataSource = dataSource;
	}
	/**
	 * @param sql
	 * @param params
	 * @return
	 * @throws SQLException
	 */
	public int update(String sql,Object objects) throws SQLException{
		//1.注册驱动
		//2.获得连接对象
		Connection conn = dataSource.getConnection();
		//3.创建preparedStatement对象
		PreparedStatement stmt = conn.prepareStatement(sql);
		//获取到参数元数据
		ParameterMetaData parameterMetaData = stmt.getParameterMetaData();
          int count = parameterMetaData.getParameterCount();
          for (int i = 1; i <= count; i++) {
              stmt.setObject(i, objects[i - 1]);
          }
          stmt.executeUpdate();
		//5.关闭资源
		stmt.close();
		conn.close();
		return i;
	}
}
```



# 第二章 JdbcTemplate(最重要)

## 2.1 JdbcTemplate概念

​	JDBC已经能够满足大部分用户最基本的需求，但是在使用JDBC时，必须自己来管理数据库资源如：获取PreparedStatement，设置SQL语句参数，关闭连接等步骤。JdbcTemplate就是Spring对JDBC的封装，目的是使JDBC更加易于使用。JdbcTemplate是Spring的一部分。
	JdbcTemplate处理了资源的建立和释放。他帮助我们避免一些常见的错误，比如忘了总要关闭连接。他运行核心的JDBC工作流，如Statement的建立和执行，而我们只需要提供SQL语句和提取结果。
Spring源码地址：https://github.com/spring-projects/spring-framework
在JdbcTemplate中执行SQL语句的方法大致分为3类：

1. `execute`：可以执行所有SQL语句，但是这个方法没有返回值，一般用于执行DDL语句。(了解)
2. `update`：用于执行`INSERT`、`UPDATE`、`DELETE`等DML语句。(重点,简单)
3. `queryXxx`：用于DQL数据查询语句。（重点，难点）

## 3.2 JdbcTemplate使用过程

### 3.2.1 Druid基于配置文件实现连接池

#### 3.2.1.1 API介绍

`org.springframework.jdbc.core.JdbcTemplate`类方便执行SQL语句

1. ```java
   public JdbcTemplate(DataSource dataSource)
   创建JdbcTemplate对象，方便执行SQL语句
   ```

2. ```java
   public void execute(final String sql)
   execute可以执行所有SQL语句，因为没有返回值，一般用于执行DDL语句。
   ```

#### 3.2.1.2 使用步骤

1. 准备DruidDataSource连接池
2. 导入依赖的jar包
   - `spring-beans-4.1.2.RELEASE.jar`
   - `spring-core-4.1.2.RELEASE.jar`
   - `spring-jdbc-4.1.2.RELEASE.jar`
   - `spring-tx-4.1.2.RELEASE.jar`
   - `com.springsource.org.apache.commons.logging-1.1.1.jar`
     ![](imgs/连接池12.png)
3. 创建`JdbcTemplate`对象，传入`Druid`连接池
4. 调用`execute`、`update`、`queryXxx`等方法

#### 3.2.1.3 案例代码

```java
public class Demo04 {
	public static void main(String[] args) {
		// 创建表的SQL语句
		String sql = "CREATE TABLE product("
				+ "pid INT PRIMARY KEY AUTO_INCREMENT,"
				+ "pname VARCHAR(20),"
				+ "price DOUBLE"
				+ ");";
				
		JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
		jdbcTemplate.execute(sql);
	}
}
```

#### 3.2.1.4 案例效果

1. 代码效果
   ![](imgs/连接池13.png)
2. 执行SQL后创建数据库效果
   ![](imgs/连接池14.png)

## 3.3 JdbcTemplate实现增删改

#### 3.3.1 API介绍

`org.springframework.jdbc.core.JdbcTemplate`类方便执行SQL语句

1. ```java
   public int update(final String sql)
   用于执行`INSERT`、`UPDATE`、`DELETE`等DML语句。
   ```

#### 3.3.2 使用步骤

1.创建JdbcTemplate对象
2.编写SQL语句
3.使用JdbcTemplate对象的update方法进行增删改

#### 3.3.3 案例代码

```java
public class Demo05 {
	public static void main(String[] args) throws Exception {
//		test01();
//		test02();
//		test03();
	}
	
	// JDBCTemplate添加数据
	public static void test01() throws Exception {
		JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
		
		String sql = "INSERT INTO product VALUES (NULL, ?, ?);";
		
		jdbcTemplate.update(sql, "iPhone3GS", 3333);
		jdbcTemplate.update(sql, "iPhone4", 5000);
		jdbcTemplate.update(sql, "iPhone4S", 5001);
		jdbcTemplate.update(sql, "iPhone5", 5555);
		jdbcTemplate.update(sql, "iPhone5C", 3888);
		jdbcTemplate.update(sql, "iPhone5S", 5666);
		jdbcTemplate.update(sql, "iPhone6", 6666);
		jdbcTemplate.update(sql, "iPhone6S", 7000);
		jdbcTemplate.update(sql, "iPhone6SP", 7777);
		jdbcTemplate.update(sql, "iPhoneX", 8888);
	}
	
	
	// JDBCTemplate修改数据
	public static void test02() throws Exception {
		JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
		
		String sql = "UPDATE product SET pname=?, price=? WHERE pid=?;";
		
		int i = jdbcTemplate.update(sql, "XVIII", 18888, 10);
		System.out.println("影响的行数: " + i);
	}

	// JDBCTemplate删除数据
	public static void test03() throws Exception {
		JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
		String sql = "DELETE FROM product WHERE pid=?;";
		int i = jdbcTemplate.update(sql, 7);
		System.out.println("影响的行数: " + i);
	}
}
```

#### 3.3.4 案例效果

1. 增加数据效果
   ![](imgs/连接池15.png)
2. 修改数据效果
   ![](imgs/连接池16.png)
3. 删除数据效果
   ![](imgs/连接池17.png)

#### 3.3.5 总结

JdbcTemplate的`update`方法用于执行DML语句。同时还可以在SQL语句中使用？占位，在update方法的`Object... args`可变参数中传入对应的参数。

## 3.6 JdbcTemplate实现查询

`org.springframework.jdbc.core.JdbcTemplate`类方便执行SQL语句

### 3.6.1 queryForInt返回一个整数(了解)

#### 3.6.1.1 API介绍

1. ```java
   public int queryForInt(String sql)
   执行查询语句，返回一个int类型的值。比如查询pid等等一些int类型的字段值
   ```

#### 3.6.1.2 使用步骤

1. 创建JdbcTemplate对象
2. 编写查询的SQL语句
3. 使用JdbcTemplate对象的queryForInt方法
4. 输出结果

#### 3.6.1.3 案例代码

```java
// queryForInt返回一个整数
public static void test01() throws Exception {
   // String sql = "SELECT COUNT(*) FROM product;";
   String sql = "SELECT pid FROM product WHERE price=18888;";
   JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
   int forInt = jdbcTemplate.queryForInt(sql);
   System.out.println(forInt);
}
```

#### 3.6.1.4 案例效果

![](imgs/连接池19.png)

### 3.6.2 queryForLong返回一个long类型整数

#### 3.6.2.1 API介绍

1. ```java
   public long queryForLong(String sql)
   执行查询语句，返回一个long类型的数据。
   ```

#### 3.6.2.2 使用步骤

1. 创建JdbcTemplate对象
2. 编写查询的SQL语句
3. 使用JdbcTemplate对象的queryForLong方法
4. 输出结果

#### 3.6.2.3 案例代码

```java
// queryForLong  返回一个long类型整数
public static void test02() throws Exception {
   String sql = "SELECT COUNT(*) FROM product;";
   // String sql = "SELECT pid FROM product WHERE price=18888;";
   JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
   long forLong = jdbcTemplate.queryForLong(sql);
   System.out.println(forLong);
}
```

#### 3.6.2.4 案例效果

![](imgs/连接池20.png)

### 3.6.3 queryForObject返回String(了解)

#### 3.6.3.1 API介绍

1. ```java
   public <T> T queryForObject(String sql, Class<T> requiredType)
   执行查询语句，返回一个指定类型的数据。
   ```

#### 3.6.3.2 使用步骤

1. 创建JdbcTemplate对象
2. 编写查询的SQL语句
3. 使用JdbcTemplate对象的queryForObject方法，并传入需要返回的数据的类型
4. 输出结果

#### 3.6.3.3 案例代码

```java
public static void test03() throws Exception {
   String sql = "SELECT pname FROM product WHERE price=7777";
   JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
   String str = jdbcTemplate.queryForObject(sql, String.class);
   System.out.println(str);
}
```

#### 3.6.3.4 案例效果

![](imgs/连接池21.png)

### 3.6.4 queryForMap返回一个Map集合对象(掌握)

#### 3.6.4.1 API介绍

1. 

   ```java
   public Map<String, Object> queryForMap(String sql)
   执行查询语句，将一条记录放到一个Map中。这条数据的各个字段名作为map的key，各个字段的值作为map的value
   ```

#### 3.6.4.2 使用步骤

1. 创建JdbcTemplate对象
2. 编写查询的SQL语句
3. 使用JdbcTemplate对象的queryForMap方法
4. 处理结果

#### 3.6.4.3 案例代码

```java
public static void test04() throws Exception {
   String sql = "SELECT * FROM product WHERE pid=?;";
   JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
   Map<String, Object> map = jdbcTemplate.queryForMap(sql, 6);
   System.out.println(map);
}
```

#### 3.6.4.4 案例效果

![](imgs/连接池22.png)

### 3.6.5 queryForList返回一个List集合对象，集合对象存储Map类型数据

#### 3.6.5.1 API介绍

1. ```java
   public List<Map<String, Object>> queryForList(String sql)
   执行查询语句，返回一个List集合，List中存放的是Map类型的数据。
   ```

#### 3.6.5.2 使用步骤

1. 创建JdbcTemplate对象
2. 编写查询的SQL语句
3. 使用JdbcTemplate对象的queryForList方法
4. 处理结果

#### 3.6.5.3 案例代码

```java
public static void test05() throws Exception {
   String sql = "SELECT * FROM product WHERE pid<?;";
   JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());
   List<Map<String, Object>> list = jdbcTemplate.queryForList(sql, 8);
   for (Map<String, Object> map : list) {
      System.out.println(map);
   }
}
```

#### 3.6.5.4 案例效果

![](imgs/连接池23.png)

### 3.6.6 query使用RowMapper做映射返回对象

#### 3.6.6.1 API介绍

1. ```java
   public <T> List<T> query(String sql, RowMapper<T> rowMapper)
   执行查询语句，返回一个List集合，List中存放的是RowMapper指定类型的数据。
   ```

#### 3.6.6.2 使用步骤

1. 定义Product类
2. 创建JdbcTemplate对象
3. 编写查询的SQL语句
4. 使用JdbcTemplate对象的query方法，并传入RowMapper匿名内部类
5. 在匿名内部类中将结果集中的一行记录转成一个Product对象

#### 3.6.6.3 案例代码

```java
// query使用rowMap做映射返回一个对象
public static void test06() throws Exception {
    JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());

   // 查询数据的SQL语句
   String sql = "SELECT * FROM product;";

   List<Product> query = jdbcTemplate.query(sql, new RowMapper<Product>() {
      @Override
      public Product mapRow(ResultSet arg0, int arg1) throws SQLException {
         Product p = new Product();
         p.setPid(arg0.getInt("pid"));
         p.setPname(arg0.getString("pname"));
         p.setPrice(arg0.getDouble("price"));
         return p;
      }
   });

   for (Product product : query) {
      System.out.println(product);
   }
}
```

#### 3.6.6.4 案例效果

![](imgs/连接池18.png)

### 3.6.7 query使用BeanPropertyRowMapper做映射返回对象

#### 3.6.7.1 API介绍

1. ```java
   public <T> List<T> query(String sql, RowMapper<T> rowMapper)
   执行查询语句，返回一个List集合，List中存放的是RowMapper指定类型的数据。
   ```

2. ```java
   public class BeanPropertyRowMapper<T> implements RowMapper<T>
   BeanPropertyRowMapper类实现了RowMapper接口
   ```

#### 3.6.7.2 使用步骤

1. 定义Product类
2. 创建JdbcTemplate对象
3. 编写查询的SQL语句
4. 使用JdbcTemplate对象的query方法，并传入BeanPropertyRowMapper对象

#### 3.6.7.3 案例代码

```java
	// query使用BeanPropertyRowMapper做映射返回对象
	public static void test07() throws Exception {
		JdbcTemplate jdbcTemplate = new JdbcTemplate(JdbcUtils.getDataSource());

		// 查询数据的SQL语句
		String sql = "SELECT * FROM product;";
		List<Product> list = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(Product.class));

		for (Product product : list) {
			System.out.println(product);
		}
	}
```

#### 3.6.6.4 案例效果

![](imgs/连接池18.png)

### 3.6.8 总结

JDBCTemplate的`query`方法用于执行SQL语句，简化JDBC的代码。同时还可以在SQL语句中使用`？`占位，在`query`方法的`Object... args`可变参数中传入对应的参数。
