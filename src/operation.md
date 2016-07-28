# 操作符

## 数字

标准的数学运算符：

*LiveScript:*

```ls
1 + 2 #=> 3
3 - 4 #=> -1
6 * 2 #=> 12
8 / 4 #=> 2
```

*JavaScript:*

```js
1 + 2;
3 - 4;
6 * 2;
8 / 4;
```

这里的求余操作符就像JavaScript一样，但是我们添加了一个更为恰当的求余操作符。

*LiveScript:*

```ls
-3 % 4  #=> -3
-3 %% 4 #=> 1
```

*JavaScript:*

```js
var ref$;
-3 % 4;
((-3) % (ref$ = 4) + ref$) % ref$;
```

幂指数操作符是右结合的，它拥有比单目运算符更高的优先级，`^`是`**`的简写。

*LiveScript:*

```ls
2 ** 4     #=> 16
3 ^ 4      #=> 81
-2 ^ 2 ^ 3 #=> -256
```

*JavaScript:*

```js
Math.pow(2, 4);
Math.pow(3, 4);
-Math.pow(2, Math.pow(2, 3));
```

递增和递减操作符：

*LiveScript:*

```ls
n = 0
n++ #=> 0
++n #=> 2
n-- #=> 2
--n #=> 0
x = n++ #=> 0
x #=> 0
n #=> 1
x = ++n #=> 2
x #=> 2
n #=> 2
```

*JavaScript:*

```js
var n, x;
n = 0;
n++;
++n;
n--;
--n;
x = n++;
x;
n;
x = ++n;
x;
n;
```

比特操作符和移位操作符：

*LiveScript:*

```ls
14 .&. 9   #=> 8
14 .|. 9   #=> 15
14 .^. 9   #=> 7
~9         #=> -10
9  .<<. 2  #=> 36
-9 .>>. 2  #=> -3
-9 .>>>. 2 #=> 1073741821
```

*JavaScript:*

```js
14 & 9;
14 | 9;
14 ^ 9;
~9;
9 << 2;
-9 >> 2;
-9 >>> 2;
```

数字的强制类型转换：

*LiveScript:*

```ls
+'4' #=>  4
-'3' #=> -3
```

*JavaScript:*

```js
+'4';
-'3';
```

## 比较

严格相等(*没有隐式类型转化*)：

*LiveScript:*

```ls
2 + 4 == 6      #=> true
\boom is 'boom' #=> true

\boom != null   #=> true
2 + 2 is not 4  #=> false
0 + 1 isnt 1    #=> false
```

*JavaScript:*

```js
2 + 4 === 6;
'boom' === 'boom';

'boom' !== null;
2 + 2 !== 4;
0 + 1 !== 1;
```

近似相等(带有隐式类型转换)：

*LiveScript:*

```ls
2 ~= '2'       #=> true
\1 !~= 1       #=> false
```

*JavaScript:*

```js
2 == '2';
'1' != 1;
```

大于小于：

*LiveScript:*

```ls
2 < 4           #=> true
9 > 7           #=> true
8 <= 8          #=> true
7 >= 8          #=> false
```

*JavaScript:*

```js
2 < 4;
9 > 7;
8 <= 8;
7 >= 8;
```

链式比较：

*LiveScript:*

```ls
1 < 2 < 4        #=> true
1 < 2 == 4/2 > 0 #=> true
```

*JavaScript:*

```js
var ref$;
1 < 2 && 2 < 4;
1 < 2 && 2 === (ref$ = 4 / 2) && ref$ > 0;
```

较大/较小 - 返回较大/较小的值

*LiveScript:*

```ls
4 >? 8     #=> 8
9 - 5 <? 6 #=> 4
```

*JavaScript:*

```js
var ref$;
4 > 8 ? 4 : 8;
(ref$ = 9 - 5) < 6 ? ref$ : 6;
```

当相等类符号(*`==`或`is`和它们的取非)的一个操作符是正则表达式字面量时，它会测试测试另外一个操作对象，相等操作符被编译成`exec`，所以你可以使用它的结果，但是不相等操作符会被编译成`test`。

*LiveScript:*

```ls
/^e(.*)/ is 'enter' #=> ["enter","nter"]
/^e(.*)/ == 'zx'    #=> null
/moo/ != 'loo'      #=> true
```

*JavaScript:*

```js
/^e(.*)/.exec('enter');
/^e(.*)/.exec('zx');
!/moo/.test('loo');
```

## 逻辑

基础：

*LiveScript:*

```ls
true and false #=> false
true && false  #=> false

true or false  #=> true
true || false  #=> true

not false      #=> true
!false         #=> true
```

*JavaScript:*

```js
true && false;
true && false;

true || false;
true || false;

!false;
!false;
```

一个在其它语言中不太常见的操作符 - 异或

*LiveScript:*

```ls
false xor true  #=> true
false xor false #=> false
1 xor 0         #=> 1
1 xor 1         #=> false
```

*JavaScript:*

```js
((true, false) || true) && !(false && true) && (false || true);
((false, false) || false) && !(false && false) && (false || false);
((0, 1) || 0) && !(1 && 0) && (1 || 0);
((1, 1) || 1) && !(1 && 1) && (1 || 1);
```

`and`，`or`和`xor`会隐式地分割开函数调用，但`||`和`&&`不会

*LiveScript:*

```ls
even 0 and 3 #=> 3
even 0 &&  3 #=> true
```

*JavaScript:*

```js
even(0) && 3;
even(0 && 3);
```

你可以调用逻辑操作符

*LiveScript:*

```ls
(f or g) 1
(f and g or h) 3 4
```

*JavaScript:*

```js
f(1) || g(1);
f(3, 4) && g(3, 4) || h(3, 4);
```

## In/Of

使用`in`来检查一个元素是否在列表中；使用`of`来检查一个键是否在一个对象中

*LiveScript:*

```ls

list = [7 8 9]
2 in [1 2 3 4 5]             #=> true
3 in list                    #=> false
\id of id: 23, name: \rogers #=> true
```

*JavaScript:*

```js
var list;
list = [7, 8, 9];
2 == 1 || 2 == 2 || 2 == 3 || 2 == 4 || 2 == 5;
in$(3, list);
'id' in {
  id: 23,
  name: 'rogers'
};
function in$(x, arr){
  var i = 0, l = arr.length >>> 0;
  while (i < l) if (x === arr[i++]) return true;
  return false;
}
```

## 管道

可以使用管道来替代一系列的嵌套的函数调用，`x |> f`和`f <| x`等价于`f(x)`。

*LiveScript:*

```ls
x = [1 2 3] |> reverse |> head #=> 3


y = reverse <| [1 2 3] #=> [3,2,1]
```

*JavaScript:*

```js
var x, y;
x = head(
reverse(
[1, 2, 3]));
y = reverse([1, 2, 3]);
```

你可以用换行来让代码变得更好看

*LiveScript:*

```ls
4
|> (+ 1)
|> even
#=> false
```

*JavaScript:*

```js
even(
(function(it){
  return it + 1;
})(
4));
```

## 函数

通过复合操作符，你可以避免使用多个函数串联来达到复合。LiveScript有两个用于复合的操作符，向前复合`>>`、向后复合`<<`。
`(f << g) x`等价于`f(g(x))`，而`(f >> g) x`等价于`g(f(x))`。例如：

*LiveScript:*

```ls
odd     = (not) << even
odd 3   #=> true
```

*JavaScript:*

```js
var odd;
odd = function(){
  return not$(even.apply(this, arguments));
};
odd(3);
function not$(x){ return !x; }
```

下面例子更清晰地区分这了两个操作符

*LiveScript:*

```ls
add-two-times-two = (+ 2) >> (* 2)
times-two-add-two = (+ 2) << (* 2)

add-two-times-two 3 #=> (3+2)*2 => 10
times-two-add-two 3 #=> (3*2)+2 => 8
```

*JavaScript:*

```js
var addTwoTimesTwo, timesTwoAddTwo;
addTwoTimesTwo = function(){
  return (function(it){
    return it * 2;
  })((function(it){
    return it + 2;
  }).apply(this, arguments));
};
timesTwoAddTwo = function(){
  return (function(it){
    return it + 2;
  })((function(it){
    return it * 2;
  }).apply(this, arguments));
};
addTwoTimesTwo(3);
timesTwoAddTwo(3);
```
你可以用空格和点来代替`<<`，例如：`f . g` 就像Haskell一样

## 列表

你可以连接两个列表

*LiveScript:*

```ls
<[ one two three ]> ++ [\four]
#=> ['one','two','three','four']
```

*JavaScript:*

```js
['one', 'two', 'three'].concat(['four']);
```

要注意的是，连接操作符的左右两边要么都有空格`xs ++ ys`，要么都没空格`xs++ys`。如果只有一边有空格，那么它会被看成是自增操作符。

当乘以符号`*`的左边是列表字面量时，可以当做是列表重复的简写

*LiveScript:*

```ls
[\ha] * 3 #=> ['ha','ha','ha']
```

*JavaScript:*

```js
['ha', 'ha', 'ha'];
```

当乘以符号`*`的右边是字符串字面量时，可以当做是join方法的简写

*LiveScript:*

```ls
<[ one two three ]> * \|      #=> 'one|two|three'
```

*JavaScript:*

```js
['one', 'two', 'three'].join('|');
```

单目操作符会展开，如果它的操作数是列表字面量时，单目操作符会应用到每一项

*LiveScript:*

```ls
r = +[4 5 6]        #=> [+4, +5, +6]
t = typeof! [\b 5 {}] #=> ["String", "Number", "Object"]
c = ~[4, 5]         #=> [-5, -6]
++player<[strength hp]>
# 对后面的操作符同样有效 -, --, typeof, ! and delete!
i = new [some, classes]
c = ^^[copy, these, {}]
delete list[1, 2, 3]
do [a, b, c]

```

*JavaScript:*

```js
var r, t, c, i, toString$ = {}.toString;
r = [+4, +5, +6];
t = [toString$.call('b').slice(8, -1), toString$.call(5).slice(8, -1), toString$.call({}).slice(8, -1)];
c = [~4, ~5];
++player['strength'], ++player['hp'];
i = [new some, new classes];
c = [clone$(copy), clone$(these), clone$({})];
delete list[1], delete list[2], delete list[3];
a(), b(), c();
function clone$(it){
  function fun(){} fun.prototype = it;
  return new fun;
}
```

## 字符串

*LiveScript:*

字符串连接

```ls
'hello' + ' ' + 'world' #=> 'hello world'
string = 'say '         #=> 'say '
string += \yeah         #=> 'say yeah'
```

*JavaScript:*

```js
var string;
'hello' + ' ' + 'world';
string = 'say ';
string += 'yeah';
```

当乘以符号`*`的左边是字符串字面量时，可以当做是字符串重复的简写

*LiveScript:*

```ls
'X' * 3      #=> 'XXX'
```

*JavaScript:*

```js
'XXX';
```

字符串的 减操作/除以操作 当右边的操作数是字符串或者是正则表达式字面量时，减操作表示`replace`，除以操作表示`split`

*LiveScript:*

```ls
'say yeah' - /h/ #=> 'say yea'
'say yeah' / \y  #=> ['sa',' ','eah']
```

*JavaScript:*

```js
'say yeah'.replace(/h/, '');
'say yeah'.split('y');
```

## 存在性

问号操作符`?`可以用于很多查询存在性的场景

*LiveScript:*

```ls
bigfoot ? 'grizzly bear'     #=> 'grizzly bear'
string = \boom if window?    #=> 'boom'
document?.host               #=> 'gkz.github.com'
```

*JavaScript:*

```js
var string;
(typeof bigfoot == 'undefined' || bigfoot === null) && 'grizzly bear';
if (typeof window != 'undefined' && window !== null) {
  string = 'boom';
}
if (typeof document != 'undefined' && document !== null) {
  document.host;
}
```

## 对象

Instanceof - 当右边的操作数是列表字面量的时候回展开

*LiveScript:*

```ls
new Date() instanceof Date           #=> true
new Date() instanceof [Date, Object] #=> true
```

*JavaScript:*

```js
var ref$;
new Date() instanceof Date;
(ref$ = new Date()) instanceof Date || ref$ instanceof Object;
```

Typeof - 增加一个感叹号会是有用的选择

*LiveScript:*

```ls
typeof /^/  #=> object
typeof! /^/ #=> RegExp
```

*JavaScript:*

```js
var toString$ = {}.toString;
typeof /^/;
toString$.call(/^/).slice(8, -1);
```

`delete`操作符会返回被删除的对象

*LiveScript:*

```ls
obj = {one: 1, two: 2}
r = delete obj.one
r #=> 1
```

*JavaScript:*

```js
var obj, r, ref$;
obj = {
  one: 1,
  two: 2
};
r = (ref$ = obj.one, delete obj.one, ref$);
r;
```

`delete!`就像`delete`在JavaScript一样，返回false仅当属性不存在和不能被delete，否则返回true

*LiveScript:*

```ls
obj = {one: 1, two: 2}
delete! obj.one #=> true
delete! Math.PI #=> false
```

*JavaScript:*

```js
var obj;
obj = {
  one: 1,
  two: 2
};
delete obj.one;
delete Math.PI;
```

属性拷贝 - 拷贝会遍历右边操作数的属性到左边的操作数中。`<<<`是作用于独有的属性(*own property*)，`<<<<`作用于所有属性。`import`和`import all`是上面操作符的别名函数，除了有一点不同：如果省略了左边的操作数，那么`this`将会是默认值

*LiveScript:*

```ls
obj = {one: 1, two: 2}
obj <<< three: 3 #=> {one: 1, two: 2, three: 3}
{go: true} <<<< window
import obj
```

*JavaScript:*

```js
var obj;
obj = {
  one: 1,
  two: 2
};
obj.three = 3;
importAll$({
  go: true
}, window);
import$(this, obj);
function importAll$(obj, src){
  for (var key in src) obj[key] = src[key];
  return obj;
}
function import$(obj, src){
  var own = {}.hasOwnProperty;
  for (var key in src) if (own.call(src, key)) obj[key] = src[key];
  return obj;
}
```

克隆 - 创造操作数的原型链克隆。它不会创造一个对象深度克隆。相反，操作数将会是返回对象的原型。要记住在克隆`^^`对象转化成JSON的时候，原型会被忽略。

*LiveScript:*

```ls
obj = {one: 1}
obj2 = ^^obj
obj2.two = 2
obj2 #=> {one: 1, two: 2}
# 上面包括了它原型的属性
# 字符串化为JSON `{two: 2}`
obj  #=> {one: 1}

```

*JavaScript:*

```js
var obj, obj2;
obj = {one: 1};
obj2 = clone$(obj);
obj2.two = 2;
obj2;


obj
function clone$(it){
  function fun(){} fun.prototype = it;
  return new fun;
}
```

插入(*infix*)操作符`with`操作符(*也被称作cloneport*)是克隆和属性拷贝的组合，等价于`^^obj <<< obj2`。要记住在克隆`^^`对象转化成JSON的时候，原型会被忽略。

*LiveScript:*

```ls
girl = {name: \hanna, age: 22}
guy  = girl with name: \john
guy  #=> {name: 'john',  age: 22}
# 返回的结果guy包含的对象原型girl
# guy的JSON: {name: 'john'}
girl #=> {name: 'hanna', age: 22}
```

*JavaScript:*

```js
var obj, obj2;
obj = {one: 1};
obj2 = clone$(obj);
obj2.two = 2;
obj2;


obj
function clone$(it){
  function fun(){} fun.prototype = it;
  return new fun;
}
```

## 部分应用，操作符作为函数

你可以部分应用操作符和把他们用作一个函数

*LiveScript:*

```ls
(+ 2) 4         #=> 6
(*) 4 3         #=> 12

(not) true      #=> false
(in [1 to 3]) 2 #=> true

```

*JavaScript:*

```js
(function(it){
  return it + 2;
})(4);
curry$(function(x$, y$){
  return x$ * y$;
})(4, 3);
not$(true);
(function(it){
  return it == 1 || it == 2 || it == 3;
})(2);
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
function not$(x){ return !x; }
```

## 导出

通过操作符`export`替代`exports`，你可以有更加清晰的方式定义模块

*LiveScript:*

```ls
export func = ->

export value

export value-a, value-b, value-c



export
  a: 1
  b: -> 123



export class MyClass

```

*JavaScript:*

```js
var func, ref$, MyClass, out$ = typeof exports != 'undefined' && exports || this;

out$.func = func = function(){};

out$.value = value;

out$.valueA = valueA;
out$.valueB = valueB;
out$.valueC = valueC;

ref$ = out$;
ref$.a = 1;
ref$.b = function(){
  return 123;
};

out$.MyClass = MyClass = (function(){
  MyClass.displayName = 'MyClass';
  var prototype = MyClass.prototype, constructor = MyClass;
  function MyClass(){}
  return MyClass;
}());
```

## 导入

导入一系列的模块会通常会造成不优雅的代码，可以使用`require!`来避免，操作数可以为字符串、数组、对象字面量。
如需导入带有横杆`-`的模块，则必须使用字符串字面量。
你可以通过对象字面量来重命名导入的模块。
你可以解构获得的对象。

*LiveScript:*

```ls
require! lib
require! 'lib1'

require! prelude-ls # no
require! 'prelude-ls'

require! [fs, path]

require! <[ fs path ]>


require! jQuery: $

require! {
  fs
  path
  lib: foo
}
```

*JavaScript:*

```js
var lib, lib1, preludeLs, fs, path, $, foo;
lib = require('lib');
lib1 = require('lib1');

preludeLs = require('preludeLs');
preludeLs = require('prelude-ls');

fs = require('fs');
path = require('path');
fs = require('fs');
path = require('path');

$ = require('jQuery');


fs = require('fs');
path = require('path');
foo = require('lib');
```

你可以通过解构来获取模块的部分

*LiveScript:*

```ls
require! {
    fs: filesystem
    'prelude-ls': {map, id}
    path: {join, resolve}:p
}
```

*JavaScript:*

```js
var filesystem, ref$, map, id, p, join, resolve;
filesystem = require('fs');
ref$ = require('prelude-ls'), map = ref$.map, id = ref$.id;
p = require('path'), join = p.join, resolve = p.resolve;
```

文件名会自动解析出

*LiveScript:*

```ls
require! 'lib.js'
require! './dir/lib1.js'
```

*JavaScript:*

```js
var lib, lib1;
lib = require('lib.js');
lib1 = require('./dir/lib1.js');
```
