有时间看看如何整合多说到Ember.js项目中，不需要再自己开发评论功能了！

[https://github.com/duoshuo/node-duoshuo](https://github.com/duoshuo/node-duoshuo)


### 动态加载

```js
DUOSHUO.initSelector('.ds-share',{type:'ShareWidget'});
```
[http://dev.duoshuo.com/docs/50b344447f32d30066000147](http://dev.duoshuo.com/docs/50b344447f32d30066000147)

不少站长来询问，希望在首页的文章列表中实现，“点击一个按钮展开该文章的评论”的功能。

其实现在的多说就已经支持这样的模式，实现方法并不复杂：

1.首先加载多说embed.js基础代码，并设置duoshuoQuery，在head内加入：
```
<script>var duoshuoQuery = {short_name:"你的多说二级域名"};</script>
<script src="http://static.duoshuo.com/embed.js"></script>
```
多说二级域名是指你注册多说时，填写的abc.duoshuo.com中的abc部分，

2.编写一个javascript函数，以下函数为示例：
```
function toggleDuoshuoComments(container){
    var el = document.createElement('div');//该div不需要设置class="ds-thread"
    el.setAttribute('data-thread-key', '文章的本地ID');//必选参数
    el.setAttribute('data-url', '你网页的网址');//必选参数
    el.setAttribute('data-author-key', '作者的本地用户ID');//可选参数
    DUOSHUO.EmbedThread(el);
    jQuery(container).append(el);
}
```
3.在按钮上增加onclick事件：
```html
<a href="javascript:void(0);" onclick="toggleDuoshuoComments('#comment-box');">展开评论</a>

<div id="comment-box" ></div>
```
类似的，如果需要在页面加载外之后，动态调用评论数刷新，请调用`DUOSHUO.ThreadCount`函数。


## disqus

[https://github.com/sir-dunxalot/ember-disqus](https://github.com/sir-dunxalot/ember-disqus)