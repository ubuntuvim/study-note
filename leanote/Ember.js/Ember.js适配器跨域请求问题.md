在适配器中重写如下方法可实现跨域。
```js
ajaxOptions: function(url, type, options) {
    var hash = this._super(url, type, options);
    hash.dataType = "jsonp";
    return hash;
}
```