使用插件生成一个包括用户新的access token
[https://github.com/auth0/node-jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)

用法
```js
user.token = jwt.sign(user, process.env.JWT_SECRET);
user.save(function(err, user1) {
    res.json({
        type: true,
        data: user1,
        token: user1.token
    });
});
```
**参考**

1. [https://zhuanlan.zhihu.com/p/19920223](https://zhuanlan.zhihu.com/p/19920223)
2. [https://github.com/huseyinbabal/token-based-auth-backend/blob/master/server.js](https://github.com/huseyinbabal/token-based-auth-backend/blob/master/server.js)