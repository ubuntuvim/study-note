最近本人在学习开发NodeJs，使用到express框架，对于网上的学习资料甚少，因此本人会经常在开发中做一些总结。

express获取参数有三种方法：官网介绍如下
```
Checks route params (req.params), ex: /user/:id
Checks query string params (req.query), ex: ?id=12
Checks urlencoded body params (req.body), ex: id=
```
1、例如：`127.0.0.1:3000/index`，这种情况下，我们为了得到`index`，我们可以通过使用`req.params`得到，通过这种方法我们就可以很好的处理Node中的路由处理问题，同时利用这点可以非常方便的实现MVC模式；
2、例如：`127.0.0.1:3000/index?id=12`，这种情况下，这种方式是获取客户端get方式传递过来的值，通过使用`req.query.id`就可以获得，类似于PHP的get方法；

3、例如：`127.0.0.1：300/index`，然后post了一个`id=2`的值，这种方式是获取客户端post过来的数据，可以通过`req.body.id`获取，类似于PHP的post方法；