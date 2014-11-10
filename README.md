# Turn paths into a JS tree datastructure

The lowdown:

```js
var Treenode = require('paths2tree')({sep: '.'});
var archy = require('archy');

var root = new Treenode('.');
root.push_path('com');
root.push_path('com.google');
root.push_path('com.google.www');
root.push_path('com.google.api');
root.push_path('com.gmail');
root.push_path('com.gmail.www');
root.push_path('com.gmail.smtp');

console.log(archy(root));
```

Output:

	.
	└─┬ com
	  ├─┬ google
	  │ ├── www
	  │ └── api
	  └─┬ gmail
		├── www
		└── smtp

Works for any kind of path separator. If you leave out the option, it
defaults to `require('path').sep`.

```js
// use OS default path separator
var Treenode = require('paths2tree')();
```

Simple.

## The `Treenode` datastructure

The datastructure is very simple, at the core is a tree node, which has
properties:

 - `leaf`: the value at the node, can be anything
 - `label`: string, **must** be present, always (that's what must means).
 - `parent`: parent of this node (`Treenode`) or undefined for the root
 - `nodes`: this nodes children, Array or Treenode`s

It has the following methods:

 - `find_child(label)`: find child node by label
 - `child(label, value)`: insert or update `leaf` property of child with matching label
 - `push_path(path, value)`: deep update a `leaf` value, path is an array of labels, starting at the root.
 - `depth()`: depth of the node (recursive function, so prob stackoverflows with deep trees)
 - `is_leaf()`: true if node has no children
 - `is_last_child()`: true if node is the last child of it's parent


