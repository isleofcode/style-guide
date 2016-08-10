The Ember Style Guide
---------------------

This Ember style guide recommends best practices so that real-world Ember programmers can write code that can be maintained by other real-world Ember programmers. A style guide that reflects real-world usage gets used, and a style guide that holds to an ideal that has been rejected by the people it is supposed to help risks not getting used at all – no matter how good it is.

The guide is separated into several sections of related rules. Rationale for each the rules is provided unless it's fairly obvious.

## Table of Contents

- [Classes](#classes)
- [Route Handlers](#route-handlers)
- [Controllers](#controllers)
- [Components](#components)
- [Models](#models)
- [Mixins](#mixins)
- [Naming](#naming)
- [Misc](#misc)

## Classes

- Use a consistent structure in your class definitions.

```javascript
// import statements go first
import Ember from 'ember';

// namespace deconstruction
const { Component } = Ember;

// default export
export default Ember.Component.extend({
  // body omitted
});
```

- Deconstruct local namespaces. This pattern also serves as a handy tool for developers to know what you actually used in a file.

```javascript
import Ember from 'ember';

// bad
export default Ember.Component.extend();

// good - single namespace
const { Component } = Ember;

export default Component.extend();

// bad
export default Ember.Component.extend({
  foo: Ember.computed('bar', function() {
    // return something...
  })
});

// good - newline multiple namespaces
const {
  Component,
  computed
} = Ember;

export default Component.extend({
  foo: computed('bar', function() {
    // return something...
  })
});
```

- Deconstruct methods with care.

```javascript
// bad
const {
  Component,
  moment: { add }
} = Ember;

export default Component.extend({
  session: service();
});

// good - Ember Core methods can be safely deconstructed
const {
  Component,
  inject: { service }
} = Ember;

export default Component.extend({
  session: service();
});
```

## Route Handlers

- Use a consistent structure for route handlers.

```javascript
export default Route.extend({
  // service declarations

  // lifecycle hooks in order of execution
  model() {},
  setupController() {},

  // custom functions

  // actions separated by a newline
});
```

- Pass models to controllers using descriptive name on an `attrs` hook. This distinguishes items passed to the controller from items on the controller itself and avoids questions of which model is being passed in.

```javascript
// bad - it is unclear which model the controller has been passed
export default Route.extend({
  model() {
    return this.store.findAll('contact');
  }
});

// good
export default Route.extend({
  model() {
    return this.store.findAll('contact');
  },

  setupController(controller, model) {
    controller.set('attrs.contacts', model);
  }
});

// good - use RSVP.hash for multiple models
export default Route.extend({
  model() {
    return RSVP.hash({
      followers: this.store.findAll('follower'),
      following: this.store.findAll('following')
    })
  },

  setupController(controller, model) {
    controller.set('attrs', model);
  }
});
```

- Handle application state in routes and component state in components.

- Rollback attributes on model edit route exit.

## Controllers

- Use controllers to decorate the model, handling query params, and where reduced component nesting noticeably improves performance on mobile devices.

- Use a consistent structure for controllers.

```javascript
export default Controller.extend({
  // passed properties
  attrs: {},
  
  // computed properties
  sortedContacts: computed('attrs.contacts.[]', function() {
    return this.get('attrs.contacts').sortBy('createdAt').reverse();
  }),
});
```

## Components

- Use a consistent structure for components.

```javascript
export default Component.extend({
  // service declarations

  // native component properties
  tagName: 'nav',

  // passed properties
  attrs: {},

  // custom properties
  foo: 'foo',

  // lifecycle hooks in order of execution
  init() {},
  didReceiveAttrs() {},
  didInsertElement() {},

  // computed properties
  fancyFoo: computed('foo', function() {
    // body omitted
  }),

  // native functions

  // actions separated by a newline
  actions: {
    doFoo() {
      // body omitted
    },

    doBar() {
      // body omitted
    }
  }
});
```

- Set tag name. Components don’t need to be wrapped in a div as they are by default. Use the `tagName` property to modify a component before wrapping it.

```javascript
// good
export default Component.extend({
  tagName: 'nav'
});
```

- Don't over nest your components. Consider if your nesting adds better details or obfuscation for new developers. Heavy DOM nesting negatively impacts performance on mobile devices.

- Align properties in Handlebars.

```hbs
# good
{{component-name
  property='foo'
  action=(action 'actionName')}}
```

## Models

- Use a consistent structure for models.

```javascript
export default Model.extend({
  // service declarations

  // attributes
  title: attr('string'),
  body: attr('string'),

  // relationships
  comments: hasMany('comment'),
  author: belongsTo('user'),

  // computed properties
  fancyTitle: computed('title', function() {
    // return something
  });

  // custom functions
});
```

- Declare and use an application serializer and inherit from it when changes are required.

- Declare and use an application adapter and inherit from it when changes are required.

- Avoid importing properties from other files. For example, a constant on Model A should not be imported to Model B in most cases;

## Mixins

- Newline after mixin declarations.

```javascript
// good
export default Component.extend(
  Mixin, {

  // body omitted
});


// good
export default Component.extend(
  Mixin1,
  Mixin2 {

  // body omitted
});
```

- Use a consistent directory structure.

```
# good
/mixins
  /models
  /routes
  /components
```

## Naming

- Name actions descriptively.

```javascript
// good
launchConfirmDialog
submitConfirm
cancelConfirm
```

## Misc

- Don't use jQuery accessors.

- Leverage hardware accelerated properties for animations. For example, use Liquid Fire for transitions.

- Within Cordova/Phonegap applications, abstract general Cordova to a service.

- Return promises. It's a _good_ thing.

## Credits

The Ember Style Guide takes inspiration from the [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide/) and the [Rails Style Guide](https://github.com/bbatsov/rails-style-guide) while the introduction is credited to those guides.
