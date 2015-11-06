phosphor-observablelist
=======================

[![Build Status](https://travis-ci.org/phosphorjs/phosphor-observablelist.svg)](https://travis-ci.org/phosphorjs/phosphor-observablelist?branch=master)
[![Coverage Status](https://coveralls.io/repos/phosphorjs/phosphor-observablelist/badge.svg?branch=master&service=github)](https://coveralls.io/github/phosphorjs/phosphor-observablelist?branch=master)

A sequence container which can be observed for changes.

[API Docs](http://phosphorjs.github.io/phosphor-observablelist/api/)


Package Install
---------------

**Prerequisites**
- [node](http://nodejs.org/)

```bash
npm install --save phosphor-observablelist
```


Source Build
------------

**Prerequisites**
- [git](http://git-scm.com/)
- [node](http://nodejs.org/)

```bash
git clone https://github.com/phosphorjs/phosphor-observablelist.git
cd phosphor-observablelist
npm install
```

**Rebuild**
```bash
npm run clean
npm run build
```


Run Tests
---------

Follow the source build instructions first.

```bash
npm test
```


Build Docs
----------

Follow the source build instructions first.

```bash
npm run docs
```

Navigate to `docs/index.html`.


Supported Runtimes
------------------

The runtime versions which are currently *known to work* are listed below.
Earlier versions may also work, but come with no guarantees.

- Node 0.12.7+
- IE 11+
- Firefox 32+
- Chrome 38+


Bundle for the Browser
----------------------

Follow the package install instructions first.

```bash
npm install --save-dev browserify
browserify myapp.js -o mybundle.js
```


Usage Examples
--------------

**Note:** This module is fully compatible with Node/Babel/ES6/ES5. Simply
omit the type declarations when using a language other than TypeScript.

To observe changes to the list, you can simply hook a callable up to the `changed` signal:

```typescript
let called = false;
let list = new ObservableList<number>();
list.changed.connect(() => { called = true; });

// Insert `1` at index `0`
list.insert(0, 1); // called === true;

console.log(list); // [1]
list.clear();
console.log(list); // []
```

for more advanced behaviour, you can use the args passed by default to the callable:

```typescript
let list = new ObservableList<number>();
list.changed.connect((sender, args) => {
  console.log(args.type); // ListChangeType.Add
  console.log(args.newIndex); // 0
  console.log(args.newValue); // 1
  console.log(args.oldIndex); // -1
  console.log(args.oldValue); // void 0
});
list.add(1); // will give the change args above.
```

You can pass default arguments into the constructor:

```typescript
let list = new ObservableList<number>([1, 1, 2, 3, 5, 8]);
// or
let strlist = new ObservableList<string>(['f', 'i', 'b']);
```

You can retrieve an item at a given index in the list using `.get`:

```typescript
let list = new ObservableList<number>([1, 1, 2, 3, 5, 8]);
list.get(4); // 5

// this will offset from the end of the list if passed
// a negative index
list.get(-2); // 5

// and will return `undefined` if the index is out of range
list.get(101); // void 0
```

`indexOf` works just like arrays in javascript/typescript:

```typescript
let list = new ObservableList<number>([1, 2, 3, 1, 2, 3]);
list.indexOf(2); // 1 - returns the first occurrence.
list.indexOf(4); // -1
```

`contains` returns a boolean to denote whether the item was found:

```typescript
let list = new ObservableList<string>(['a', 'b', 'c']);
list.contains('a'); // true
list.contains('b'); // false
```

`ObservableList` also has `slice` behaviour:

```typescript
let list = new ObservableList<number>([1, 2, 3]);
list.slice(); // [1, 2, 3] - this returns a copy
list.slice(1); // 2
list.slice(-1); // 3
list.slice(4); // []
list.slice(1, -1); // 2
```

To set an item at a given index, use `set`. This returns the item which previously occupied that index:

```typescript
let list = new ObservableList<number>([1, 2, 3, 4]);
list.set(1, 5); // returns 2
console.log(list); // [1, 5, 3, 4]
list.set(-1, 8); // returns 4
console.log(list); // [1, 5, 3, 8]
```

To replace all the items in a list, use `assign`. This also returns the previous items:

```typescript
let list = new ObservableList<string>(['a', 'b', 'c']);
list.assign(['f']); // returns ['a', 'b', 'c']
console.log(list); // 'f'
```

To add items to the list, use `add`:

```typescript
let list = new ObservableList<number>([1, 2, 3]);
list.add(4); // returns 3, the index of the new item.
console.log(list); // [1, 2, 3, 4]
```

To move items internally, use `move`:

```typescript
let list = new ObservableList<number>([1, 2, 3]);

// move item at index `1` to index `2`
list.move(1, 2); // true
console.log(list); // [1, 3, 2]
```

To remove an item from the list, use `remove`. This will remove the first occurrence of the item:

```typescript
let list = new ObservableList<number>([1, 2, 3, 1]);
list.remove(1); // returns 0
console.log(list); [2, 3, 1]
```

To remove an item at a specific index, use `removeAt`:

```typescript
let list = new ObservableList<number>([1, 2, 3]);
list.removeAt(1); // 2
console.log(list); // [1, 3]
```
