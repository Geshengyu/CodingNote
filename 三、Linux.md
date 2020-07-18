## 三、Linux

### 3.1 体系结构

用户态：用户上层活动

内核态：本质是一段管理计算机硬件设备的程序

> 系统调用：内核的访问接口
>
> 公用函数库：系统调用的组合拳

### 3.2 如何查找特定的文件

`find -name "target.java"`       查找文件名

`find / -name "target.java"`       全局搜索 

`find / -iname "target*"`        不区分大小写

### 3.3 检索文件内容

`grep 'partial\[true\]' target.log`     筛选包含相应字符串的某文件的对应行

`grep -o 'engine\[[0-9a-z]*\]'`       筛选出符合相关正则表达式的内容

`grep -v 'grep'`      过滤掉相关字符串的内容

### 3.4统计文件内容

`awk '{print $1,$4}' netstat.txt`       筛选文件中某些列的数据

`awk '$1=="tcp"&&$2==1 {print $0}' netstat.txt`     依据列的条件筛选数据

`awk '{enginearr[$1]++}END{for(i in enginearr)print i "\t" enginearr[i]}'`    对内容逐行进行统计操作，并显示统计结果

### 3.5 批量替换文本内容

`sed -i 's/^Str/String/' replace.java`     替换以Str开头的替换成String

`sed -i 's/\.$/\;/' replace.java`       替换以.结尾的

`sed -i 's/Jack/me/g' replace.java`     对整行内容进行替换

`sed -i '/^ *$/d' replace.java`    删除空行