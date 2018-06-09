# Let's talk about javascript

## Transpilers, polyfills and shims

A _transpiler_ is program that transforms a program. Typically, this transformation is required in Javascript as
a javascript engine might not support a specific syntax that a program is written with. The transpiler translates
the new/unsupported/unknown syntax to one that the target system will support. As example consider ES6 syntax
(e.g. the fat arrow notation). While only a few browsers supported natively the ES6 syntax, developers were already
using its features thanks to transpilers.

A _polyfill_ is program that extends the existing API to include methods and operation not yet available in a
javascript engine. This is achieved using the dynamic nature of javascript. Polyfills primary are concerned with
APIs while transpilers with syntax.

A _shim_ works like a polyfill but typically does not extend an API but more like reimplements to fix its shortcommings
and defiencies.
