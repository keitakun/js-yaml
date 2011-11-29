JS-YAML - YAML 1.1 parser for JavaScript
========================================

[Online Demo](http://nodeca.github.com/js-yaml/)

This is a native port of [PyYAML](http://pyyaml.org/), the most advanced YAML parser.
Now you can use all modern YAML feature right in JavaScript. Originally snapshoted version - PyYAML 3.10 (2011-05-30).

## Installation

### YAML module for node.js

```
npm install js-yaml
```

### bundled YAML library for browser

``` html
<script src="js-yaml.min.js"></script>
<script type="text/javascript">
var doc = jsyaml.load('greeting: hello\nname: world');
</script>
```

Also we support AMD loaders, e.g. [RequireJS](http://requirejs.org/).

## API

JS-YAML automatically registers handlers for `.yml` and `.yaml` files. You can load them just with `require`.
That's mostly equivalent to calling loadAll() on file handler and picking out the first found document.
Notice, if the file has more than one document, this will silently ignore all
the documents except the first one.

``` javascript
require('js-yaml');

// Get array of documents, or throw exception on error
var doc = require('/home/ixti/example.yml');

console.log(doc);
```


### load (string|buffer|file\_resource)

Parses source as single YAML document. Returns JS object or throws exception on error.

This function does NOT understands multi-doc sources, it throws exception on those.

``` javascript
var yaml = require('js-yaml');

// pass the string
fs.readFile('/home/ixti/example.yml', 'utf8', function (err, data) {
  if (err) {
    // handle error
    return;
  }
  try {
    console.log( yaml.load(data) );
  } catch(e) {
    console.log(e);
  }
});
```


### loadAll (string|buffer|file\_resource, iterator)

Same as `Load`, but understands multi-doc sources and apply iterator to each document.

``` javascript
var yaml = require('js-yaml');

// pass the string
fs.readFile('/home/ixti/example.yml', 'utf8', function (err, data) {
  if (err) {
    // handle error
    return;
  }

  try {
    yaml.loadAll(data, function (doc) {
      console.log(doc);
    });
  } catch(e) {
    console.log(e);
  }
});
```


### safeLoad (string|buffer|file\_resource)

Same as `load()` but uses _safe_ schema - only recommended tags of YAML
specification (no JavaScript-specific tags, e.g. `!!js/regexp`).


### safeLoadAll (string|buffer|file\_resource, iterator)

Same as `loadAll()` but uses _safe_ schema - only recommended tags of YAML
specification (no JavaScript-specific tags, e.g. `!!js/regexp`).


## JavaScript YAML tags scheme

The list of standard YAML tags and corresponding JavaScipt types. See also
[YAML Tag Discussion](http://pyyaml.org/wiki/YAMLTagDiscussion) and [Yaml Types](http://yaml.org/type/).

```
!!null ''                   # null
!!bool 'yes'                # bool
!!int '3...'                # number
!!float '3.14...'           # number
!!binary '...base64...'     # buffer
!!timestamp 'YYYY-...'      # date
!!omap [ ... ]              # array of key-value pairs
!!pairs [ ... ]             # array or array pairs
!!set { ... }               # array of objects with given keys and null values
!!str '...'                 # string
!!seq [ ... ]               # array
!!map { ... }               # object
```

**JavaScript-specific tags**

```
!!js/regexp /pattern/gim    # RegExp
!!js/undefined ''           # Undefined
!!js/func   function () {}  # Function
```

### Caveats

Note, that if you use arrays as key in JS, it's automatically converted to string.
So, if you have maps as key in YAML, result will flat:

``` yaml
---
? - foo
  - bar
: - baz
```

=>

``` javascript
{ "foo,bar": ["baz"] }
```

## License

View the [LICENSE](https://github.com/nodeca/js-yaml/blob/master/LICENSE) file (MIT).
