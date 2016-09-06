新建的用户但是没有权限的，需要授权。授权命令如下：

root用户登录系统。

```sh
$ visodu
```

这个命令修改的是`/etc/sudoers`。然后找到，`root ALL=(ALL) ALL`，参考再加一行。

```
ubuntuvim ALL=(ALL) ALL
```

`ubuntuvim`为用户名。

查看是否添加成功。

```
cat /etc/sudoers
```

新增的用户在命令行可能无法删除，按Backspace删除的时候出现`^h`字符，需要用下面的命令设置。

```sh
stty erase ^h
```