

# Scala

### 环境准备

安装scala

scala安装包解压，然后配置环境变量  vim ~/.bash_profile    export PATH=$PATH:scala/bin



idea配置scala

settings=>plugins=>搜索scala下载=>project structure =>Global libarys=>点击加号添加scala依赖 =>pom文件添加依赖

```
    <dependencies>
        <dependency>
            <groupId>org.scala-lang</groupId>
            <artifactId>scala-library</artifactId>
            <version>2.12.11</version>
        </dependency>
    </dependencies>
```

之后新建java文件时，就能选择scala文件了。其实也不用pom依赖，上面pom依赖之后主要是在project structure的module中增加了

scala支持，直接右键项目目录，选择add framwork support 添加scala既可以



常用关键字

•  trait, extends, with, type, for

•  private, protected, abstract, sealed, final, implicit, lazy, override

•  try, catch, finally, throw 

•  if, else, match, case, do, while, for, return, yield

•  def, val, var 

•  this, super

•  new

•  true, false, null



### 对象创建

main方法:  scala 可以直接创建对象，对象才能调main方法,为了表明完全面对对象,所以只有对象的方法才能调用

```java
class Ajava{
  public static void main(String[] args ){}
}
```



scala基于java也没法直接凭空创建对象,所以创建一个object文件，scala会底层创建一个伴生类

```scala
object Ascala{
  def main (args :Array[String]):Unit={
    
    var name:String = "aa"
    var age:Int = 6
    var id:Long = 0L
    
    //自动类型推断可以不加类型
    var name="a"
    
    //var是声明变量，val声明的是值不可变，和java的final相同
    val  age = 20  
    
   //字符串重的替换符,必须在""前加s
     println(s"我的名字${name1}")
  
  }
}
```

变量类型区别 java中有int,double基本数据类型，想调一些Interger方法要临时转换，但是scala中所有的都是引用类型，只有Int

声明变量区别  String name="a" =>   var name:String ="a"

方法参数区别  String str   =>  str:String   

方法返回值区别 void  => :Unit    ,String  => :String 

scala代码不用;来表示一行结束



### 类创建

```scala
class Person() {
  var  name:String = ""
  var  age:Int =0
}
```

语法区别,类名要加() ,并且类中的属性必须有默认值，如果没有默认值则类必须声明是abstract



### 方法创建







# Spark