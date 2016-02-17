phosphor-observablelist
=======================

[![Build Status](https://travis-ci.org/phosphorjs/phosphor-observablelist.svg)](https://travis-ci.org/phosphorjs/phosphor-observablelist?branch=master)
[![Coverage Status](https://coveralls.io/repos/phosphorjs/phosphor-observablelist/badge.svg?branch=master&service=github)](https://coveralls.io/github/phosphorjs/phosphor-observablelist?branch=master)

This module provides a sequence container which can be observed for changes. Additional methods to manipulate lists, insert and move elements, are also included.


<a name='install'></a>Package Install
-------------------------------------

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

Any bundler that understands how to `require()` files with .js and .css
extensions can be used with this package.


Usage Examples
--------------

**Note:** This module is fully compatible with Node/Babel/ES6/ES5. Simply
omit the type declarations when using a language other than TypeScript.

To test the `phosphor-queue` module in a node interactive shell after the
[installation](#install), open a terminal in your current working directory and
run:

```bash
node
```

Then import the module into node with the following command:

```node
> observablelist = require('phosphor-observablelist');
```

The `ObservableList()` constructor can now be used to create new lists:

```node
> let list = new observablelist.ObservableList([1, 1, 2, 3, 5, 8]);

> let strlist = new observablelist.ObservableList(['f', 'i', 'b', 'j']);
```

Two basic methods of Observable Lists are `get()` and `set()`. The `get()`
method retrieves a value at a specific index, if the index is negative it
offsets from the end of the list, indexes out of range return `undefined`. The
`set()` method, on the other hand, sets the value of an item at a given
position, the same index rules apply in this case.

```node
> list.get(4)
5
> list.get(-2)
5
> list.get(100)
undefined

> list.set(1, 5)
1
> list.set(-2, 8)
5

> list
[ 1, 5, 2, 3, 8, 8 ]
```

There are other methods to retrieve information about the elements in Observable
Lists: `indexOf()` looks for its argument in the list and returns the index of
the first occurrence, `contains()` returns `true` if it the given argument is
in the list and `false` otherwise.

```node
> let strlist1 = new obslist.ObservableList(['f', 'i', 'b', 'j']);

> strlist.indexOf('b');
2
> strlist.indexOf('p');
-1

> strlist.contains('b');
true
> strlist.contains('p');
false
```

Observable Lists also support slicing by means of the `slice()` method which
takes up to 2 arguments. If no argument is passed it returns a copy of the
list, if only one argument is passed it returns a slice from that index to the
end of the list, two arguments determine the lower and upper indexes for the
slice.

```node
> let list = new obslist.ObservableList([1, 2, 3, 1, 2, 3]);

> list.slice()
[ 1, 2, 3, 1, 2, 3 ]
> list.slice(2)
[ 3, 1, 2, 3 ]
> list.slice(1)
[ 2, 3, 1, 2, 3 ]
> list.slice(-1)
[ 3 ]
> list.slice(1, -1)
[ 2, 3, 1, 2 ]
```

There are additional methods to alter a `ObservableList`, the method names are
somewhat self-descriptive.  `add()` adds a new item and returns its index,
`move()` takes two arguments and moves the element at the fist position to the
second position returning `true` if the operation was successful. To remove the
first occurrence a given value use `remove()` which will return the
corresponding index, if any. Elements at a specific position are removed by
`removeAt()`, which takes as argument the index, removes it and returns the
deleted element. To clear a list and remove the elements altogether use
`clear()`.

```node
> let list = new obslist.ObservableList([1, 2, 3, 1, 2, 3]);

> list.add(5);
6
> list;
[ 1, 2, 3, 1, 2, 3, 5 ]

> list.move(1, -1)
true
> list
[ 1, 3, 1, 2, 3, 5, 2 ]

> list.remove(3);
1
> list
[ 1, 1, 2, 3, 5, 2 ]
> list.removeAt(-1);
2
> list;
[ 1, 1, 2, 3, 5 ]

> list.clear();
[ 1, 1, 2, 3, 5 ]
> list
[]

```

The main advantage of using `ObservableList` is that the resulting object can be
observed for changes. To do this you have to hook a callable up to the
`changed` signal. For instance, we can use a Boolean variable `called` to check
whether or not the list has been changed:

```node

> let list = new obslist.ObservableList([1, 2, 3, 4, 1, 2, 3]);

> let called = false;
> list.changed.connect(() => { called = true; });

> called
false
> list.insert(0, 5);
0
> called
true
```

This basic monitoring can be further expanded, you can use the arguments passed
by default to the callable;

```node
> list.changed.connect((sender, args) => {
... console.log(args.type);
... console.log(args.newIndex);
... console.log(args.newValue);
... console.log(args.oldIndex);
... console.log(args.oldValue);
... });

> list.add(9);
0
8
9
-1
undefined
8
```

API
---
[API Docs](http://phosphorjs.github.io/phosphor-observablelist/api/)
