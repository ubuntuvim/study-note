* 复制安装包到`/opt/apache-maven-3.0.5`下
* 配置，打开`sudo gedit ~/.bashrc`

```shell
export M2_HOME=/opt/apache-maven/apache-maven-3.0.5
export M2=$M2_HOME/bin
export MAVEN_OPTS="-Xms256m -Xmx512m"  // 可选
export PATH=$M2:$PATH
```

* 重载配置文件，使其生效

```shell
source ~/.bashrc
```

* 测试是否配置成功

在终端输入`mvn -v`，如果看到类似如下信息说明配置成功。

```shell
Apache Maven 3.0.5 (r01de14724cdef164cd33c7c8c2fe155faf9602da; 2013-02-19 21:51:28+0800)
Maven home: /opt/apache-maven/apache-maven-3.0.5
Java version: 1.7.0_65, vendor: Oracle Corporation
Java home: /opt/jdk/jdk1.7.0_65/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "linux", version: "4.4.0-21-generic", arch: "amd64", family: "unix"
```


下载主题后解压，然后复制到`/usr/share/themes`，再在设置里修该主题即可。

复制命令修改权限
```shell
sudo cp -r ./themeName /usr/share/themes
sudo chmod -R 777 /usr/share/themes/themeName
```