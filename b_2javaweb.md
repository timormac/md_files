## javaweb

#### dao理念

DAO (DataAccessobjects 数据存取对象) 是指位于业务逻辑和持久化数据之间实现对持久化数据的访问。通俗来讲，就是将数据库操作都封装起来。

DAO模式是一种结构模式，它允许我们使用抽象API将应用程序/业务层与持久层（通常是关系数据库，但它可以是任何其他持久性机制）隔离开来。

其实际为一个为数据库或其他持久化机制提供了抽象接口的对象，在不暴露底层持久化方案实现细节的前提下提供了各种数据访问操作。在实际的开发中，应该将所有对数据源的访问操作进行抽象化后封装在一个公共API中。用程序设计语言来说，就是建立一个接口，接口中定义了此应用程序中将会用到的所有事务方法。在这个应用程序中，当需要和数据源进行交互的时候则使用这个接口，并且编写一个单独的类来实现这个接口，在逻辑上该类对应一个特定的数据存储。

案例

```java
//接口
public interface userDao {
    // 查询所有用户
    List<User> findAllUsers() throws Exception;
}

//实现类
public class userDaoImpl 
    extends BaseDao 
    implements userDao {
    // 查询所有用户
    public List<User> findAllUsers() throws Exception {
        Connection conn=BaseDao.getConnection();
        String sql="select * from user";
        PreparedStatement stmt= conn.prepareStatement(sql);
        ResultSet rs = stmt.executeQuery();
        List<User> userList = new ArrayList<User>();
        while(rs.next()) {
            User user = new User(
                    rs.getInt("id"),
                    rs.getString("name"),
                    rs.getDate("birthday")
                    );
                userList.add(pet);
        }
        BaseDao.closeAll(conn, stmt, rs);
        return userList;
    }
}

实体类
public class User {
    private Integer id;
    private String name;
    private Date birthday;
 
    // 以下set和get方法就不写了
}
```



#### idea中创建web工程

课程里讲的那种右键的方式，我这边用不了。

先在原来的项目中，创建一个java的moudule,然后邮件module ,add framework support ,

选中web application 4.0 之后工程就有web特殊目录了，然后在Web-INF下手动建立一个lib。



#### idea中用tomcat运行项目

![](/Users/timor/Desktop/近期准备/md图片/add_tomcat.png)

点击箭头，然后添加tomcat,然后点击平时的运行，就是运行tomcat了



## tomcat

#### 目录层级

bin  conf  lib  logs  

temp(运行时残生的临时数据)

webapps 用来存放部署的web工程

work  tomcat工作时的目录，用来存放运行jsp翻译为servlet的源码



#### conf文件配置

1   在conf下的server.xml文件  ，有个<connector port="8080" 修改8080的默认端口号



#### bin下指令

catalina  run 启动tomcat



#### web部署

在conf/catalina/localhost 下创建一个timor.xml ，里面写



#### idea配置tomcat

settings中，application sever 中，点击加号指定tomcat路径