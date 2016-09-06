#### 获取父路由的参数

## 删除

```javascript
App.ApplicationAdapter = DS.Adapter.extend({
  deleteRecord: function(store, type, record) {
    var data = this.serialize(record, { includeId: true });
    var id = record.get('id');
    var url = [type, id].join('/');

    return new Ember.RSVP.Promise(function(resolve, reject) {
      jQuery.ajax({
        type: 'DELETE',
        url: url,
        dataType: 'json',
        data: data
      }).then(function(data) {
        Ember.run(null, resolve, data);
      }, function(jqXHR) {
        jqXHR.then = null; // tame jQuery's ill mannered promises
        Ember.run(null, reject, jqXHR);
      });
    });
  }
});
```

```js
this.parentController.getProperties(this.parentController.queryParams);
```

[https://github.com/emberjs/rfcs/blob/master/text/0050-improved-actions.md](https://github.com/emberjs/rfcs/blob/master/text/0050-improved-actions.md)

### 2. 使用数组下标取值
```js
model() {
  return [
    [1, "text a"],
    [2, "text b"],
    [3, "text c"],
  ]
}
```
```html
<ul>
{{#each model as |item|}}
  <li>
    {{item.[0]}}: {{item.[1]}}
  </li>
{{/each}}
</ul>

<ul>
{{#each model as |item|}}
  <li>
    {{item.0.text}}: {{item.1.text}}
  </li>
{{/each}}
</ul>

{{#each model as |item index|}}
  Index is: {{index}}
{{/each}}
```

## 服务程序
```js
// server/index.js

// other dependencies installed via `npm install --save-dev`
var bodyParser = require('body-parser');

module.exports = function(app) {

  app.use(bodyParser.json({ type: "application/json" }));

  app.get('/api/items/:item', function(req, res) {
    const item = localdb.find('item', req.params.item);
    res.send({ item: item });
  });

  // other API endpoints

}
```