# 本机mac

## 账号相关

#### 1.mircsoft账号

账户  a4433@riverdale.rac.sch.uk
密码  Lpc19950419



#### 2.github账号

mac

账号 2867102374@qq.com 

密码 lpc19950419

windows

账号timorwindows

密码 Lpc19950419



#### 3 google账号

mac号

账号 2867102374@qq.com

密 lpc19950419



windows

账号2049293487@qq.com 

密lpc19950419



#### 4新qq

2049293487

lpc19950419



## 一.mac系统

#### 1. 没有ll指令

只有ls指令，没有ll指令，在～目录下，创建了一个 .bash_profile文件，加了一句alias ll = 'ls -alF' 之后ll就能用了

需要source ～/.bash_profile 



#### 2.mac配置环境变量

安装了homebrew ，没配置环境变量，无法直接执行brew 指令,也是在 ~/.bash_profile 加一句

export PATH="/opt/homebrew/bin:$PATH" ，这句代码执行的是，把/opt/homebrew/bin  拼接在$PATH这个变量前面，

终端打印$PATH 结果是

```
/opt/homebrew/bin /Library/Frameworks/Python.framework/Versions/2.7/bin /usr/local/bin /System/Cryptexes/App/usr/bin /usr/bin /bin /usr/sbin /sbin /usr/local/share/dotnet ~/.dotnet/tools /Library/Frameworks/Mono.framework/Versions/Current/Commands
```

具体原因如下图

```
Mac系统下的环境变量
a. /etc/profile
b. /etc/paths
c. ~/.bash_profile
d. ~/.bash_login
e. ~/.profile
f. ~/.bashrc
说明：
1、其中a和b是系统级别的，系统启动就会加载，其余是用户接别的。
2、c,d,e按照从前往后的顺序读取，如果c文件存在，则后面的几个文件就会被忽略不读了，以此类推。
3、~/.bashrc没有上述规则，它是bash shell打开的时候载入的。
4、建议在c中添加环境变量，以下也是以在c中添加环境变量来演示的
```



打开新的终端之后，配置的.bash_profile 里面的环境变量不生效

- 1、我们是在bash中配置的[环境变量](https://so.csdn.net/so/search?q=环境变量&spm=1001.2101.3001.7020)，而当前系统是使用的是shell。
- 2、查看当前使用的shell：

```bash
1、终端输入：echo $SHELL 
2、输出是/bin/zsh，说明使用的是zsh
```

解决方法  在~/.zshrc  文件加中添加source ~/.bash_profile    



#### 3.homebrew 

下载指令brew install   

homebrew 下载的软件不用配置环境变量就能终端直接使用，和yum差不多. 

homebrew下载的软件目录在 /usr/local里面





#### 4.java安装目录

当初下载java的时候是通过dmg文件下载的，dgm的下载的不需要自己配置 JAVA_HOME ，就可以在终端执行java

但是通过dmg下载的java目录，可以通过指令查询  /usr/libexec/java_home -V

```
/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home
```



#### 5.word和excel激活

```
账户 
a4433@riverdale.rac.sch.uk
密码
Lpc19950419

注意登陆后，需要点击右上角 关于Microft word下面一行 ，激活word才行，并不是登陆就行
```



## 二.mac指令

 1 shift+command +. 可以显示隐藏文件

2 control + 空格  切换输入法

3 command+option+v 将复制的剪切下来



## 外设键盘指令

外设连接：

键盘开蓝牙：fn + q 蓝牙1。fn +w 蓝牙2 fn +e 蓝牙3. 多设备切换也是。 

B是蓝牙，G是接收器。
