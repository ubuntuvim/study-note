如何在linux系统里安装配置JDK呢？

* 复制JDK安装包到`/opt/jdk`下，复制命令
```shell
sudo cp -r /home/ubuntuvim/software/jdk1.7.0_65 /opt/jdk/
```
我的JDK放在`/home/ubuntuvim/software/`目录下。

* 配置环境变量

编辑文件` ~/.bashrc`
```shell
sudo  ~/.bashrc
// 在配置文件末尾增加如下内容
export JAVA_HOME=/opt/jdk/jdk1.7.0_65
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```
执行命令使得配置生效
```
source ~/.bashrc
```

* 测试是否配置成功

在终端下输入
```shell
java -version
```
看到类似如下信息说明配置成功。
```
java version "1.7.0_65"
Java(TM) SE Runtime Environment (build 1.7.0_65-b17)
Java HotSpot(TM) 64-Bit Server VM (build 24.65-b04, mixed mode)
```