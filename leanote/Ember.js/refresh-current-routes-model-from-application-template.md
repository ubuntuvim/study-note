```js
// app/controllers/application
actions: {
  refreshCurrentModel() {
    let route = Ember.getOwner(this).lookup(`route:${get(this, 'currentRouteName')}`);
    return route.refresh();
  }
}
```

```html
{{!-- application.hbs --}}
<button {{action 'refreshCurrentModel'}}>Refresh</button>
{{outlet}}
```