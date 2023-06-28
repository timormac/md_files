# idea快捷键 

1.新建一行并到行头 shift+ enter

2.快速导包 option+enter

3.双击shift 搜索

4 .导入构造器 ，getter，setter等

​	control+enter

5.查找方法对应代码 

​	command+鼠标左键

6.快速生成变量名和对应类型

​	option+comman+v 可以快速生成变量类型和变量名称

​	也可以使用.var 然后摁tab键 也可以

7 快速创建for循环  

​	fori + tab  创建个for巡船

​	集合名字.for +tab快速创建集合的for循环

8 command +/单行注释

​	command+option+/ 多行注释

9 else-if +tab自动生成(){}

10 删除一行  command+后退 

11 根据类 快速new对象   类名.new + tab   

12 快速实现接口中方法 control + i

12 重写非必须方法 control + o

13 异常捕获上抛 option +shift + 回车

14 查看接口的实现类 control+H ,鼠标选中接口

15 shift + TAB 选中的代码整体前移

16 /add +TAB生成参数模版







最常用：
⌘ + ⌥ + L 格式化代码
⌘ + N 快速打开某个类,查看类里面的所有方法；
⌘ + ⌥ + T 可以把代码包在一个块内，例如：try/catch,if,if else,try catch,do while;
⌘ + ⇧ + F 在全工程查找
⌘ + ⇧ + Enter 自动补全末尾的字符（括号，分号），例如敲完if/for时也可以自动补上{}
花括号。
⌘ + P 可以显示参数信息
⌘ + R 在当前窗口替换
⌘ + ⇧ + R 在全工程替换





# idea其他设置

#### 配置maven 

在setting=> build => build tools =>勾选overwirte,更改repository和settings.xml

idea默认maven位置 : /Users/timor/.m2/settings.xml             /Users/timor/.m2/repository

idea默认maven是集成的maven3，需要退出项目然后设置maven才是全局的maven

注意从git上下载的项目和手动到入的项目,maven还是用idea自带的maven3



#### 配置签名信息

settings =>Editor=>File and Code=>然后找到Files旁边的include=>点击File Header

```
/**  
* @Title: ${NAME}
* @Package: ${PACKAGE_NAME}
* @Description:
* @Author: XXX
* @Date: ${DATE} ${TIME}
* @Version:1.0
*/
```

#### 函数注释配置

settings =>edit=>live Template+>点击右侧➕ =>选择Template group =>自定义名称=>选中刚定义的group=>点击右侧➕

=>live Template =>Abbreviation输出调用指令(add) =>Description填写一下=>勾选最下方java，声明指令的作用为java代码

=>/add加 tab调用指令

模版如下：

```
**
 * @description: $description$
 * @return: $return$
 * @Params:$params$
 */
```



#### 集成tomcat

settings=>Bulid,excution=>Application Servers =>点击加号选中tomcat=>指定路径home



#### 配置scala

安装scala

scala安装包解压，然后配置环境变量  vim ~/.bash_profile    export PATH=$PATH:scala/bin

idea配置scala

settings=>plugins=>搜索scala下载=>project structure =>Global libarys=>点击加号添加scala依赖

=>接右键项目，选择add framwork 添加scala支持 ，其实是在在project structure的module中增加了scala的denpendcy



# idea集成git

1  mac版右键代码打断点位置，选择 annotate with git blame ，可以查看当前代码是哪个版本初始拉取的(最后一次变化版本)

选择 annotate with git blame后，左键点击打断点处，可以看到历史commit版本（点击左下角git查看log效果一样）

2 idea中没有add，一般直接勾选更改和新建的文件，然后直接commit的时候，已经执行过add了







# idea的使用

#### 误删文件还原

右键变动的文件夹，选择local History



#### debug使用

自己弄一个工具，然后都用自己手写的类和对象，最后看看链路怎么走，怎么进去的。

1 step over  2 step into 3 force step into 4 step out 5 run to curse(进去光标)

step over直接跳过当前行

step into 进入方法 ,若当前行有多个方法，不同的方法会有提示，鼠标选择进入哪个想要看的方法

force step into 可以进入到jdk库方法，而step into 不行

step out 跳出方法，回到你点step into时的位置

 run to curse 当你点入一个方法里面有很多行，你想具体看某行的实现，鼠标光标放到对应行首点run to curse





#### 目录层级 

1   .idea目录  ，out目录，src目录   out目录里面是是运行前编译放在这里

2   项目分级，一个project可以有多个module，一个模块负责一个功能。pooject和module区别，刚开始创建的时候，默认是一个单工程的项目，当你新建一个module是，会在你的目录下出现一个新的目录，所以一般初始创建的第一个src别编码![](/Users/timor/Desktop/近期准备/md图片/project_module.png)



#### idea导入项目

idea导入项目的时候，如果maven等无法下载。可以把/idea和。iml文件删除。重新导入，idea重新构建项目。



#### 打包maven项目

打包之后的jar包，里面没有resource也没有src等路径。所以读取配置文件的路径，在idea里能跑，打成jar包后不能跑。



#### 设置语法等级

project structure 

第一个是选择jdk版本，第二个是制定可执行语法等级，比如你是17版本jdk，但是如果指定是8的语法等级也不能用17的语法

之前默认语法等级是5，导致无法使用lamda表达式



#### 外部导入module

如何从外部导入一个module，先将目录复制到对应的目录层级下，然后在project structure中导入module这样才会把复制过去的目录识别为一个module



#### project structure

有时候需要我们手动导入jar包，比如之前severlet的包，在project structure中，project setting下的libraries可以选定本项目下的哪些module可以使用这个包，而platform setting的global libraries 是所有module都能用的

#### run功能模版

idea导航栏有个run，有的pom文件，你把依赖设置成<scop>provided</scop>时，会导致无法直接run。

点进run=》点击edit configrations=>edit template=>applictions=>modify options=>add denpendency with provided

这个模版配置一次，以后就不用配置了.windows上的操作是un=>edit configrations=>templateapplictions=>勾选provided



# idea问题记录

1.pom导入jar依赖没问题，不过点进去之后，用idea 下载soure源码一直失败，在目录下的terminal执行 mvn dependency:sources

2.从gitbub 拉取mac的项目，在winodows上运行找不到类。因为pom是provided，需要winodows配置run模版，配置with provided



# idea问题待解决
