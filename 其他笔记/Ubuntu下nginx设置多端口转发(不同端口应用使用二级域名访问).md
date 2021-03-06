*本例子使用的是阿里云的ECS服务器，如果是其他的服务器请根据实际情况修改nginx的配置文件。*

修改配置文件：`/alidata/server/nginx/conf/vhosts`。

```
//  第一个应用配置
server {
    listen       80 default;
    server_name  www.ddlisting.com;
        index index.html index.htm index.jsp *;
        root /alidata/www/default;

        location ~ \.jsp$ {
                proxy_pass    http://127.0.0.1:8080;
        }

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
                expires 30d;
        }

        location ~ .*\.(js|css)?$
        {
                expires 1h;
        }

        access_log  /alidata/log/nginx/access/default.log;
}
//第二个应用配置，这个应用不是运行在80端口上的。并且可以是另外一个服务器
//使用属性proxy_pass设置
server {
    listen       80;
    server_name  blog.ddlisting.com;

        location / {
                proxy_redirect off;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://127.0.0.1:2368;
        }

        access_log  /alidata/log/nginx/access/blog.log;
}
```

配置完成需要重启nginx。
`/etc/init.d/nginx restart`

配置修改完成之后需要设置域名的解析设置，在解析设置中增加一个二级域名。比如：

![域名配置](http://7xnrhh.com1.z0.glb.clouddn.com/%E5%9F%9F%E5%90%8D%E9%85%8D%E7%BD%AE.png)

我的主域名是：[ddlisting.com](ddlisting.com)，增加了一个二级域名为[blog.ddlisting.com](blog.ddlisting.com)，
然后结合nginx的配置实现，服务器上的一个运行在`2368`端口的应用可以以[blog.ddlisting.com](blog.ddlisting.com)方式访问，而且不需要指出端口就可以访问（一般情况访问需要加端口，比如[blog.ddlisting.com:2368](blog.ddlisting.com:2368)）。

所以呢，不管应用再多只要一个域名足够了，其他的应用配置的nginx即可，并且访问时候不需要加端口。非常方便！！


**参考网址**：
[Ubuntu下nginx设置多端口转发(反向代理)](http://touzi.github.io/Ubuntu%E4%B8%8Bnginx%E8%AE%BE%E7%BD%AE%E5%A4%9A%E7%AB%AF%E5%8F%A3%E8%BD%AC%E5%8F%91(%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86)/
)
