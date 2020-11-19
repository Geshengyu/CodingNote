## 三、Linux操作系统

### 3.1 体系结构

用户态：用户上层活动

内核态：本质是一段管理计算机硬件设备的程序

> 系统调用：内核的访问接口
>
> 公用函数库：系统调用的组合拳

### 3.2 常用的命令

#### 1 find 查找特定的文件

`find -name "target.java"`       查找文件名

`find / -name "target.java"`       全局搜索 

`find / -iname "target*"`        不区分大小写

#### 2 grep 检索文件内容

`grep 'partial\[true\]' target.log`     筛选包含相应字符串的某文件的对应行

`grep -o 'engine\[[0-9a-z]*\]'`       筛选出符合相关正则表达式的内容

`grep -v 'grep'`      过滤掉相关字符串的内容

#### 3 awk 统计文件内容

`awk '{print $1,$4}' netstat.txt`       筛选文件中某些列的数据

`awk '$1=="tcp"&&$2==1 {print $0}' netstat.txt`     依据列的条件筛选数据

`awk '{enginearr[$1]++}END{for(i in enginearr)print i "\t" enginearr[i]}'`    对内容逐行进行统计操作，并显示统计结果

#### 4 sed 批量替换文本内容

`sed -i 's/^Str/String/' replace.java`     替换以Str开头的替换成String

`sed -i 's/\.$/\;/' replace.java`       替换以.结尾的

`sed -i 's/Jack/me/g' replace.java`     对整行内容进行替换

`sed -i '/^ *$/d' replace.java`    删除空行

#### 5 tail 

tail 命令可用于**查看文件的内容**，有一个常用的参数 **-f** 常用于查阅正在改变的日志文件。默认显示文件最后的10行文本。

`tail -200f filename` 会把 filename 文件里的最尾部的200行内容显示在屏幕上，并且不断刷新，只要 filename 更新就可以看到最新的文件内容。

> 详细用法见：https://www.runoob.com/linux/linux-comm-tail.html

#### 6 cat

cat（英文全拼：concatenate）命令用于连接文件并打印到标准输出设备上。

#### 7 more 与 less

more 命令类似 cat ，不过会以一页一页的形式显示，更方便使用者逐页阅读，而最基本的指令就是按空白键（space）就往下一页显示，按 b 键就会往回（back）一页显示，而且还有搜寻字串的功能（与 vi 相似），使用中的说明文件，请按 h 。

less 与 more 类似，但使用 less 可以随意浏览文件，而 more 仅能向前移动，却不能向后移动，而且 less 在查看之前不会加载整个文件。

#### 8 ps

ps （英文全拼：process status）命令用于显示当前进程的状态，类似于 windows 的任务管理器。

```shell
# ps -ef //显示所有命令，连带命令行
UID    PID PPID C STIME TTY     TIME CMD
root     1   0 0 10:22 ?    00:00:02 /sbin/init
root     2   0 0 10:22 ?    00:00:00 [kthreadd]
root     3   2 0 10:22 ?    00:00:00 [migration/0]
root     4   2 0 10:22 ?    00:00:00 [ksoftirqd/0]
root     5   2 0 10:22 ?    00:00:00 [watchdog/0]
root     6   2 0 10:22 ?    /usr/lib/NetworkManager
……省略部分结果
root   31302 2095 0 17:42 ?    00:00:00 sshd: root@pts/2 
root   31374 31302 0 17:42 pts/2  00:00:00 -bash
root   31400   1 0 17:46 ?    00:00:00 /usr/bin/python /usr/sbin/aptd
root   31407 31374 0 17:48 pts/2  00:00:00 ps -ef
```

> 详细用法见：https://www.runoob.com/linux/linux-comm-ps.html

#### 9 scp 与 mv

 scp 命令用于 Linux 之间复制文件和目录。

```sh
scp test.class root@192.168.1.1:/opt/
```

mv（英文全拼：move file）命令用来为文件或目录改名、或将文件或目录移入其它位置。

```shell
mv test.class /opt/temp
```

#### 10 netstat

netstat 命令用于显示网络状态。

显示UDP端口号的使用情况

```shell
netstat -apu
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address        Foreign Address       State    PID/Program name  
udp    0   0 *:32768           *:*                   -          
udp    0   0 *:nfs            *:*                   -          
udp    0   0 *:641            *:*                   3006/rpc.statd   
udp    0   0 192.168.0.3:netbios-ns   *:*                   3537/nmbd      
udp    0   0 *:netbios-ns        *:*                   3537/nmbd      
udp    0   0 192.168.0.3:netbios-dgm   *:*                   3537/nmbd      
udp    0   0 *:netbios-dgm        *:*                   3537/nmbd      
udp    0   0 *:tftp           *:*                   3346/xinetd     
```

> 推荐参考的资料：https://www.runoob.com/w3cnote/linux-useful-command.html