---
title: Tuples and Records
date: "2021-03-18T12:40:32.169Z"
template: "post"
draft: false
slug: "tuples-and-records"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

### Tuples
Both tuples and records have the same syntax. They can be defined by using a # prefix in front of objects and arrays, like so:

```JavaScript
// This is a normal array
const arr = [];

// This is a tuple
const tuple = #[];
```

When working with tuples, there are some rules you need to be aware of:

There cannot be holes in an array, eg.: [1, ,2] is disallowed

They can only contain primitives or other tuples and records

Supports instance methods similar to Arrays, but with a few changes

For example, operations that mutate the array are replaced with new operations that instead, return a new array. Therefore, eg.: there's no push, instead you can use pushed that returns a new tuple with the pushed value, or with to change a value at a given index:

```JavaScript
const tuple = #['🍄', '🍅', '🥕'];

// Both returns a new tuple
tuple.pushed('🥒');  // returns #['🍄', '🍅', '🥕', '🥒'];
tuple.with(0, '🌳'); // returns #['🌳', '🍅', '🥕']
tuple.js
You can also create tuples from existing arrays using Tuple.from():

Tuple.from(['🍄', '🍅', '🥕']);

// Likewise, you can turn a tuple into an ordinary array:
Array.from(tuple);
tuple.js
And of course, they are immutable and will throw an error if you try to change their value or use non-primitives:

const tuples = #['🍄', '🍅', '🥕'];

// TypeError: Callback to Tuple.prototype.map may only return primitives, Records or Tuples
tuples.map(tuple => new Button(tuple));
```

---

### Records
Just like tuples, records are also denoted by a hash:

```JavaScript
// This is a regular object
const obj = { ... };

// This is a record
const record = #{
    tuple: #['🍄', '🍅', '🥕'] // Records can also contain tuples
};
```
When working with records, you also need to keep in mind some rules:

You cannot use the `__proto__` identifier in records
Methods are also disallowed. Just like tuples, they can only contain primitives.
To create a new record, you also have the option to use Record, or Record.fromEntries when working with tuples:

```JavaScript
const record = Record({
    mushroom: '🍄',
    tomato: '🍅',
    carrot: '🥕'
});

// Or
const record = Record.fromEntries(#['🍄', '🍅', '🥕']);
record.js
And since they are new data types, you would get "record" back when using the typeof operator:

typeof #{ ... } // returns "record"
typeof #[ ... ] // returns "tuple"
```
