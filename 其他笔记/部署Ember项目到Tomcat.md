* 执行命令打包
```
ember build --prod
```

* 复制打包后的项目到tomcat的`tomcat目录\webapps\ROOT`目录下。原来ROOT的内容可以全部删除。
* 如果还是无法访问请配置`tomcat目录/conf/server.xml`文件。在最后的`Host`标签内增加 如下行的配置 
```html
<Context docBase="C:\Users\Administrator\Desktop\web" path="/" reloadable="true"/>
```
其中`C:\Users\Administrator\Desktop\web`是你打包后项目所在路径。
比如我的配置如下图：

![配置截图](http://7xnrhh.com1.z0.glb.clouddn.com/QQ%E5%9B%BE%E7%89%8720160426190410.png)

* 最后访问[http://127.0.0.1:8080/](http://127.0.0.1:8080/)
