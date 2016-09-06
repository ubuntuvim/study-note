```html
{{#if (eq item.status "complete")}}
  <span>OK</span>
{{/if}}
```

```js
// app/helpers/eq.js

import Ember from 'ember';

const eq = (params) => params[0] === params[1];
export default Ember.Helper.helper(eq);
```