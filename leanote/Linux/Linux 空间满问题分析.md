## 问题描述： 
 
用户在系统内进行创建文件的时候，出现空间不足提示：“No space left on device …” 
 
## 解决过程： 
 
Linux 系统空间满，常见的原因包括： 
（1）分区容量满； 
（2）分区inode耗尽； 
（3）僵尸文件：已删除文件因句柄被占用未释放导致相应空间未释放。 
 
 
上述3种情况的处理思路分别说明如下： 

#### 1、分区容量满——空间使用分析 
  a)   使用如下指令查看空间使用情况  
```shell
# df -hT

Filesystem          Type   Size  Used Avail Use% Mounted on
tmpfs               tmpfs   16G  228K  16G   1% /dev/shm
/dev/sda1           ext4   477M   59M 393M  13% /boot
```
  b)    使用如下指令逐层分析各目录的空间占用情况  
```
du -m --max-depth=1 |sort -gr
```
逐层进入空间占用最高的目录，继续执行上述指令，逐步定位占用过高空间的文件或目录，然后进行相应清理。 

#### 2、inode容量满——inode使用分析 
 
 a)     使用如下指令查看inode使用情况 
```
df -i
```
b)     使用如下指令分析inode使用情况  
```
for i in /*; do echo $i; find $i | wc -l; done
```
逐层进入inode占用最高的目录，继续执行上述指令，逐步定位占用过高空间的文件或目录，然后进行相应清理。 

#### 3、僵尸文件分析 

a)     使用如下指令查看已经删除，但是句柄未被释放的文件僵尸文件及其空间占用情况： 
```
for i in `lsof | grep delete | awk -F" "'{print $9}'` ;do du -h $i;done | sort -gr
```
b)     使用如下指令查找上述僵尸文件归属进程ID 
```
 for i in `lsof | grep delete | awk -F" "'{print $9}'` ;do du -h $i;done | sort -gr
```
c)     使用如下指令查看相应的进程信息： 
```
ll /proc/[size=font-size:9.0pt,9.0pt]<pid[size=font-size:9.0pt,9.0pt]信息>/exe
[size=font-size:9.0pt,9.0pt]比如：
# ll /proc/10835/exe
```

d)     重启相应进程后，就能释放相应句柄，释放被占用的空间。 


原文：[https://bbs.aliyun.com/read/252526.html?spm=5176.bbsl229.0.0.oGEWk1&fpage=2](https://bbs.aliyun.com/read/252526.html?spm=5176.bbsl229.0.0.oGEWk1&fpage=2)
 