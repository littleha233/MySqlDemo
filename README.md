# MySqlDemo
Basic operations for MySQL by Java
## 使用java连接操作MySQL数据库



#### 1. 在MySQL中创建数据库、创建表

```sql
# 创建数据库chain
mysql> create database chain;
Query OK, 1 row affected (0.00 sec)

# 切换使用数据库chain
mysql> use chain
Database changed

# 查看当前使用的数据库
mysql> select database();
+------------+
| database() |
+------------+
| chain      |
+------------+
1 row in set (0.00 sec)

# 在数据库chain中创建表account [address(key), balance]
mysql> create table account (address varchar(40), balance int, primary key(address));
Query OK, 0 rows affected (0.01 sec)
```

***这样创建了一个chain的数据库，其中有一个表account；***

#### 2. 在DataGrip中插入数据、执行SQL命令

- 图形化界面在表中插入、删除数据
- 在console中执行SQL命令 https://zhuanlan.zhihu.com/p/391363536

#### 3. 使用java连接MySQL并操作数据库

1. 导入MySQL依赖，在pom.xml中添加

   ```xml
   <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.28</version>
           </dependency>
   ```

2. 数据库全局配置代码

   ```java
   // MySQL 8.0 以上版本 - JDBC 驱动名及数据库 URL
       static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
       static final String DB_URL = "jdbc:mysql://localhost:3306/chain?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC";
   
   
       // 数据库的用户名与密码，需要根据自己的设置
       static final String USER = "root";
       static final String PASS = "littleha233";
   ```

3. 连接数据库代码

   ```java
   		Connection conn = null;
       Statement stmt = null;
       try{    
       	// 注册 JDBC 驱动
         Class.forName(JDBC_DRIVER);
         // 打开链接
         System.out.println("连接数据库...");
         conn = DriverManager.getConnection(DB_URL,USER,PASS);
   ```

4. 查询数据库（使用到了SQL语句，CRUD操作体现在了SQL语句中）

   ```java
   		// 执行查询
       System.out.println("init statement ...");
       stmt = conn.createStatement();
   		// SQL语句执行查询操作
       String sql;
       sql = "SELECT address, balance FROM account";
       ResultSet rs = stmt.executeQuery(sql);
   
       // 展开结果集数据库
       while(rs.next()){
           // 通过字段检索
           String address = rs.getString("address");
           int balance = rs.getInt("balance");
   
           // 输出数据
           System.out.println("address: "+address +" balance: "+balance);
       }
       // 完成后关闭
       rs.close();
       stmt.close();
       conn.close();
       }catch(SQLException se){
       // 处理 JDBC 错误
       se.printStackTrace();
       }catch(Exception e){
       // 处理 Class.forName 错误
       e.printStackTrace();
       }finally{
       // 关闭资源
       try{
           if(stmt!=null) stmt.close();
       }catch(SQLException se2){
       }// 什么都不做
       try{
           if(conn!=null) conn.close();
       }catch(SQLException se){
           se.printStackTrace();
       }
   ```

   

   
