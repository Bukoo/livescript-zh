# 赋值

LiveScript中的基本赋值方式跟你了解的应该是差不多的，比如：`variable = value`，但不需要有变量声明。跟CoffeeScript不一样的是，如果你想修改外层作用域的变量，你需要使用`:=`操作符。

*LiveScript*
```livescript

x = 10

do ->
  x = 5

x #=> 10

do -> 
  x := 2

x #=> 2
```

*JavaScript*

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

几乎所有东西都是表达式，也就是说，你的代码可以写成这样子：

*LiveScript*
```livescript
x = if 2 + 2 == 4
    then 10
    else 0
x #=> 10
```

*JavaScript*
```js
var x;
x = 2 + 2 === 4 ? 10 : 0;

x;
```

循环、switch语句、甚至try/catch语句都是表达式。如果你想声明一个变量但不初始化它，你可以使用`var`关键字。

*LiveScript*
```livescript
var x
```

*JavaScript*
```js
var x;
```

你也可以在LiveScript中使用`const`来声明常量。常量检查会在LS编译成为JS时发生 - 跟JS引擎编译JS的时候一样。(参考：[How the V8 engine works?](http://thibaultlaurens.github.io/javascript/2013/04/29/how-the-v8-engine-works/), [理解JavaScript的编译过程与运行机制](http://blog.csdn.net/celte/article/details/39412683))

要是编译下面的语句的话：

*LiveScript*
```livescript
const x = 10
x = 0
```

会得到结果：`redeclaration of constant "x" on line 2`。

可是，如果是对象被声明为常量的话，你还是能修改他们的属性的。通过使用`-k`或者`--const`这两个命令行标识，你可以强制把所有变量编译成常量。

## 操作符

