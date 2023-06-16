# java篇

## 问题待解决

1  看一些源码的时候，有时候需要传入一个借口，实现某个方法，比如flink的自定义map方法，有个问题，就是我现在想知道，这个传入的map方法，到底是谁在哪一步开始调用的，现在一直没头绪



## 问题解决记录



## 代码原则

详情请看csdn收藏的代码优化6大原则

对有代码，要有拓展性，如果需要添加新的功能，不能具体到去用到的代码每一处去修改，这种要提取出来，弄合适的设计模式

比如获取一个解析器，有json解析器，xml解析器，需要if,else来判断文件格式来获取解析器，这样代码看上去不简洁，并且如果要是有新的解析类型那么，需要在所有这段代码里都要添加一个新的if-else，不符合对拓展开放的原则。



## java脚本指令

```
java -cp  a.jar   com.cbk.Test  java运行指定jar包中的类
```



## 父类设置无参构造器

测试过，如果父类没有无参构造器，那么子类继承的时候，必须自己创建一个构造器，不然报错显示父类没有默认构造器。

所以为什么代码里总有一个无参构造器多此一举，或者子类手动显式写一个和父类一样的构造器，调super(a,b)。

## 修饰符private

场景1，有些属性 设置为private属性，这样这些属性赋值的时候需要使用getter和setter方法，因为有的赋值可能满足类型范围，但是不满足需求范围，可以在getter和setter中做条件处理



private修饰的属性和方法只能在本类中使用，在子类用调用不了父类的private修饰的方法。



## 修饰符final

final修饰的类不能被继承，修饰的方法不能被重写。 被修饰的变量一旦被赋值不能改变



## 接口interface

个人理解，在一些初始代码中，代码执行的范型 就是接口，然后后续不管后面新建了什么class，都会转为接口然后调用对应方法。

因为是父子关系所以不用转，直接就能调用方法。

对有多实现的接口，这样互相不影响，各自用自己的方法。

3.为什么看不懂代码，因为看到的代码都是调用接口的方法，而没有找到对应的实现类，找不到真正的执行原码。



## 包的命名

包的命名 一般都是com.caibeike.项目名称.模块名称

这样命名的原因好像是，这样不同项目的jar包，最后能在同一个目录下,在mvn中artifact是项目名字，groupid是公司名字，

当你创建mvn项目的时候根据grouid，默认创建的包层级就是groupid，这样当把项目打成jar包的时候，在仓库里也是一个目录下的

```xml
  <groupId>com.cbk.timor</groupId>
  <artifactId>learning_maven_origin</artifactId>
  <version>1.0</version>
```



## trycatch的使用场景

代码中总有数据库中读取的数据，然后你转化为array的，当你去读取然后取执行方法时，可能array中是空的，所以这时候应该执行try,catch 出现了空指针异常，执行处理，而不是让程序终止。

常见的类似需要try catch的，空指针，文件读取不存在，数据角标越界



## 抽象类abstract

## 多态

主要应用就是一些方法中定义数据类型是父类,可以接受子类



## 接口(新特性)

接口和抽象类的主要区别就是，1 接口可以多继承，类不行   2接口只有抽象方法，抽象类可有实现方法

接口多继承例子，fly接口，usb接口 如果你新的类要具有飞行和连接usb，必须实现上面2个借口



新特性：

接口只能extends 多个其他接口，但是不能implements其他接口

接口可以继承接口，可以继承多个接口，并且接口中可以有实现方法和静态方法

接口不能直接new对象，不过在看一些源码中,有的接口里有一些静态方法可以获得接口的实现对象

例如在flink中的WatermarkStrategy接口，继承了extends TimestampAssignerSupplier, WatermarkGeneratorSupplier

并且很多静态方法中的，返回值就是WatermarkStrategy的实现对象 





## 注解

@override注解，如果子类方法没有覆盖超类的方法，那么编译的时候能检验出来。

@Deprecated

​	用于表示被标记的数据已经过时，不建议使用。

​	可以用于修饰 属性、方法、构造、类、包、局部变量、参数。

​	它会被编译器程序读取。





在 Java 里注解有许多用途，可以归纳为三类：

编译检查：通过代码里标识的元数据让编译器能实现基本的编译检查，编译器可以使用注解来检测错误或抑制警告。
编译时和部署时的处理：程序可以处理注解信息以生成代码，XML 文件等。
运行时处理：可以在运行时检查某些注解并处理。
作为 Java 程序员，尤其是编程十几年年的老手，多多少少都曾经历过被各种配置文件（xml、properties）支配的恐惧。过多的配置文件会使得项目难以维护。使用注解可以减少配置文件或代码，是注解最大的用处，现在 Spring 家族的 SpringBoot 就是靠注解维护各种 Bean 组件的，让开发中者不再用XML指定各种Java Bean 的路径、名称等属性，减少了不少项目配置的步骤，从而让Java项目的开发提速了不少



## 泛型应用场景

应用场景，范型类，范型接口，范型方法，集合中用范型, 范型不是真正的限制类型，而是编译的时候检查

集合中使用范型

```java
//集合中不使用范型，默认是Object类型，用范型可以限制集合中类型
List<String>  list = new ArrayList<String>()
```

类范型, 范型方法

```java
public class ClassMethodGenericType<T> {

    T  t;

    void add(T t){}

    T get(){ return null;}

    //方法范型，这里范型适用范围只能在本方法里
    public <W> void  sort(W w){  }

    <M> Man<M> getMan(){ return new Man(); }
  
    //测试
    public static void main(String[] args) {

        ClassMethodGenericType<Object> obj = new ClassMethodGenericType<>();
        //范型方法指定范型在方法前面写
        Man<String> man = obj.<String>getMan();
    }

}
```

集合类型不继承多态用范型解决

```java
public class Test {
  //func方法参数为List<Person>,虽然Man是Person的子类，但是配合List就无法形成多态
	 void func( List<Person> list ) {} 
	 void func( List<Man> list ) {} 
}

//解决方法
public class Test2 {
  //这种可以多态形式接List<Person所有子类>，如果把 extends Person去掉,相当于Object
	 void func( List<? extends Person> list ) {} 
  
}

```



## enum枚举类场景

状态码

这样看枚举直接能看到不同状态码对应的错误信息，，不然你找不到哪些码对应哪些信息，如果不用枚举，你只能写注释或者文档。

```java
public class EnumTest {
    public static void main(String[] args) {
        ErrorCodeEnum errorCode = ErrorCodeEnum.SUCCESS;
        System.out.println("状态码：" + errorCode.code() + 
                           " 状态信息：" + errorCode.msg());
    }
}
enum ErrorCodeEnum {
    SUCCESS(1000, "success"),
    PARAM_ERROR(1001, "parameter error"),
    SYS_ERROR(1003, "system error"),
    NAMESPACE_NOT_FOUND(2001, "namespace not found"),
    NODE_NOT_EXIST(3002, "node not exist"),
    NODE_ALREADY_EXIST(3003, "node already exist"),
    UNKNOWN_ERROR(9999, "unknown error");
 
    private int code;
    private String msg;
 
    ErrorCodeEnum(int code, String msg) {
        this.code = code;
        this.msg = msg;
    }
 
    public int code() {
        return code;
    }
 
    public String msg() {
        return msg;
    }
 
  	//ErrorCodeEnum.values() 获取枚举类中所有枚举实例
    public static ErrorCodeEnum getErrorCode(int code) {
        for (ErrorCodeEnum it : ErrorCodeEnum.values()) {
            if (it.code() == code) {
                return it;
            }
        }
        return UNKNOWN_ERROR;
    }
}
```

代码可变化性

这里代码写死了，使用"BLUE"去做判断条件，但是如果后续有改动把COLOR_BLUE枚举类的值改成别的了，那么后续产生的数据，传给color，这里代码就过不了，这里应该用枚举来做判断条件。

对于有的判断条件，比如产品说消费多少评价为高等会员，给一个优惠，这种值要用枚举，不能固定写死。如果多处代码都用到这个值，你只能去找出来一个个改。

```java

public class EnumTest {
    public static final String COLOR_RED = "RED";
    public static final String COLOR_BLUE = "BLUE";
    public static final String COLOR_GREEN = "GREEN";
    public static void main(String[] args) {
        String color = "BLUE";
        if ("BLUE".equals(color)) {
            System.out.println("蓝色");
        }
    }

```

枚举实现单例

因为枚举只会在类加载时装载一次，所以它是线程安全的，这也是《Effective Java》作者极力推荐使用枚举来实现单例的主要原因

```java
public class Singleton {
    // 枚举类型是线程安全的，并且只会装载一次
    private enum SingletonEnum {
        INSTANCE;
        // 声明单例对象
        private final Singleton instance;
        // 实例化
        SingletonEnum() {
            instance = new Singleton();
        }
        private Singleton getInstance() {
            return instance;
        }
    }
    // 获取实例（单例对象）
    public static Singleton getInstance() {
        return SingletonEnum.INSTANCE.getInstance();
    }
    private Singleton() {
    }
    // 类方法
    public void sayHi() {
        System.out.println("Hi,Java.");
    }
}
class SingletonTest {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        singleton.sayHi();
    }
}
```



## 设计模式应用场景

#### 单例模式

饿汉

适用于项目中频繁获取对象的场景，例如：获取缓存对象、获取一些工具类对象等等，由于这些对象使用频率较高，所以在获取对象时，我们使用单例模式指定获取一个对象，不然每次使用就new 1次

```java

public class SignletonHungry {
 
    //1. 私有的静态的最终的对象
    private static final SignletonHungry singl=new SignletonHungry();
 
    //2. 私有的无参构造函数
    private SignletonHungry(){
 
    }
 
    //3. 公共的静态的实例方法
    public static SignletonHungry getInstance(){
        return singl;
    }
 
    //测试方法
    public static void main(String[] args) {
        //利用for循环 模拟多线程环境调用
        for (int i = 0; i < 100; i++) {
            new Thread(()->{
                //看每次获取对象的hashcode是否一致 判断是否获取了同一个对象
                System.out.println("获取的hashCode是： "+SignletonHungry.getInstance().hashCode());
            }).start();
        }
    }
}
```

**优点：**这种写法比较简单，就是在类装载的时候就完成实例化，避免了线程同步问题。

**缺点：**但是因为在指定对象时就进行初始化，在类比较大的时候，也会造成一定的资源消耗。

懒汉

比饿汉节省空间，只有用到了才会创建，但是有线程安全问题，

懒汉模式在多线程并发获取单例类时，存在现场安全的问题，那么既然存在线程安全问题，我们怎么去改善这个问题呢？请看[线程锁](https://so.csdn.net/so/search?q=线程锁&spm=1001.2101.3001.7020)模式。

```java
public class SignletonFull {
 
    //1. 私有的静态的对象 先不new 默认为null值
    private static SignletonFull signletonFull;
 
    //2. 私有的无参构造器
    private SignletonFull(){}
 
    //3. 公共的静态的方法
    public static SignletonFull getInstance() throws InterruptedException {
        if(signletonFull==null){
            Thread.sleep(1000);
            signletonFull=new SignletonFull();
        }
        return signletonFull;
    }
 
}
```

其他模式

还有线程锁模式，双重判断模式，静态内部类模式，先不学

#### 工厂模式

应用场景：有时候创建一个对象需要很多逻辑判断，比如获取一个解析器，有json解析器，xml解析器，需要if,else来判断文件格式来获取解析器，这样代码看上去不简洁，并且如果要是有新的解析类型那么，需要在所有这段代码里都要添加一个新的if-else，不符合对拓展开放的原则，这时候可以用工厂模式

```java
//获取解析器接口
public interface ITypeParser {
    void parse(String text);
}

//json解析器
public class JsonTypeParser implements ITypeParser {
    @Override
    public void parse(String text) {
        System.out.println("jsonParser");
    }
}

//xml解析器
public class XmlTypeParser implements ITypeParser {
    @Override
    public void parse(String text) {
        System.out.println("xmlParser");
    }
}


//如果不用工厂模式，获取解析器需要这样
public void test1(){
    String fileExtension = "json"
    ITypeParser parser = null;
    if("json".equalsIgnoreCase(fileExtension)){
        parser = new JsonTypeParser();
    }else if("xml".equalsIgnoreCase(fileExtension)){
        parser = new XmlTypeParser();
    }
}

//工厂模式 获取解析器的在执行代码上更简洁，并且增加新的解析，只需要修改工厂的代码，不需要修改执行代码
public class TypeParserFactory {
    private static final Map<String, ITypeParser> cacheParsers = new HashMap<>();

    static {
        cacheParsers.put("json",new JsonTypeParser());
        cacheParsers.put("xml",new XmlTypeParser());
    }

    public static ITypeParser getTypeParser(String fileExtension){
        return cacheParsers.get(fileExtension);
    }
}

```



#### 观察者模式

　　观察者模式，又叫做通知模式，是一种一对多的模式，因此观察者模式也叫做发布订阅模式。

　　在软件开发中，比如我们的产品有这样一个功能，用户下单支付成功之后，就会发送一条短信通知用户，如果之后希望不仅发送短信，还需要发送邮件，还需要语音通知，在这样的情况下，我们就可以采用观察者模式，我们将支付成功信息放入到消息队列中，至于发短信还是发邮件，由各个业务模块订阅消息队列自己处理。这样在订单模块里面，就不需要一个个通知短信模块，邮件模块了。



# java常用类库

#### 日期库

```java
import org.apache.commons.lang3.time.DateFormatUtils;

DateFormatUtils.format(ms,"yyyy-MM-dd HH:mm:ss.SSS")
```

#### json库





# 源码解读习惯

1 有一些代码,做一些判断之类的，但是并没有返回值什么的，比如说对于一个传入的集合不能为空,然后才能进行后续操作，

源码里后续操作并不是通过if判断是否为空来执行的，而是做一个判空操作，若为空直接throw异常，这样后续不会执行

例子:一些操作没有返回值，发挥作用是用throw异常

```java
    public <OUT> DataStreamSource<OUT> fromCollection(
            Collection<OUT> data, TypeInformation<OUT> typeInfo) {
      	
        //这个操作是用来判断集合为空的，点进去看是做判断和throw异常
        Preconditions.checkNotNull(data, "Collection must not be null");

        //这个操作是判断元素为同类型，并且没有null元素，点进去看是做判断和throw异常
        FromElementsFunction.checkCollection(data, typeInfo.getTypeClass());

        SourceFunction<OUT> function = new FromElementsFunction<>(data);
        return addSource(function, "Collection Source", typeInfo, Boundedness.BOUNDED)
                .setParallelism(1);
    }
```

