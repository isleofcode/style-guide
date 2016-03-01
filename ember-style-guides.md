#Ember Guides

Contents:

- General
- Controllers;
- Routes;
- Components;
- Order of Declaration;

##General

- Deconstruct local namespace. Cases such as:

```javascript
import Ember from ‘ember’;

export default Ember.Component.extend({
  title: Ember.computed(‘foo’, function() {
    //something
  })
});
```

should be:

```javascript
import Ember from ‘ember’;

const {
  Component,
  computed
} = Ember;

export default Component.extend({
  title: computed(‘foo’, function() {
    //something
  })
});
```

another one:

```javascript
const {
Component,
inject
} = Ember;

const { service } = inject;

export default Component.extend({
session: inject.service();
});
```

Where there is only one import, don’t newline the const declaration:

```javascript
const { Component } = Ember;

export default Component.extend();
```

This pattern also serves as a handy tool for developers to know what you actually used in a file;

- Avoid importing properties from other files. For example, a constant on Model A should not be imported to Model B in most cases;

- Don't use jQuery accessors;

- Animations should leverage hardware accelerated properites. For transitions use Liquid Fire;

- Components  on’t need to be wrapped in a div, they are divs by default. Use the classNames and/or tagName property to modify a component before wrapping it;

- Don't over nest your components. Consider if your nesting adds better details or obfuscation for new developers;

- For mobile, remember that heavy DOM nesting negatively impacts
  performance;

- Within Cordova/Phonegap applications, abstract general Cordova to a service;

- In general, you want to rollbackAttributes on route exit for model edit pages;

- Returning promises is a _good_ thing;

 Don’t alias your models as model, or use the auto set ‘model’ property on a controller. Model is not descriptive, and makes you wonder ‘which model?’;

- Action names should be descriptive, and are usually verbNoun;

- Follow the paradigm that components handle their internal state and routes handle application state;

- Where possible, declare a single Application Serializer. When mangling/changes are required, inherit ApplicationSerializer vs the one in use (e.g. javascriptON Serializer);

- Same as above for Adapters.;

- RTFM: guides.emberjavascript.com;


##Controllers

Controllers in general should not be used. Most controllers should will like this:


```javascript
import Ember from 'ember';

const { Controller } = Ember;

export default Controller.extend({
attrs: {}
});
```

We don’t do much more here. It is ok to sometimes add some code, for example a small computed property to filter an item. Another exception is obviously queryParams.

Sometimes we still use Controllers in mobile where the saved nesting
noticeably improves performnance.

##Routes

- Instead of setting a model directly on a controller (e.g. controller.modelName), use an attrs hook. This also makes it easy to distinguish items that _are_ on the controller proper.

An example route:

```javascript
export default Route.extend({
  model() {
    return this.store.find(‘contact’);
  },

  setupController(controller, model) {
    controller.set(‘attrs.contact’, model);
  }
});
```

And in your templates, you can access attrs.contact. 

- For multi model routes, leverage RSVP.hash:
```javascript
export default Route.extend({
  model() {
    return RSVP.hash({
      followers: this.store.find(‘follower’),
      following: this.store.find(‘following’)
    }),
  },

  setupController(controller, model) {
    controller.set(‘attrs’, model);
  }   
});
```

##Mixins

Always newline after Mixin declarations. For example:

```javascript
export default Component.extend(
  Mixin, {

  //code here
});
```

```javascript
export default Component.extend(
  Mixin1,
  Mixin2 {

  //code here
});
```

###Directory Structure:

```
/mixins
  /models
  /routes
  /components
```

##Components:

###Component Declarations:
Align component / other hbs style properties as such:

```hbs
{{component-name
  property=‘foo’
  action=(action ‘actionName’)}}
```

The order of declaration for a component is always properties, and then actions

##Order of Declaration

The highest level  order is:

* 1) Services;
* 2) Native Properties; and
* 3) Custom Properties.

Lifecycle hooks (e.g. model, setupController) should be in order of execution.

###Components:
* 1) Services;
* 2) Native component properties;
* 3) Passed / custom properties;
* 4) Lifecycle hooks;
* 5) Computed properties;
* 6) Any native functions; and
* 7) Actions.

Create a newline after each, and always line separate actions. So for example

```javascript
export default Component.extend({
  tagName: ‘li’,
  classNames: ‘list-items’,

  title: ‘Passed title’,

  doSomething() {
  }.on(‘didInsertElement’),

  funkyTitle: computed(‘title’, function() {
    const title = this.get(‘title’);
    return `FUNKY ${title}`;
  },

  actions: {
    actionOne() {
    },

    actionTwo() {
    }
});
```

###Routes:
* 1) Service declarations;
* 2) Lifecycle hooks;
* 3) Any custom functions; and
* 4) Actions


###Models:
* 1) Service declarations
* 2) attrs;
* 3) Relationships;
* 4) Computed Properties; and
* 5) Custom functions.

```javascript
export default Model.extend({
  title: attr(‘string’),
  subTitle: attr(‘string’),

  comments: hasMany(‘comment’),
  author: belongsTo(‘user’),

  someProperty: computed(‘title’, function() {
    //do something
  });
});
```
