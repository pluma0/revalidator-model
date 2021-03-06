**NOTE:** This package is no longer being maintained. If you are interested in taking over as maintainer or are interested in the npm package name, get in touch by creating an issue.

# Synopsis

**revalidator-model** is a simple model library based on [revalidator](https://github.com/flatiron/revalidator).

[![stability 2 - unstable](http://b.repl.ca/v1/stability-2_--_unstable-yellow.png)](http://nodejs.org/api/documentation.html#documentation_stability_index) [![license - Unlicense](http://b.repl.ca/v1/license-Unlicense-lightgrey.png)](http://unlicense.org/) [![Flattr this](https://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=pluma&url=https://github.com/pluma/revalidator-model)

[![Build Status](https://travis-ci.org/pluma/revalidator-model.png?branch=master)](https://travis-ci.org/pluma/revalidator-model) [![Coverage Status](https://coveralls.io/repos/pluma/revalidator-model/badge.png?branch=master)](https://coveralls.io/r/pluma/revalidator-model?branch=master) [![Dependencies](https://david-dm.org/pluma/revalidator-model.png?theme=shields.io)](https://david-dm.org/pluma/revalidator-model)

[![NPM status](https://nodei.co/npm/revalidator-model.png?compact=true)](https://npmjs.org/package/revalidator-model)

# Install

## With NPM

```sh
npm install revalidator-model
```

## From source

```sh
git clone https://github.com/pluma/revalidator-model.git
cd revalidator-model
npm install
make test
```

# Usage Example

```javascript
var model = require('revalidator-model');
var List = model({
    properties: {
        items: {type: 'array'}
    },
    proto: {
        size: function() {
            return this.items.length;
        }
    },
    defaults: {
        flavor: 'pungent'
    },
    hydrate: {
        items: function(val) {
            return val.split(', ');
        }
    },
    dehydrate: {
        items: function(val) {
            return val.join(', ');
        }
    }
});
var list = List.hydrate({items: 'foo, bar, qux', foo: 'bar'});
console.log(list.items); // ['foo', 'bar', 'qux']
console.log(list.size()); // 3
console.log(list.flavor); // 'pungent'
console.log(list.dehydrate()); // {items: 'foo, bar, qux'}
console.log(list.validate()); // {valid: true, errors: []}
var list2 = new List({items: 5, flavor: 'spicy'});
console.log(list2.flavor); // 'spicy';
console.log(list2.items); // 5
console.log(list2.validate().valid); // false
list2.dehydrate(); // fails with error: "Object 5 has no method split."
```

# API

## model(schema:Object):Model

Creates a `Model` with the given revalidator `schema`.

In addition to the properties recognized by `revalidator` (`properties`, `patternProperties`, `additionalProperties`), the `schema` can have the following properties:

### schema.proto:Object (optional)

The `prototype` instances of the `Model` should inherit from. Use this to specify methods you want to have access to on your model's instances.

### schema.defaults:Object (optional)

Default property values to be copied to new instances of this
`Model`. Arrays and objects will be deep-cloned.

### schema.hydrate:Function (optional)

A [transformation](https://github.com/pluma/transform-object) that will be applied to objects processed by your model's `hydrate` method.

### schema.dehydrate:Function (optional)

A [transformation](https://github.com/pluma/transform-object) that will be applied to model instance when processed by its `dehydrate` method.

## new Model(data:Object):Instance

Creates a new instance with the given `data`. Use of the `new` keyword is optional.

Any properties of the given `data` object that are not recognized will be ignored.

## Model.hydrate(data:Object):Instance

See `schema.hydrate`. Hydrates the object from the given `data` and returns a new `Model` instance.

## Model.schema:Object

The `schema` that was used to create this `Model`.

## Model.validate(data:Object):Object

Validates the given `data` against the `Model`'s schema using `revalidator`. The result object has two attributes:

### valid:Boolean

Whether the data passed validation.

### errors:Array

An array of error messages if the validation failed.

## Model#validate():Object

Validates the instance. Shorthand for `Model.validate(instance)`.

## Model#dehydrate():Object

See `schema.dehydrate`. Dehydrates the instance's data and returns it.

# Unlicense

This is free and unencumbered public domain software. For more information, see http://unlicense.org/ or the accompanying [UNLICENSE](https://github.com/pluma/revalidator-model/blob/master/UNLICENSE) file.
