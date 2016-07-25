# 函数

使用LiveScript来定义函数是非常轻便的：

*LiveScirpt:*

``` livescript
(x, y) -> x + y


-> # 一个空函数

times = (x, y) ->
    x * y
# 多行函数，并且像JavaScript一样，
# 函数被赋值给一个var声明的变量
```

*JavaScript:*

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

*LiveScirpt:*

``` livescript
f = !-> 2
g = (x) !-> x + 2
```

*JavaScript:*

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

*LiveScirpt:*

``` livescript
x = 4
Math.pow x, 3 #=> 64
Math.pow 2 3
```

*JavaScript:*

``` javascript
var x;
x = 4;
Math.pow(x, 3);
Math.pow(2, 3);
```

如果你调用的函数没有参数，你可以在函数名后面加一个`!`，同时，当你链式地调用被`!`修饰的函数时，你不需要使用`.`。

*LiveScirpt:*

``` livescript
f!
[1 2 3].reverse!slice 1 #=> [2,1]
```

*JavaScript:*

``` javascript
f();
[1, 2, 3].reverse().slice(1);
```

`and`，`or`，`xor`，带空格的`.` 和`?.`都和隐式调用很类似——都能够在不适用圆括号的情况下链式使用。

*LiveScirpt:*

``` livescript
$ \h1 .find \a .text! #=> LiveScirpt
```

*JavaScript*:

``` javascript
$('h1').find('a').text();
```

你可以使用`do`来调用一个没有参数的函数：

*LiveScirpt:*

``` livescript
do => 3 + 2 #=> 5
```

*JavaScript:*

``` javascript
(function() {
  return 3 + 2;
})();
```

如果你对一个有函数名的函数使用`do`，当`do`不是被用来作为一个表达式时，该函数将会首先通过函数声明被声明。

*LiveScirpt:*

``` livescript
i = 0
f 9 #=> 9
i   #=> 1
do function f x
  ++i
  x
i   #=> 2 
```

*JavaScript:*

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

*LiveScirpt:*

``` livescript
func do
  a : 1
  b : 2
```

*JavaScript:*

``` javascript
func({
  a : 1,
  b : 2
});
```

`do`能够使你在不使用多余的括号的情况下做很多事情。

*LiveScirpt:*

``` livescript
pow do
  1
  2
h 1 do
  a : 2
  b : 5
```

*JavaScript:*

``` javascript
pow(1, 2);

h(1, {
  a : 2,
  b : 5
});
```

你也可以通过使用反引号\`来中缀式地调用函数：

*LiveScirpt:*

``` livescript
add = (x, y) -> x + y
3 `add` 4 #=> 7
```

*JavaScript:*

``` javascript
var add;
add = function(x, y) {
  return x + y;
}
add(3, 4);
```

使用省略提示符`...`意味着被调用函数使用当前函数的参数作为参数。这在调用`super`函数的时候非常有用。

*LiveScirpt:*

``` livescript
f = (x, y) ->
  x + y

g = (a, b) ->
  f ...

g 3 4 #=> 7
```

*JavaScript:*

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

## 参数

扩展的参数：

*LiveScirpt:*

``` livescript
set-person-params = (
  person # 设置参数的目标对象
  person.age
  person.height
) -> person

person = set-person-params {}, 21, 180cm
#=> {age: 21, height: 100}
```

*JavaScript:*

``` javascript
var setPersonParams, person;
setPersonParams = function(person, age, height) {
  person.age = age;
  person.height = height;
  return person;
};
person = setPersonParams({}, 21, 180);
```

和`this`结合在一起的时候，这一点非常有用。

*LiveScirpt:*

``` livescript
set-text = (@text) -> this
```

*JavaScript:*

``` javascript
var setText;
setText = function(text) {
  this.text = text;
  return this;
}
```

你可以设置默认参数：

*LiveScirpt:*

``` livescript
add = (x = 4, y = 3) -> x + y
add 1 2 #=> 3
add 1   #=> 4
add!    #=> 7
```

*JavaScript:*

``` javascript
var add;
add = function(x, y) {
  x == null && (x = 4);
  y == null && (y = 3);
  return x + y;
};
add(1, 2);
```

事实上，你可以使用任何逻辑操作符（在参数中，`x = 2` 是 `x ? 2` 的语法糖）：

*LiveScirpt:*

``` livescript
add = (x && 4, y || 3) -> x + y
add 1 2 #=> 6
add 2 0 #=> 7
```

*JavaScript:*

``` javascript
var add;
add = function(x, y) {
  x && (x = 4);
  y || (y = 3); 
};
add(1, 2);
add(2, 0);
```

你也可以对参数进行解构：

*LiveScirpt:*

``` livescript
set-cords = ({x, y}) -> "#x,#y"
set-cords y: 2, x: 3 #=> '3,2'
```

*JavaScript:*

``` javascript
var setCords;
setCords = function(arg$) {
  var x, y;
  x = arg$.x, y = arg$.y;
  return x + ',' + y;
};
setCords({
  y: 2,
  x: 3
});
```

你甚至可以为被解构的参数设置默认值（或者使用任何逻辑语句），这和Python语言的关键字参数的工作方式是一样的。

*LiveScirpt:*

``` livescript
set-cords = ({x = 1, y = 3} = {}) -> "#x,#y"
set-cords y: 2, x: 3 #=> '3,2'
set-cords x: 2       #=> '2,3'
set-cords y: 7       #=> '1,7'
set-cords!           #=> '1,3'
```

*JavaScript:*

``` javascript
var setCords;
setCords = function(arg$) {
  var ref$, ref1$, x, y;
  ref$ = arg$ != null ? arg$ : {}, x = (ref1$ = ref$.x) != null ? ref1$ : 1, y = (ref1$ = ref$.y) != null ? ref1$ : 3;
  return x + "," + y;
};
setCords({
  y: 2,
  x: 3
});
setCords({
  x: 2
});
setCords({
  y: 7
});
setCords();
```

你也可以在参数中使用省略提示符：

*LiveScirpt:*

``` livescript
f = (x, ...ys) -> x + ys.1
f 1 2 3 4 # 4
```

*JavaScript:*

``` javascript
var f, slice$ = [].slice;
f = function(x) {
  var ys;
  ys = slice$.call(arguments, 1);
  return x + ys[1];
}
f(1, 2, 3, 4);
```

你也可以在你的参数中使用一元操作符。使用`+` 和 `!!` 操作符能够分别将你的参数转换为数字和布尔值，或者使用克隆操作符`^^`来确保在函数中对对象的改变不会影响到原来的对象。你仍然可以使用扩展的参数，例如. `(!!x.x) ->`。

*LiveScirpt:*

``` livescript
f = (!!x) -> x
f 'truthy string' #=> string

g = (+x) -> x
g '' #=> 0

obj = {prop: 1}
h = (^^x) ->
  x.prop = 99
  x
h obj
obj.prop #=> 1
```

*JavaScript:*

``` javascript
var f, g, obj, h;
f = function(x) {
  x = !!x;
  return x;
};

g = function(x) {
  x = +x;
  return x;
}

obj = { prop: 1};
h = function() {
  x = clone$(x);
  x.prop = 99;
  return x;
};
h(obj);
obj.prop;

funcion clone$(it) {
  function fun() {} fun.prototype = it;
  return new fun;
}
```

## 柯里化

柯里化函数的功能非常强大。其本质是，当被调用时提供的参数少于定义时的参数，柯里化函数返回一个部分应用的函数。也就是说，柯里化函数返回参数为未提供部分的函数，并且你提供的参数的值已经被绑定。柯里化函数使用长箭头来定义。或许使用一个例子能够更能够帮助你理解。

*LiveScirpt:*

``` livescript
times = (x, y) --> x * y
times 2, 3       #=> 6 （和期望中一样正常工作）
double = times 2
double 5         # 10
```

*JavaScript:*

``` javascript
var times, double;
times = curry$(function(x, y) {
    return x * y;
});
times(2, 3);
double = times(2);
double(5);
function curry$(f, bound) {
  var context;
  _curry = function(args) {
    return f.length > 1 ? function() {
      var prams = args ? args.concat() : [];
      contex = bound ? context || this : this;
      return params.push.apply(params, arguments) < f.length && arguments.length ? _curry.call(context. params) : f.apply(context, params);
    } : f;
  };
  return _curry();
}
```

你可以使用长波浪箭头： `~~>`，来定义绑定柯里化函数。

如果你调用一个没有参数的柯里化函数，它也能够允许你使用默认参数。

*LiveScirpt:*

``` livescript
f = (x = 5, y = 10) --> x + y
f! #=> 15
g = f 20
g 7 #=> 27
g!  #=> 30
```

*JavaScript:*

``` javascript
var f, g;
f = curry$(function(x, y) {
  x == null && (x = 5);
  y == null && (y = 10);
  return x + y;
});
f();
g = f(20);
g(7);
g();
function curry$(f, bound) {
  var context;
  _curry = function(args) {
    return f.length > 1 ? function() {
      var prams = args ? args.concat() : [];
      contex = bound ? context || this : this;
      return params.push.apply(params, arguments) < f.length && arguments.length ? _curry.call(context. params) : f.apply(context, params);
    } : f;
  };
  return _curry();
}
```

## 显示命名函数

你能够创建显示命名函数，显示命名函数的定义将会被提升到作用域的顶部——这在将通用函数的定义放在文件结尾而不是顶部的时候非常有用。显示命名的函数是常量，它们不能够被重复定义。

*LiveScirpt:*

``` livescript
util!   #=> '函数在声明前就能够被调用'
until2! #=> 2

function util
  `avaulable above declaration`
function util2 then 2
```

*JavaScript:*

``` javascript
util();
util2();
function util() {
  return 'available above declaration';
}
function util2() {
  return 2;
}
```

你可以在函数定义前面加一个波浪号`~`，使该函数成为一个绑定函数：

*LiveScirpt:*

``` livescript
~function add x, y
  @result = x + y
```

*JavaScript:*

``` javascript
var this$ = this;
function add(x, y) {
  return this$.result = x + y;
}
```

你可以在函数定义前面加一个感叹号`!`来阻止函数返回。

*LiveScirpt:*

``` livescript
util! #=> nothing
!function util x the x
```

*JavaScript:*

``` javascript
util();
function util(x) {
  x;
}
```

当然，如果你想要的话，你也可以结合使用`~` 和 `!`来创建一个绑定的无返回函数。

## 绑定函数

绑定函数可以使用波浪箭头`~>`来定义。使用长波浪箭头能够定义柯里化的绑定函数。在显示命名的函数前面加`~`能够使该函数被绑定。

绑定函数的绑定是词法作用域上的绑定而不是正常的动态绑定。这意味着，`this`的值和绑定函数被调用的上下文无关，`this`的值总是绑定函数被定义时的上下文中`this`的值。

*LiveScirpt:*

``` livescript
obj = new
  @x      = 10
  @normal = -> @x
  @bound  = ~> @x

obj2 = x: 5
obj2.normal = obj.normal
obj2.bound  = obj.bound

obj2.normar! #=> 5
obj2.bound!  #=> 10
```

*JavaScript:*

``` javascript
var obj, obj2;
obj = new function() {
  var this$ = this;
  this.x = 10;
  this.normal = function() {
    return this.x;
  };
  this.bound = function() {
    return this$.x;
  };
};
obj2 = {
  x: 5
};
obj2.normal = obj.normal;
obj2.bound = obj.bound;
obj2.normal();
obj2.bound();
```

更多绑定函数在类中的使用可以查看[OOP](http://livescript.net/#oop)章节。

## `Let`, `New`

`let`是`(function(a){...}.call(this, b))`的简写。

*LiveScirpt:*

``` livescript
let $ = jQuery
  $.isArray [] #=> true
```

*JavaScript:*

``` javascript
(function($) {
    $.isArray([]);
}.call(this, jQuery));
```

你可以使用`let`定义`this`（又名`@`）。

*LiveScirpt:*

``` livescript
x = let @ = a: 1, b: 2
  @b ^ 3
x #=> 8
```

*JavaScript:*

``` javascript
var x;
x = (function() {
  return Math.pow(this.b, 3);
}.call({
  a: 1,
  b: 2
}));
x;
```

使用new绑定上下文：

*LiveScirpt:*

``` livescript
dog = new
  @name = \spot
  @mutt = true
#=> {name: 'spot', mutt: true}
```

*JavaScript:*

``` javascript
var dog;
dog = new function() {]
  this.name = 'spot';
  this.mutt = true;
};
```

## 访问、调用函数的简写

对于像`map`和`filter`这样的高阶函数，简写是非常有用的。`.porp`是`(it) -> it.prop`的简写。

*LiveScirpt:*

``` livescript
map (.length), <[ hello there you ]>
#=> [5,5,3]

filter (.length < 4), <[ hello there you]>
#=> ['you']
```

*JavaScript:*

``` javascript
map(function(it) {
  return it.length;
}, ['hello', 'there', 'you']);
filter(function(it) {
  return it.length < 4;
}, ['hello', 'there', 'you']);
```

你也可以使用简写来调用对象的方法：

*LiveScirpt:*

``` livescript
map (.join `|`) [[1 2 3], [7 8 9]]
#=> ['1|2|3', '7|8|9']
```

*JavaScript:*

``` javascript
map(function(it) {
  return it.join('|');
}, [[1, 2, 3], [4, 5, 6]]);
```

`(obj.)`是`(it) -> obj[it]`的简写。

*LiveScirpt:*

``` livescript
obj = one: 1, two: 2, three: 3
map (obj.), <[ one three ]>
#=> [1, 3]
```

*JavaScript:*

``` javascript
var obj;
obj = {
  one: 1,
  two: 2,
  three: 3
};
map(function(it) {
  return obj[it];
}, ['one', 'three']);
```

## Backcalls

Backcalls是非常有用的。Backcalls允许你能够扁平化你的回调函数。Backcalls通过向左指的箭头来定义。定义backcalls的所有语法和定义绑定函数（`<~`），柯里化函数（`<--`，`<~~`），不返回内容的函数（`<-!`）的箭头是一样的——除了箭头所指的方向是相反的。

*LiveScirpt:*

``` livescript
<- $
alert \boom
```

*JavaScript:*

``` javascript
$(function() {
  return alert('boom');
});
```

Backcalls能够使用参数，并且你可以使用`_`占位符来指定你想要让它出现的地方。

*LiveScirpt:*

``` livescript
x <- map _, [1 to 3]
x * 2
#=> [2, 4, 6]
```

*JavaScript:*

``` javascript
map(function(x) {
  return x * 2;
}, [1, 2, 3]);
```

如果你想要你的backcalls后面继续编写代码，你可以使用`do`语句来区分它们。

*LiveScirpt:*

``` livescript
do
  data -> $.get 'ajaxtest'
  $ '.result' .html data
  processed <-! $.get 'ajaxprocess', data
  $ '.result' .append processed

alert 'hi'
```

*JavaScript:*

``` javascript
$.get('ajaxtest', function(data) {
  $('.result').html(data);
  $.get('ajaxprocess', data, function(processed) {
    $('.result').append(processed);
  });
});
alert('hi');
```

## 部分应用

你可以使用下划线`_`作为占位符来定义部分应用函数。有时候，你所调用的函数不是柯里化的，或者如果函数的参数并不是合适的顺序。在这些情况下部分应用函数是非常有用的。

*LiveScirpt:*

``` livescript
filter-nums = filter _, [1 to 5]
filter-nums even  #=> [2,4]
filter-nums odd   #=> [1,3,5]
filter-nums (< 3) #=> [1, 2]
```

*JavaScript:*

``` javascript
var filterNums, slice$ = [].slice;
filterNums = partialize$.apply(this, [filter, [void 8, [1, 2, 3, 4, 5]], [0]]);
filterNums(even);
filterNums(odd);
filterNums(function(it) {
  return it < 3;
});
function partialize$(f, args, where) {
  var context = this;
  return function() {
    var params = slice$.call(arguments), i,  len = params.length, wlen = where.length, ta = args ? args.contact() : []. tw = where ? where.concat(): [];
    for(i = 0; i < len; ++i) { ta[tw[0]] = params[i]; tw.shift(); }
      return len < wlen && len ? partialize$.apply(context, [f, ta, tw]) : f.apply(context, ta);
  };
}
```

如果你调用一个部分应用函数而不传递参数，它会像允许你使用默认参数那样执行，而不是返回自身。

如果你使用的函数没有良好的参数顺序并且不是柯里化的（例如 underscore.js），部分应用函数在管道式调用上将非常有帮助。

*LiveScirpt:*

``` livescript
[1 2 3]
|> _.map _, (* 2)
|> _reduce _, (+), 0
#=> 12
```

*JavaScript:*

``` javascript
_.reduce(_.map([1, 2, 3], (function(it) {
  return it * 2;
})), curry$(function(x$, y$) {
  return x$ + y$;
}), 0);
function curry$(f, bound){
  var context,
  _curry = function(args) {
    return f.length > 1 ? function(){
      var params = args ? args.concat() : [];
      context = bound ? context || this : this;
      return params.push.apply(params, arguments) <
          f.length && arguments.length ?
        _curry.call(context, params) : f.apply(context, params);
    } : f;
  };
  return _curry();
}
```

## Arguments

如果你只有一个参数，你可以不用定义一个参数而是直接使用`it`来访问。

*LiveScirpt:*

``` livescript
f = -> it + 2
f 3 #=> 5
```

*JavaScript:*

``` javascript
var f;
f = function(it) {
  return it + 2;
}
f(3);
```

你可以使用简写`&`来访问`arguments`对象。第一个参数用`&0`表示，第二个参数用`&1`表示，以此类推。单独使用`&`表示`arguments`整个对象。

*LiveScirpt:*

``` livescript
add-three-numbers = -> &0 + &1 + &2
add-three-numbers 1 2 3 #=> 6
```

*JavaScript:*

``` javascript
var addThreeNumbers;
addThreeNumbers = function() {
  return arguments[0] + arguments[1] + arguments[2];
};
addThreeNumbers(1, 2, 3);
```

注意当`add-three-numbers`函数声明的参数数量为0时，柯里化将无法工作。

## 更多
