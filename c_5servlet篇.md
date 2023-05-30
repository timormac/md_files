## servlet

servlet是运行在服务器是那个的小型java程序，用来接受和响应客户端请求







#### 问题

1   servlet接口没有

javax.[servlet](https://so.csdn.net/so/search?q=servlet&spm=1001.2101.3001.7020)不存在或未找到，程序包javax.servlet.http不存在或未定义等错误。原因是servlet和JSP均不是java平台javase（标准版）的一部分，而是JavaEE的一部分，因此必须告知编译器servlet的位置。



解决方法:从tomacat的lib中拷贝出servelet-api.jar ,在idea的project structure中的lib导入，这样代码就是导入了