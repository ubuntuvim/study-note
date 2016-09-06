1. 直接在配置文件`/etc/enviroment`增加NodeJS的`bin`路径。
2. 在`~/.profile`中增加如下配置
```shell
export NODE_HOME=/opt/node-v4.4.4-linux-x64
export PATH=$PATH:$NODE_HOME/bin
export NODE_PATH=$NODE_HOME/lib/node_modules
```