#General Javascript Guides

- As a standard, we line break at 80. There are few exceptions;

- Use a double space, e.g. two space bars, as the standard spacing unit;

- Variables names should be camelCased;

- Comments should be useful and not fodder. They should point out different or difficult things. They should not consider generally presumed language or framework functionality (e.g. //sendaction bubbles action);

- Always use es6 syntax, e.g. () vs function();

- Space the parentheses in statements, for example:

```javascript
if (condition) { };

vs

if(condition) {};
```

- When an array becomes >2 lines, break it into newlines:

```javascript
export default Component.extend({
  classNames: [
  ‘class-1’,
  ‘class-2’
  ]
});
```

- Use const for most variable declarations. Use let in cases where you need to re-assign a value. Use var almost never;

- Forward declare as much as possible. So instead of:

```javascript
export default Component.extend({
  actions: {
    authenticate() {
      this.sendAction(‘authenticate’, this.get(‘email’), this.get(‘password’));
    }
  }
});
```

do:

```javascript
export default Component.extend({
  actions: {
    authenticate() {
      const credentials = this.getProperties(‘email’, ‘password’);
      this.sendAction(‘authenticate’, credentials);
    }
  }
});
```

- Div nesting:
```html
<div class=“foo”> Some long string </div>
```

becomes hard to manage really fast. In most cases, you instead want:

```html
<div class=‘foo’>
  Some long string
</div>
```

Templates should be kept nice looking too!
