# 函数

使用LiveScript来定义函数是非常轻便的：

``` livescript
(x, y) -> x + y


-> # an empty function

times = (x, y) ->
    x * y
# multiple lines, and be assigned to
# a var like in JavaScript
```

``` javascript
var times;
(function(x, y) {
    return x + y;
});
(function() {});

times = function(x, y) {
    return x * y;
}
```

正如你所看到的那样，函数定义非常简短！同时你可能注意到，我们省略了 `return`。在LiveScript中，几乎所有东西都是表达式，并且函数中最后到达的表达式都会被自动返回。

你可以在函数箭头前面加一个`!`来阻止自动返回。

``` livescript
f = !-> 2
g = (x) !-> x + 2
```

``` javascript
var f, g;
f = function() {
  2;
}
g = function() {
  x + 2;
}
```

## 调用

调用函数的时候你可以省略圆括号，并且如果前一项是不可调用的，你还可以省略分隔参数的`,`，这就像数组里你可以省略`,`那样。

``` livescript
x = 4
Math.pow x, 3 #=> 64
Math.pow 2 3
```

``` javascript
var x;
x = 4;
Math.pow(x, 3);
Math.pow(2, 3);
```

如果你调用的函数没有参数，你可以在函数名后面加一个`!`，同时，当你链式地调用被`!`修饰的函数时，你不需要使用`.`。

``` livescript
f!
[1 2 3].reverse!slice 1 #=> [2,1]
```

``` javascript
f();
[1, 2, 3].reverse().slice(1);
```

`and`，`or`，`xor`，带空格的`.` 和`?.`都和隐式调用很类似——都能够在不适用圆括号的情况下链式使用。

``` livescript
$ \h1 .find \a .text! #=> LiveScirpt
```

``` javascript
$('h1').find('a').text();
```

你可以使用`do`来调用一个没有参数的函数：

``` livescript
do => 3 + 2 #=> 5
```

``` javascript
(function() {
  return 3 + 2;
}}();
```

如果你对一个有函数名的函数使用`do`，当`do`不是被用来作为一个表达式，该函数将会首先通过函数声明被声明。

``` livescript
i = 0
f 9 #=> 9
i   #=> 1
do function f x
  ++i
  x
i   #=> 2 
```

``` javascript
var i;
i = 0;
f(9);
i;
function f(x) {
  ++i;
  return x;
} f();
i;
```

你不能使用隐式对象作为参数来调用一个函数，如果你想要这么做的话，你可以使用`do`：
``` livescript
func do
  a : 1
  b : 2
```

``` javascript
func({
  a : 1,
  b : 2
});
```

`do`能够使你在不使用多余的括号的情况下做很多事情。

``` livescript
pow do
  1
  2
h 1 do
  a : 2
  b : 5
```

``` javascript
pow(1, 2);

h(1, {
  a : 2,
  b : 5
});
```

你也可以通过使用反引号\`来中缀式地调用函数：

``` livescript
add = (x, y) -> x + y
3 `add` 4 #=> 7
```

``` javascript
var add;
add = function(x, y) {
  return x + y;
}
add(3, 4);
```

使用省略提示符`...`意味着被调用函数使用当前函数的参数作为参数。这在调用`super`函数的时候非常有用。

``` livescript
f = (x, y) ->
  x + y

g = (a, b) ->
  f ...

g 3 4 #=> 7
```

``` javascript
var f, g;
f = function(x, y) {
  return x + y;
}
g = function(a, b) {
  return f.apply(this, arguments);
}
g(3, 4);
```