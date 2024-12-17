# CookieShop
Before developing the functional modules, CookieShop needs to set up the project development environment. The environment setup includes the following 7 steps:

### 1. Determine the Project Development Environment
- **Operating System**: Windows 7, Windows 10, or any higher version of Windows
- **Web Server**: Tomcat 8.5
- **Java Development Kit**: JDK 1.8
- **Browser**: Google Chrome
- **Development Tool**: IntelliJ IDEA 2019.3
- **Database**: MySQL 5.7

### 2. Create Database Tables
Create a database named `cookieshop` in MySQL and create the corresponding tables according to the table structure in the `cookieshop` database.

### 3. Create Project and Import JAR Packages
In IntelliJ IDEA, create a project named `CookieShop` as a Web Application. Import the necessary JAR packages into the `WEB-INF/lib` folder of the project.

### 4. Database Connection Pool
This project uses the **c3p0** database connection pool to connect to the database. You need to import the **c3p0** connection pool JAR package.

### 5. Utility Packages
This project uses **DBUtils** for handling data persistence operations, so you need to import the **DBUtils** utility package.

### 6. Configure c3p0-config.xml File
After importing the JAR packages and deploying them to the classpath, create a `c3p0-config.xml` file under the `src` directory to configure the database connection parameters.

```xml
<c3p0-config>
   <default-config>
      <property name="driverClass">com.mysql.cj.jdbc.Driver</property>
      <property name="jdbcUrl">
         jdbc:mysql://localhost:3306/cookieshop?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
      </property>
      <property name="user">root</property>
      <property name="password">root</property>
   </default-config>
</c3p0-config>
```

### 7. Create Filter for Character Encoding
To prevent request and response encoding issues, create a filter named `EncodeFilter` under the `filter` package in the `src` directory. This filter will ensure that the entire website uses UTF-8 encoding to avoid character encoding issues.

```java
@WebFilter(filterName = "EncodeFilter", urlPatterns = "/*")
public class EncodeFilter implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
            throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        chain.doFilter(req, resp);
    }
}
```

### 8. Create DBUtils Utility Class
Create a class named `DataSourceUtils` in the `utils` package under the `src` directory. This class will be responsible for obtaining the data source and database connections.

```java
public class DataSourceUtils {
    private static DataSource ds = new ComboPooledDataSource();

    public static DataSource getDataSource() {
        return ds;
    }

    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }
}
```

### 9. Create PriceUtil Utility Class
Create a class named `PriceUtil` in the `utils` package. This class will be used to calculate the total amount for the shopping cart and orders.
在开发功能模块之前，需要先进行项目开发环境的搭建工作，项目开发环境的搭建包括以下7个步骤。

确定项目开发环境
操作系统:Windows7、Windows10或更高的Windows版本
Web服务器:Tomcat8.5
Java开发包:JDK 1.8
浏览器:谷歌浏览器
开发工具:IntelliJ IDEA 2019.3
数据库:MySQL 5.7

创建数据库表
在MySQL数据库中创建一个名称为cookieshop的数据库，并根据表结构在cookieshop数据库中创建相应的表。

创建项目，引入JAR包
在IntelliJ IDEA中创建一个名称为CookieShop的Web Application，将项目所需JAR包导入到项目的WEB-INF/lib文件夹下。
数据库连接池
本项目使用c3p0数据库连接池连接数据库，需要c3p0的数据库连接池JAR包
工具包
本项目使用DBUtils工具处理数据的持久化操作，需要导入DBUtils工具包

配置c3p0-config.xml文件
将JAR包导入到项目中并发布到类路径后，在src根目录下新建c3p0-config.xml文件，用于配置数据库连接参数。
<c3p0-config>
   <default-config>
      <property name="driverClass">com.mysql.cj.jdbc.Driver</property> 
      <property name="jdbcUrl">
          jdbc:mysql://localhost:3306/cookieshop?serverTimezone=UTC&amp;
           useUnicode=true&amp;characterEncoding=utf-8</property>
      <property name="user">root</property>
      <property name="password">root</property>
   </default-config>
</c3p0-config>

编写Filter过滤器
为防止项目中请求和响应出现乱码情况，在src下创建一个名称为filter的包，在该包下新建一个过滤器EncodeFilter类来统一全站的编码，防止出现乱码的情况。
@WebFilter(filterName = "EncodeFilter",urlPatterns = "/*")
public class EncodeFilter implements Filter {
    …
    public void doFilter(ServletRequest req, ServletResponse resp,
       FilterChain chain) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        chain.doFilter(req, resp);
   }
    …
}

编写DBUtils工具类
在src下创建一个名称为utils的包，在该包下新建DataSourceUtils类，用于获取数据源和数据库连接。
public class DataSourceUtils {
    private static DataSource ds=new ComboPooledDataSource();
    public static DataSource getDataSource(){
        return  ds;
    }
	public static Connection getConnection() throws SQLException {
   	   return ds.getConnection();
    }
}

编写PriceUtil工具类
在utils包下创建PriceUtil类，用于对购物车以及订单金额进行计算。


