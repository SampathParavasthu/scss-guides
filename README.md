# scss-guides
List of all scss guides - tips and tricks.

Use [SassMeister](https://www.sassmeister.com/) - The live Sass Playground.


## Table of Contents

1. [Variables](#variables)
1. [Utilities](#utilities)
1. [Inheritance](#inheritance)
1. [Mixins](#mixins)
1. [Import](#import)
1. [List Functions](#list-functions)
1. [Maps](#maps)
1. [Math Operators and Functions](#math-operators-and-functions)
1. [If Directives](#if-directives)
1. [Loops](#loops)
1. [Function Directives](#function-directives)
1. [Additional resources for readings](#additional-resources-for-readings)

## Variables

*scss*

```scss

// Numbers
$width: 200px;
$float: 1.5;
$int: 20;

// Strings
$string: "images/hello.png";
$stringTwo: no-repeat;
$stringSelector: p;

// Colors
$color: gray;
$colorTwo: #ccc;
$colorThree: rgb(230, 230, 230);

// Special cases
$list: 1px 1px 1px black, 
       3px 3px 3px red;
$map: (
    "key1": "value1",
    "color": blue
);
$bool: true;
$null: null;


/////////////// USAGE ///////////////
div {
    width: $width;
    background: url($string) $stringTwo;
    box-shadow: $list;
}

div #{$stringSelector} {
    color: $color;
    background: $colorThree;
    padding: $width / $int;
}

```

*css*

```css

div {
  width: 200px;
  background: url("images/hello.png") no-repeat;
  box-shadow: 1px 1px 1px black, 3px 3px 3px red;
}

div p {
  color: gray;
  background: #e6e6e6;
  padding: 10px;
}

```

**[⬆ back to top](#table-of-contents)**

## Utilities

- **@degug**

Sometimes it’s useful to see the value of a variable or expression while you’re developing your stylesheet. That’s what the @debug rule is for: it’s written @debug <expression>, and it prints the value of that expression in the console, along with the filename and line number.

*scss*

```scss
$length: 2;

@debug "length here is #{$length}";
```

**[⬆ back to top](#table-of-contents)**

## Inheritance

- **With descendent selectors**

*scss*

```scss

#parent {
    background: green;
    
    #child-one {
        background: yellow;
        font-size: 4px;
    }
    
    &.child-two {
        padding: 10px;
    }
    
    #child-three, .child-four {
        border: 1px solid red;
    }
    
    button {
        background: red;
        &:hover {
            background: green;
        }
    }
}

```

*css*

```css

#parent {
  background: green;
}

#parent #child-one {
  background: yellow;
  font-size: 4px;
}

#parent.child-two {
  padding: 10px;
}

#parent #child-three, #parent .child-four {
  border: 1px solid red;
}

#parent button {
  background: red;
}

#parent button:hover {
  background: green;
}

```

- **With `@extend` keyword**

*scss*

```scss

$selectors-list: h1,h2,h3,h4,h5,h6;

%shared-one{
    font-size: 2rem;
}

%shared-two{
    box-sizing: border-box;    
}

%headers{
    color: tomato;
}

.para{
    @extend %shared-one;
}

.class-three {
    padding: 10px;
    @extend %shared-one;
}

.class-one, .class-two {
    @extend %shared-two;
}

#{$selectors-list}{
    @extend %headers;
}

```

*css*

```css

.class-three, .para {
  font-size: 2rem;
}

.class-one, .class-two {
  box-sizing: border-box;
}

.class-three {
  padding: 10px;
}

h1, h2, h3, h4, h5, h6 {
  color: tomato;
}

```

**[⬆ back to top](#table-of-contents)**

## Mixins

- **Without any arguments**

*scss*

```scss

@mixin flex-box {
    display: flex;
}

.container {
    @include flex-box();
}

```

*css* 

```css

.container {
  display: flex;
}

```

- **With argument**

*scss*

```scss

@mixin flex-box($direction) {
    display: flex;
    flex-direction: $direction;
}

.container {
    @include flex-box(row);
}

```

*css*

```css

.container {
  display: flex;
  flex-direction: row;
}

```

- **With arguments and has default value**

*scss*

```scss

@mixin flex-box($direction: column) {
    display: flex;
    flex-direction: $direction;
}

.container {
    @include flex-box();
}

```

*css*

```css

.container {
  display: flex;
  flex-direction: column;
}

```

- **With multiple arguments**

*scss*

```scss

$width: 100px;

@mixin box($x, $y) {
    width: $x;
    height: $y;
}

.container {
    @include box($width, 50px);
}

```

*css*

```css

.container {
  width: 100px;
  height: 50px;
}

```

- **With `@content` directive**

*scss*

```scss

@mixin customKeyframes($name){
    @keyframes #{$name}{
        @content;
    }
}


@include customKeyframes(toast){
    0%{
        opacity: 0;
    }
    100% {
        opacity: 100%;
    }
}

```

*css*

```css

@keyframes toast {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 100%;
  }
}

```

**[⬆ back to top](#table-of-contents)**

## Import


- **Importing css file**


*scss*

```scss
@import 'base.css';

```

*css*


```css
@import url(base.css);
```

- **Importing scss partial file**

*scss*

```scss
/* _main.scss file - partial file */
$bgColor: white;

@mixin flex-box(){
  display: flex;
}

div {
  padding: 20px;
}
```

```scss
/* base.scss */
@import '_main.scss';

p {
  background-color: red;
  @include flex-box();
  color: $bgColor;
}

```

*css*

```css
/* base.css */

div {
  padding: 20px;
}

p {
  background-color: red;
  display: flex;
  color: white;
}

```

**[⬆ back to top](#table-of-contents)**

## List Functions

*list types*

```scss

$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

```

- **length**

Returns the length of $list.

*syntax*

```scss
length($list) //=> number 
```

*scss*

```scss
$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

html {
  font-size: length($list1);
  font-size: length($list2);
  font-size: length($list3);
  font-size: length($list4);
}
```

*css*

```css
html {
  font-size: 1;
  font-size: 3;
  font-size: 3;
  font-size: 3;
}
```


Reference - [Official docs for length](https://sass-lang.com/documentation/modules/list#length)

- **nth**

Returns the element of $list at index $n.

If $n is negative, it counts from the end of $list. Throws an error if there is no element at index $n.

*syntax*

```scss
nth($list, $n) 
```

*scss*

```scss
$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

html {
    font-size: nth($list1, 1);
    font-size: nth($list2, 2);
    font-size: nth($list3, 3);
    font-size: nth($list4, 1);
}
```

*css*

```css
html {
  font-size: 100px;
  font-size: 200px;
  font-size: sans-serif;
  font-size: 1px 1px 3px black;
}
```


Reference - [Official docs for nth](https://sass-lang.com/documentation/modules/list#nth)

- **set-nth**

Returns a copy of $list with the element at index $n replaced with $value.

If $n is negative, it counts from the end of $list. Throws an error if there is no existing element at index $n.

*syntax*

```scss
set-nth($list, $n, $value) //=> list 
```

*scss*

```scss
$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

html {
    font-size: set-nth($list1, 1, 200px);
    font-size: set-nth($list2, 2, 10px);
    font-size: set-nth($list3, 3, open-sans);
    font-size: set-nth($list4, 1, 3px 2px 1px red);
}
```

*css*

```css
html {
  font-size: 200px;
  font-size: 100px 10px 300px;
  font-size: Helvetica, Arial, open-sans;
  font-size: 3px 2px 1px red, 3px 1px 3px black, 1px 1px 3px black;
}
```


Reference - [Official docs for set-nth](https://sass-lang.com/documentation/modules/list#set-nth)

- **list-separator**

Returns the name of the separator used by $list, either space, comma, or slash.

If $list doesn’t have a separator, returns space.

*syntax*

```scss
list-separator($list) //=> unquoted string 
```

*scss*

```scss
$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

html {
    font-size: list-separator($list1);
    font-size: list-separator($list2);
    font-size: list-separator($list3);
    font-size: list-separator($list4);
}
```

*css*

```css
html {
  font-size: space;
  font-size: space;
  font-size: comma;
  font-size: comma;
}
```


Reference - [Official docs for list-separator](https://sass-lang.com/documentation/modules/list#separator)

- **join**

Returns a new list containing the elements of $list1 followed by the elements of $list2.

If $separator is comma, space, or slash, the returned list is comma-separated, space-separated, or slash-separated, respectively. If it’s auto (the default), the returned list will use the same separator as $list1 if it has a separator, or else $list2 if it has a separator, or else space. Other values aren’t allowed.

If $bracketed is auto (the default), the returned list will be bracketed if $list1 is. Otherwise, the returned list will have square brackets if $bracketed is truthy and no brackets if $bracketed is falsey.

*syntax*

```scss
join($list1, $list2, $separator: auto, $bracketed: auto) //=> list 
```

*scss*

```scss
$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

$list_with_comma_at_end: 100px 200px 300px,;

html {
    font-size: join($list1, $list2, comma);
    font-size: join($list2, $list3, space);
    font-size: join($list2, $list4);
    font-size: join($list_with_comma_at_end, $list4);
}
```

*css*

```css
html {
  font-size: 100px, 100px, 200px, 300px;
  font-size: 100px 200px 300px Helvetica Arial sans-serif;
  font-size: 100px 200px 300px 1px 1px 3px black 3px 1px 3px black 1px 1px 3px black;
  font-size: 100px 200px 300px, 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;
}
```

Reference - [Official docs for join](https://sass-lang.com/documentation/modules/list#join)

- **append**

Returns a copy of $list with $val added to the end.

If $separator is comma, space, or slash, the returned list is comma-separated, space-separated, or slash-separated, respectively. If it’s auto (the default), the returned list will use the same separator as $list (or space if $list doesn’t have a separator). Other values aren’t allowed.

Note that unlike list.join(), if $val is a list it’s nested within the returned list rather than having all its elements added to the returned list.

*syntax*

```scss
append($list, $val, $separator: auto) //=> list 
```

*scss*

```scss
$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

html {
    font-size: append($list1, $list2);
    font-size: append($list2, black);
    font-size: append($list3, open-sans, space);
    font-size: append($list3, open-sans);
}
```

*css*

```css
html {
  font-size: 100px 100px 200px 300px;
  font-size: 100px 200px 300px black;
  font-size: Helvetica Arial sans-serif open-sans;
  font-size: Helvetica, Arial, sans-serif, open-sans;
}
```

Reference - [Official docs for append](https://sass-lang.com/documentation/modules/list#append)

- **index**

Returns the index of $value in $list.

If $value doesn’t appear in $list, this returns null. If $value appears multiple times in $list, this returns the index of its first appearance.

*syntax*

```scss
index($list, $value) //=> number | null
```

*scss*

```scss
$list1: 100px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 1px 1px 3px black, 3px 1px 3px black, 1px 1px 3px black;

html {
    font-size: index($list2, 200px);
    font-size: index($list2, $list1);
    font-size: index($list3, Arial);
    font-size: index($list4, 1px 1px 3px black);
    font-size: index($list4, 0px); // null will be returned
}
```

*css*

```css
html {
  font-size: 2;
  font-size: 1;
  font-size: 2;
  font-size: 1;
}
```

Reference - [Official docs for index](https://sass-lang.com/documentation/modules/list#index)

- **zip**

Combines every list in $lists into a single list of sub-lists.

Each element in the returned list contains all the elements at that position in $lists. The returned list is as long as the shortest list in $lists.

The returned list is always comma-separated and the sub-lists are always space-separated.

*syntax*

```scss
zip($lists...) //=> list 
```

*scss*

```scss
$list1: 500px;
$list2: 100px 200px 300px;
$list3: Helvetica, Arial, sans-serif;
$list4: 10px 50px 100px;
$list5: short mid long;
$list6: a b;
$list7: 1 2;


html {
    font-size: zip($list4, $list5);
    font-size: zip($list6, $list7);
    font-size: zip($list2, $list1);
    font-size: zip($list3, open-sans);
}
```

*css*

```css
html {
  font-size: 10px short, 50px mid, 100px long;
  font-size: a 1, b 2;
  font-size: 100px 500px;
  font-size: Helvetica open-sans;
}
```

Reference - [Official docs for zip](https://sass-lang.com/documentation/modules/list#zip)

**[⬆ back to top](#table-of-contents)**

## Maps

*map types*

```scss

$map1: (
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3'
);

$map2: ( 'color1': red, 'color2': green);

$map3: (
    'stringKey': 'stringValue',
    'numberKey': 20,
    'list': (100px, 200px),
    'nestedMap': (
        'color': red,
        'url': 'image/photo.png'
    )
);

```

- **map-keys**

Returns a comma-separated list of all the keys in $map.

*syntax*

```scss
map-keys($map) //=> list 
```

*scss*

```scss
$map1: (
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3'
);

$map2: ( 'color1': red, 'color2': green);

$map3: (
    'stringKey': 'stringValue',
    'numberKey': 20,
    'list': (100px, 200px),
    'nestedMap': (
        'color': red,
        'url': 'image/photo.png'
    )
);

html {
    list: map-keys($map1);
    list: map-keys($map2);
    list: map-keys($map3);
}
```

*css*

```css
html {
  list: "key1", "key2", "key3";
  list: "color1", "color2";
  list: "stringKey", "numberKey", "list", "nestedMap";
}
```

Reference - [Official docs for map-keys](https://sass-lang.com/documentation/modules/map#keys)

- **map-values**

Returns a comma-separated list of all the values in $map.

*syntax*

```scss
map-values($map) //=> list 
```

*scss*

```scss
$map1: (
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3'
);

$map2: ( 'color1': red, 'color2': green);

$map3: (
    'stringKey': 'stringValue',
    'numberKey': 20,
    'list': (100px, 200px),
    'nestedMap': (
        'color': red,
        'url': 'image/photo.png'
    )
);

html {
    list: map-values($map1);
    list: map-values($map2);
    list: map-values($map3); // won't work for nested maps
}
```

*css*

```css
html {
  list: "value1", "value2", "value3";
  list: red, green;
}
```

Reference - [Official docs for map-values](https://sass-lang.com/documentation/modules/map#values)

- **map-has-key**

If $keys is empty, returns whether $map contains a value associated with $key.

*syntax*

```scss
map-has-key($map, $key, $keys...) //=> boolean 
```

*scss*

```scss
$map1: (
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3'
);

$map2: ( 'color1': red, 'color2': green);

$map3: (
    'stringKey': 'stringValue',
    'numberKey': 20,
    'list': (100px, 200px),
    'nestedMap': (
        'color': red,
        'url': 'image/photo.png'
    )
);

html {
  boolean: map-has-key($map1, 'key1');
  boolean: map-has-key($map2, 'color1');
  boolean: map-has-key($map3, 'color');
  boolean: map-has-key($map3, 'nestedMap', 'color');
}
```

*css*

```css
html {
  boolean: true;
  boolean: true;
  boolean: false;
  boolean: true;
}
```

Reference - [Official docs for map-has-key](https://sass-lang.com/documentation/modules/map#has-key)

- **map-get**

If $keys is empty, returns the value in $map associated with $key.

If $map doesn’t have a value associated with $key, returns null.

*syntax*

```scss
map-get($map, $key, $keys...) 
```

*scss*

```scss
$map1: (
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3'
);

$map2: ( 'color1': red, 'color2': green);

$map3: (
    'stringKey': 'stringValue',
    'numberKey': 20,
    'list': (100px, 200px),
    'nestedMap': (
        'color': red,
        'url': 'image/photo.png'
    )
);

html {
  value: map-get($map1, 'key1');
  value: map-get($map2, 'color1');
  value: map-get($map3, 'list');
  value: map-get($map3, 'nestedMap', 'url');
}
```

*css*

```css
html {
  value: "value1";
  value: red;
  value: 100px, 200px;
  value: "image/photo.png";
}
```

Reference - [Official docs for map-get](https://sass-lang.com/documentation/modules/map#get)

- **map-merge**

If no $keys are passed, returns a new map with all the keys and values from both $map1 and $map2.

If both $map1 and $map2 have the same key, $map2‘s value takes precedence.

All keys in the returned map that also appear in $map1 have the same order as in $map1. New keys from $map2 appear at the end of the map.

*syntax*

```scss
map-merge($map1, $map2)
map-merge($map1, $keys..., $map2) //=> map 
```

*scss*

```scss
$map1: (
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3'
);

$map2: ( 'color1': red, 'color2': green);

$map3: (
    'stringKey': 'stringValue',
    'numberKey': 20,
    'list': (100px, 200px),
    'nestedMap': (
        'color': red,
        'url': 'image/photo.png'
    )
);

$mergedMap1: map-merge($map1, $map2);
$mergedMap2: map-merge($map2, $map3);

html {
  keys: map-keys($mergedMap1);
  value: map-values($mergedMap1);
}

div {
    keys: map-keys($mergedMap2); // no nested keys will be returned
    values: map-values($mergedMap2); // does not work for nested maps
}
```

*css*

```css
html {
  keys: "key1", "key2", "key3", "color1", "color2";
  value: "value1", "value2", "value3", red, green;
}

div {
  keys: "color1", "color2", "stringKey", "numberKey", "list", "nestedMap";
}
```

Reference - [Official docs for map-merge](https://sass-lang.com/documentation/modules/map#merge)

- **map-remove**

Returns a copy of $map without any values associated with $keys.

If a key in $keys doesn’t have an associated value in $map, it’s ignored.

*syntax*

```scss
map-remove($map, $keys...) //=> map 
```

*scss*

```scss
$map1: (
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3'
);

$map2: ( 'color1': red, 'color2': green);

$map3: (
    'stringKey': 'stringValue',
    'numberKey': 20,
    'list': (100px, 200px),
    'nestedMap': (
        'color': red,
        'url': 'image/photo.png'
    )
);

$removedMap1: map-remove($map2, 'color1');
$removedMap2: map-remove($map1, 'key1', 'key2');
$removedMap3: map-remove($map3, 'nestedMap', 'color'); // does not work for nested maps

html {
  keys: map-keys($removedMap1);
  value: map-values($removedMap1);
}

div {
    keys: map-keys($removedMap2);
    value: map-values($removedMap2);
}

p {
    keys: map-keys($removedMap3);
}
```

*css*

```css
html {
  keys: "color2";
  value: green;
}

div {
  keys: "key3";
  value: "value3";
}

p {
  keys: "stringKey", "numberKey", "list";
}
```

Reference - [Official docs for map-remove](https://sass-lang.com/documentation/modules/map#remove)

**[⬆ back to top](#table-of-contents)**

## Math Operators and Functions

*math operators types*

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

// Addition
div {
    width: $a + $b;
    width: #{($a + $b)}px;
    width: $x + $y;
    width: $a + $x;
    width: $a + 50;
}

// Subtraction
div {
    width: $b - $a;
    width: #{($b - $a)}px;
    width: $y - $x;
    width: $x - $y;
    width: $x - $a;
    width: $z - 50;
}

// Multiplication
div {
    width: $b * $a;
    width: #{($b * $a)}px;
    width: $z * 50;
    width: $x * $a;
    //width: $y * $x; // invalid
}

// Division
div {
    width: $b / $a;
    width: #{($b / $a)}px;
    width: $z / 50;
    width: $x / $a;
    width: $y / $x; // px will be stripped
}

// Combined
div {
    width: $b * $a + $x;
    width: $b * $a + $x * $c / $z; // px will be stripped due to division
    width: $a + $b + ($x * $c); // bracket will be calculated first, then rest will proceed
}
```

*css*

```css
// Addition
div {
  width: 30;
  width: 30px;
  width: 170px;
  width: 90px;
  width: 60;
}

// Subtraction
div {
  width: 10;
  width: 10px;
  width: 10px;
  width: -10px;
  width: 70px;
  width: 50px;
}

// Multiplication
div {
  width: 200;
  width: 200px;
  width: 5000px;
  width: 800px;
}

// Division
div {
  width: 2;
  width: 2px;
  width: 2px;
  width: 8px;
  width: 1.125;
}

// Combined
div {
  width: 280px;
  width: 224;
  width: 2430px;
}

```

- **round**

Rounds $number to the nearest whole number.

*syntax*

```scss
round($number) //=> number 
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: round($eq1);
    width: round($eq2);
}

```

*css*

```css
div {
  width: 2;
  width: 3px;
}
```

Reference - [Official docs for round](https://sass-lang.com/documentation/modules/math#round)

- **floor**

Rounds $number down to the next lowest whole number.

*syntax*

```scss
floor($number) //=> number 
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: floor($eq1);
    width: floor($eq2);
}


```

*css*

```css
div {
  width: 1;
  width: 2px;
}
```

Reference - [Official docs for floor](https://sass-lang.com/documentation/modules/math#floor)

- **ceil**

Rounds $number up to the next highest whole number.

*syntax*

```scss
ceil($number) //=> number 
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: ceil($eq1);
    width: ceil($eq2);
}

```

*css*

```css
div {
  width: 2;
  width: 3px;
}
```

Reference - [Official docs for ceil](https://sass-lang.com/documentation/modules/math#ceil)

- **percentage**

Converts a unitless $number (usually a decimal between 0 and 1) to a percentage.

*syntax*

```scss
percentage($number) //=> number 
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: percentage($eq1);
    width: percentage(0.4);
    width: percentage($c);
    width: percentage($b / $a);
    //width: percentage($eq2); // invalid
}
```

*css*

```css
div {
  width: 150%;
  width: 40%;
  width: 3000%;
  width: 200%;
}
```

Reference - [Official docs for percentage](https://sass-lang.com/documentation/modules/math#percentage)

- **abs**

Returns the absolute value of $number. If $number is negative, this returns -$number, and if $number is positive, it returns $number as-is.

*syntax*

```scss
abs($number) //=> number 
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: abs($eq1);
    width: abs(-4px);
    width: abs($c - $x);
    width: abs($eq2);
}

```

*css*

```css
div {
  width: 1.5;
  width: 4px;
  width: 50px;
  width: 2.6666666667px;
}
```

Reference - [Official docs for abs](https://sass-lang.com/documentation/modules/math#abs)

- **min**

Returns the lowest of one or more numbers.

*syntax*

```scss
min($number...) //=> number 
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: min($eq1, $a, $b);
    width: min($x, $y, $z);
    width: min($x, $y, $b, $c);
}
```

*css*

```css
div {
  width: 1.5;
  width: 80px;
  width: 20;
}
```

Reference - [Official docs for min](https://sass-lang.com/documentation/modules/math#min)

- **max**

Returns the lowest of one or more numbers.

*syntax*

```scss
max($number...) //=> number 
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: max($eq1, $a, $b);
    width: max($x, $y, $z);
    width: max($x, $y, $b, $c);
}
```

*css*

```css
div {
  width: 20;
  width: 100px;
  width: 90px;
}
```

Reference - [Official docs for max](https://sass-lang.com/documentation/modules/math#max)

- **random**

If $limit is null, returns a random decimal number between 0 and 1.

*syntax*

```scss
random($limit: null) //=> number
```

*scss*

```scss
$a: 10;
$b: 20;
$c: 30;

$x: 80px;
$y: 90px;
$z: 100px;

$eq1: $c / $b; // 1.5
$eq2: $x / $c; // 2.6666666667px


div {
    width: random();
    width: random(5);
    width: random(4);
}
```

*css*

```css
div {
  width: 0.1495831532;
  width: 5;
  width: 1;
}
```

Reference - [Official docs for random](https://sass-lang.com/documentation/modules/math#random)

**[⬆ back to top](#table-of-contents)**

## If Directives

- **@if**

*scss*

```scss
$element: h1;
$number: 10;
$sum: 10 + 2;

html {
    @if ($number == 10){
        width: #{$number}px;
    }
}

@if ($element == h1){
    #{$element}{
        padding: 10px;
    }
}
```

*css*

```css
html {
  width: 10px;
}

h1 {
  padding: 10px;
}
```

- **@else**

*scss*

```scss
$element: h1;
$number: 10;
$sum: 10 + 2;

html {
    @if ($number != 10){
        width: #{$number}px;
    }
    @else {
        width: 12px;
    }
}

@if ($element != h1){
    #{$element}{
        padding: 10px;
    }
}
@else {
    h3{
        padding: 15px;
    }
}
```

*css*


```css
html {
  width: 12px;
}

h3 {
  padding: 15px;
}
```

- **@else if**

*scss*

```scss
$element: h1;
$number: 10;
$sum: 10 + 2;

html {
    @if ($number != 10){
        width: #{$number}px;
    }
    @else if ($number == 11) {
        width: 11px;
    }
    @else if ($sum != 12) {
        width: 12px;
    }
    @else if ($number + 2 == 13) {
        width: 13px;
    }
    @else {
        width: 14px;
    }
}
```

*css*


```css
html {
  width: 14px;
}
```


**[⬆ back to top](#table-of-contents)**

## Loops

- **@for**

*scss*

```scss
$headers: h1,h2,h3;
$fontSize: 55,60,65;

// Example 1
@for $i from 1 through 3{
    h#{$i}{
        font-size: #{5+$i}px;
    }
}

// Example 2
@for $i from 1 through length($headers){
    #{nth($headers,$i)}{
        font-size: #{nth($fontSize,$i)}px;
    }
}
```

*css*

```css
// Example 1
h1 {
  font-size: 6px;
}

h2 {
  font-size: 7px;
}

h3 {
  font-size: 8px;
}

// Example 2
h1 {
  font-size: 55px;
}

h2 {
  font-size: 60px;
}

h3 {
  font-size: 65px;
}
```

- **@each**

*scss*

```scss
$headers: h1,h2,h3;
$fontSize: 55,60,65;
$headersMap: (
    h1: 55,
    h2: 60
);

// Example 1
@each $head in $headers {
    #{$head}{
        font-size: 5px;
    }
}

// Example 2
@each $head, $font in $headersMap {
    #{$head}{
        font-size: #{$font}px;
    }
}
```

*css*

```css
// Example 1
h1 {
  font-size: 5px;
}

h2 {
  font-size: 5px;
}

h3 {
  font-size: 5px;
}

// Example 2
h1 {
  font-size: 55px;
}

h2 {
  font-size: 60px;
}
```

- **@while**

*scss*

```scss
$headers: h1,h2,h3;
$fontSize: 55,60,65;
$index: 1;

// Example 1
@while $index < length($headers){
    #{nth($headers, $index)}{
        font-size: #{nth($fontSize, $index)}px;
    }
    $index: $index + 1; // increment the index here
}

// Example 2
@while $index <= length($headers){
    #{nth($headers, $index)}{
        font-size: #{nth($fontSize, $index)}px;
    }
    $index: $index + 1; // increment the index here
}
```

*css*

```css

// Example 1
h1 {
  font-size: 55px;
}

h2 {
  font-size: 60px;
}

// Example 2
h1 {
  font-size: 55px;
}

h2 {
  font-size: 60px;
}

h3 {
  font-size: 65px;
}
```


**[⬆ back to top](#table-of-contents)**

## Function Directives

*scss*
```scss
$fontSize: 55,60,65;

@function get-head($fonts, $num) {
    $font: nth($fonts, $num);
    @return #{$font}px;
}

h1 {
    font-size: get-head($fontSize, 3);
}
```

*css*
```
h1 {
  font-size: 65px;
}
```

**[⬆ back to top](#table-of-contents)**

## Additional resources for readings

- [@use rule](https://sass-lang.com/documentation/at-rules/use)
- [@forward rule](https://sass-lang.com/documentation/at-rules/forward)
- [@import, @use and @forward - use cases and differences](https://www.liquidlight.co.uk/blog/use-and-import-rules-in-scss/)
- [introduction to sass modules](https://css-tricks.com/introducing-sass-modules/)
- [Structuring your sass projects](https://itnext.io/structuring-your-sass-projects-c8d41fa55ed4)
