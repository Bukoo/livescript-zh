# 介绍

[英文原文](http://livescript.net/#introduction)

就像很多现代的编程语言，块通过空格缩进来界定。语句通过取代了分号的换行来界定。(当然，你可以继续使用分号，如果你想在一行中写下多于一条语句)。
例如：(LiveScript在上方，编译后的JavaScript在下方)

*LiveScript:*

```ls
if 2 + 2 == 4
  do-something()
```

*JavaScript:*

```js
if (2 + 2 === 4) {
  doSomething();
}
```

> 你可以在LiveScript官网右方的编译器或REPL中尝试编译运行所有的例子

要使得代码更加简洁，你可以在调用函数的时候省略括号。

*LiveScript:*

```ls
add 2,3
```

*JavaScript:*

```js
add(2,3);
```

还有注释：

*LiveScript:*

```ls
# from here to the end of the line.
```

*JavaScript:*

```js
// from here to the end of the line.
```

> 使用Lisp的hacker们，你们可能会很高兴知道在变量和函数名中能够使用短横线(*dash*)。这种命名方式等价于驼峰命名法。例如：`my-value = 42` == `myValue = 42`。

还有一点是Livescript的后缀名是`.ls`。

## 函数的定义

在LiveScript中定义一个函数是很方便的。

*LiveScript:*

```ls

(x, y) -> x + y

-> # an empty function

times = (x, y) ->
  x * y
# multiple lines, and be assigned to
# a var like in JavaScript
```

*JavaScript:*

```js
var times;
(function(x, y){ return x + y; });

(function(){});

times = function(x, y){
  return x * y;
};
```

就像你看到的一样，函数定义是相当短的。你可能还会注意到我们省略了`return`。在LiveScript中，几乎所有东西都是表达式并且最后的一个值会被自动返回。但是，如果你想的话，你依然可以使用`return`来强制返回。你还可以在在箭头前面加上感叹号来表示不自动返回`no-ret = (x) !-> ...`。

## 赋值

基本的赋值和你所想的一样，`variable = value`，无需变量的声明。但是不同于CoffeeScript，你必须使用`:=`来修改上一级作用域的值。

*LiveScript:*

```ls
x = 10


do ->
  x = 5

x #=> 10

do ->
  x := 2

x #=> 2

```

*JavaScript:*

```js
var x;
x = 10;

(function(){
  var x;
  return x = 5;
})();
x;

(function(){
  return x = 2;
})();
x;
```

几乎所有东西都是表达式，意味着你可以做这样的事：

*LiveScript:*

```ls
x = if 2 + 2 == 4
    then 10
    else 0
x #=> 10
```

*JavaScript:*

```js
var x;
x = 2 + 2 === 4 ? 10 : 0;

x;
```

循环，分支，甚至是try/catch语句都是表达式。

如果你想仅仅声明一个变量，而不初始化它，你可以使用`var`。

*LiveScript:*

```ls
var x
```

*JavaScript:*

```js
var x;
```

在LiveScript中你还可以使用关键字`const`来声明常量。在编译时，它们会被检查是否有修改过。

如果尝试编译下面的代码:
```
const x = 10
x = 0
```
会出现这样的错误提示: `redeclaration of constant "x" on line 2`。
而对象(*object*)即使被声明为常量，它任然不会被冻结(frozen)，你依旧可以改变它的属性。通过在命令行加上编译参数`-k`或者`--const`，可以强制所有的变量成为常量。

## 注意

关于与CoffeeScript的差异，可以查看[CoffeeScript to LiveScript Conversion Guide](http://livescript.net/#coffee-to-ls)。

在LiveScript官网上，你可以通过双击样例代码，把它们放到右方的编译器中，或者你在以在编译器中随意修改代码来捣鼓一下，按下Run这个按钮就会运行那些代码。要注意的是LiveScript将所有编译好的JS代码封装在`(function(){...contents...}).call(this);`这样一个函数中。为了简洁，这个函数在我们给出的样例代码中是全部省略掉的。
