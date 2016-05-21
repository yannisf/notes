# sass

Sass is a CSS extension language.

## Comments
Sass supports standard multiline CSS comments with /* */, as well as single-line comments with //. The multiline
comments are preserved in the CSS output where possible, while the single-line comments are removed.

## Variables
Variables is a way to store values and reuses consistently among the stylesheet.

```
$primary-color: black;

body {
  color: $primary-color;
}
```

## Nesting
```
x {
    y {
        prop1: val1;
    }

    z {
        prop2: val2;
    }
}
```

Will generate:

```
x y {
    prop1: val1;
}

x z {
    prop2: val2;
}
```

## Partials and Import

A partial is a Sass file named with a leading underscore. The underscore lets Sass know that the file should not be
generated into a CSS file. Sass partials are used with the @import directive. When importing the _ and the extension
should be omitted.

To import _style.scss:

```
@import 'style';
```

## Mixins
A mixin defines resuable groups of CSS declarations. Values can be pass to the mixin as well. A mixin is created with the
@mixin directive and used with the @include directive

```
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box {
    @include border-radius(10px);
}
```

## Extend
Using @extend a set of CSS properties can be shared from one selector to another (DRY).


```
.x {
    prop1: value1;
    prop2: value2;
}

.y {
    @extend .x;
    prop3: value3;
}

.z {
    @extend .x;
    prop4: value4;
}
```

Generates:

```
.x, .y, .z {
    prop1: value1;
    prop2: value2;
}

.y {
    prop3: value3;
}

.z {
    prop4: value4;
}