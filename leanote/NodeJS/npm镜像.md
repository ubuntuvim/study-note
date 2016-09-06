## npm镜像使用方法

> npm镜像，就是npm的只读副本。

使用方法有3种，推荐第一种或者第三种

### 1. 编辑 ~/.npmrc 加入下面内容
```
reistry = http://registry.cnpmjs.org
```
永久生效。

### 2. config命令
```
npm config set registry http://registry.cnpmjs.org 
npm info underscore
```
（如果上面配置正确这个命令会有字符串response）
永久生效。

### 3. 命令行指定
```
npm --registry http://registry.cnpmjs.org 
info underscore 
```
本次有效。

**推荐**：`http://registry.cnpmjs.org`镜像速度比较快！！比淘宝NPM还要快！！！