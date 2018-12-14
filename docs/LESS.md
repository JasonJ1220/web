# LESS
> http://lesscss.org/

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。

## Install
- install LESS
```
npm install -g less
```
- compile LESS
```
lessc styles.less
lessc styles.less styles.css
lessc --clean-css styles.less styles.min.css
```

## Overview
### Variables
LESS
```
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

#header {
  color: @light-blue;
}
```
CSS
```
#header {
  color: #6c94be;
}
```

### Mixins
```
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  /*use bordered inside other rule-sets*/
  .bordered;
}

.post a {
  color: red;
  /*use bordered inside other rule-sets*/
  .bordered;
}
```

### Nested Rules
LESS
```
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

CSS
```
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```

### Nested Directives and Bubbling
Directives such as media or keyframe can be nested in the same way as selectors.
LESS
```
.screen-color {
  @media screen {
    color: green;
    @media (min-width: 768px) {
      color: red;
    }
  }
  @media tv {
    color: black;
  }
}
```
CSS
```
@media screen {
  .screen-color {
    color: green;
  }
}
@media screen and (min-width: 768px) {
  .screen-color {
    color: red;
  }
}
@media tv {
  .screen-color {
    color: black;
  }
}
```

### Operations
```
// numbers are converted into the same units
@conversion-1: 5cm + 10mm; // result is 6cm
@conversion-2: 2 - 3cm - 5mm; // result is 1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // result is 4px

// example with variables
@base: 5%;
@filler: @base * 2; // result is 10%
@other: @base + @filler; // result is 15%
```
### Escaping
Escaping allows you to use any arbitrary string as property or variable value. 
```
.weird-element {
  content: ~"^//* some horrible but needed css hack";
}
```

```
.weird-element {
  content: ^//* some horrible but needed css hack;
}
```

### Functions
```
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```

### Namespaces and Accessors
```
#bundle {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white
    }
  }
  .tab { ... }
  .citation { ... }
}
```
Now if we want to mixin the .button class in our #header a, we can do:
```
#header a {
  color: orange;
  #bundle > .button;
}
```

### Scope
作用域:先找同级变量，如果没找到再向上冒泡找父级的变量；
```
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

### Comments
```
/* One hell of a block
style comment! */
@var: red;

// Get in line!
@var: white;
```

### Importing
```
@import "library"; // library.less
@import "typo.css";
```