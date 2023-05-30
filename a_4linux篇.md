

# 系统环境

1   环境变量配置完事之后要执行：source /etc/profile ,这行命令执行的是，将/etc/profile.d下的所有*.sh文件执行

2  /etc/profile.d下的环境变量是对所有用户生效，~/.bashrc  ~/.profile  /.bash_profile 这几个只对当前登陆用户生效，所以个人脚本可以放置到这两个里面，mac里面没有bashrc 只有bash_profile

 3 path = $path::/usr/locar/new/bin    这条命令的意思为: 使PATH自增:/usr/locar/new/bin，既PATH=PATH+":/usr/locar/new/bin";

4 自定义脚本不用加入路径的话，就直接新建一个目录,然后在个人的环境变量中，把bin的path加进去，然后就可以了

操作为，在~/.bashrc  中加入 PATH=$PATH:~/bin ,然后在～新建一个bin就行



# vim编辑

dd：删除当前行

# 常用指令

### 服务器交互相关

##### 传送文件

scp   ./cbk_kafka_data_test-1.0-jar-with-dependencies.jar    dev@pre-13:/home/dev/timor_dir



##### 服务器端口代理

本地端口代理，通过ssh跳板机连数据库

 用法1：远程端口映射到其他机器HostB 上启动一个 PortB 端口，映射到 HostC:PortC 上，在 HostB 上运行：HostB$ ssh -L 0.0.0.0:PortB:HostC:PortC   user@HostC用法2：本地端口通过跳板映射到其他机器HostA 上启动一个 PortA 端口，通过 HostB 转发到 HostC:PortC上，在 HostA 上运行：HostA$ ssh -L 0.0.0.0:PortA:HostC:PortC  user@HostBssh -L 127.0.0.1:3308:rm-bp192qdgcbyq9e2db.mysql.rds.aliyuncs.com:3306   timo@jump-server.caibeike.netssh -L 127.0.0.1:27018:172.16.248.138:27017  timo@jump-server.caibeike.netssh -L 127.0.0.1:27021:mongo-main-01.caibeike-in.net:27017  timo@jump-server.caibeike.net跳板机密码: ID7UBobRvbJlzJz1bPfb一次同时映射多个端口ssh -L 0.0.0.0:PortA:HostC:PortC  -L 0.0.0.0:PortB:HostB:PortB -L    user@HostBssh -L 127.0.0.1:3308:rm-bp192qdgcbyq9e2db.mysql.rds.aliyuncs.com:3306  -L 127.0.0.1:27018:172.16.248.138:27017 -L 127.0.0.1:27021:mongo-main-01.caibeike-in.net:27017  timo@jump-server.caibeike.net不过好像不好用

### 查看目录/文件相关

##### 查看目录或文件

tail -n 10  a.txt  查看文件最后几行

tail -f a.txt 持续查看文件末尾，直到退出

whereis  a.txt  获取a.txt的绝对路径

ls -a  查看隐藏文件

ls -l ( ll ) 显示目录下文件详细信息 （读写权限，拥有者，文件大小，创建日期）

tree  或  tree  目录    显示目录树结构











### 服务器状态相关

##### 查看进程

ps aux 

[ps命令](https://so.csdn.net/so/search?q=ps命令&spm=1001.2101.3001.7020)用于报告当前系统的进程状态。可以搭配kill指令随时中断、删除不必要的程序。ps命令是最基本同时也是非常强大的进程查看命令，使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵死、哪些进程占用了过多的资源等等，总之大部分信息都是可以通过执行该命令得到的。

- a：显示当前终端下的所有进程信息，包括其他用户的进程。
- u：使用以用户为主的格式输出进程信息。
- x：显示当前用户在所有终端下的进程。

ps -elf

- -e：显示系统内的所有进程信息。
- -l：使用长（long）格式显示进程信息。
- -f：使用完整的（full）格式显示进程信息。



##### 查看内存情况

free   -h 显示最大内存及可用内存

top   以全屏交互式的界面显示进程排名，及时跟踪包括CPU、内存等系统资源占用情况，默认情况下每三秒刷新一次，

### 执行程序脚本相关



##### nohup

不挂断地运行命令。no hangup的缩写，意即“不挂断”。

nohup Command [ Arg ... ] [　& ]

nohup 命令运行由 Command参数和任何相关的 Arg参数指定的命令，忽略所有挂断（SIGHUP）信号。
nohup放在命令的开头，表示不挂起（no hang up），也即，关闭终端或者退出某个账号，进程也继续保持运行状态，一般配合&符号一起使用。如nohup command &。



dev/null 表示空设备文件
0 表示stdin标准输入
1 表示stdout标准输出
2 表示stderr标准错误
> file 表示将标准输出输出到file中，也就相当于 1>file
> 是定向输出到文件，如果文件不存在，就创建文件；如果文件存在，就将其清空；一般我们备份清理日志文件的时候，就是这种方法：先备份日志，再用`>`，将日志文件清空（文件大小变成0字节）；
> >这个是将输出内容追加到目标文件中。如果文件不存在，就创建文件；如果文件存在，则将新的内容追加到那个文件的末尾，该文件中的原有内容不受影响。
> >2> error 表示将错误输出到error文件中
> >2>&1 也就表示将错误重定向到标准输出上
> >2>&1 >file ：错误输出到终端，标准输出重定向到文件file，等于 > file 2>&1(标准输出重定向到文件，错误重定向到标准输出)。
> >& 放在命令到结尾，表示后台运行，防止终端一直被某个进程占用，这样终端可以执行别到任务，配合 >file 2>&1可以将log保存到某个文件中，但如果终端关闭，则进程也停止运行。如 command > file.log 2>&1 & 。
> >————————————————
> >版权声明：本文为CSDN博主「琦彦」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> >原文链接：https://blog.csdn.net/fly910905/article/details/90739559



Shell中可能经常能看到：>/dev/null 2>&1

  eg：sudo kill -9 `[ps](http://www.111cn.net/fw/photo.html) -elf |[grep](https://so.csdn.net/so/search?q=grep&spm=1001.2101.3001.7020) -v grep|grep $1|awk '{print $4}'` 1>/dev/null 2>/dev/null

命令的结果可以通过%>的形式来定义输出

/dev/null 代表空设备文件

  \> 代表重定向到哪里，例如：echo "123" > /home/123.txt
  1 表示stdout标准输出，系统默认值是1，所以">/dev/null"等同于"1>/dev/null"
  2 表示stderr标准错误
  & 表示等同于的意思，2>&1，表示2的输出重定向等同于1

那么本文标题的语句：

1>/dev/null 首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。

2>&1 接着，标准错误输出重定向等同于 标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。

 

2>&1写在后面的原因

  格式：command > file 2>&1  == command 1> file 2>&1

首先是command > file将标准输出重定向到file中， 2>&1 是标准错误拷贝了标准输出，也就是同样被重定向到file中，最终结果就是标准输出和错误都被重定向到file中。

如果改成： command 2>&1 >file

2>&1 标准错误拷贝了标准输出的行为，但此时标准输出还是输出到终端。当 >file 后，标准输出才被重定向到file，但标准错误仍然保留了先前的设置，即保持输出到终端。

#### 

