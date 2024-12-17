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

