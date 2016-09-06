1.切换到root用户下,怎么切换就不用说了吧,不会的自己百度去.

2.添加sudo文件的写权限,命令是:
```
chmod u+w /etc/sudoers
```

3.编辑sudoers文件
```
vi /etc/sudoers
```
找到这行 `root ALL=(ALL) ALL`,在他下面添加`xxx ALL=(ALL) ALL` (这里的`xxx`是你的用户名)

ps:这里说下你可以`sudoers`添加下面四行中任意一条
```
youuser            ALL=(ALL)                ALL
%youuser           ALL=(ALL)                ALL
youuser            ALL=(ALL)                NOPASSWD: ALL
%youuser           ALL=(ALL)                NOPASSWD: ALL
```

第一行:允许用户youuser执行sudo命令(需要输入密码).
第二行:允许用户组youuser里面的用户执行sudo命令(需要输入密码).
第三行:允许用户youuser执行sudo命令,并且在执行的时候不输入密码.
第四行:允许用户组youuser里面的用户执行sudo命令,并且在执行的时候不输入密码.

4.撤销sudoers文件写权限,命令:
```
chmod u-w /etc/sudoers
```

这样普通用户就可以使用sudo了.