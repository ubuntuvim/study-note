### 上传文件

命令格式：
```
scp 本地文件 user@host:远程路径
```

上传当前目录下的`web.zip`到服务器的`/alidata/www/default/`目录下。
```
scp ./web.zip root@120.24.90.140:/alidata/www/default
unzip web.zip
```