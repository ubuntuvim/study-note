[升级文档](https://github.com/ember-cli/ember-cli/releases)

## 升级

### 升级全局Ember-cli

先删除旧版的ember-cli，然后清除缓存，然后在安装新版ember-cli

```
npm uninstall -g ember-cli

npm cache clean

bower cache clean

npm install -g ember-cli@2.6.0-beta.2
```

### 升级项目的Ember-cli

```
// 删除原来插件目录
rm -rf node_modules bower_components dist tmp

// 安装新版ember-cli
npm install --save-dev ember-cli@2.6.0-beta.2 

// 重新安装依赖
npm install
bower install

//使用新版ember-cli重新初始化项目
ember init
```