Ô­Ö·£º[http://discuss.emberjs.com/t/get-current-url-in-emberjs/3019](http://discuss.emberjs.com/t/get-current-url-in-emberjs/3019)

```js
afterModel: function (model){
    _this = this;

    Ember.run.next(function(){
        console.log _this.get('router.url')
    });
}
```

```js
afterModel: function(model, transition) {
  console.log(Ember.get(transition, 'targetName'));
}
```
