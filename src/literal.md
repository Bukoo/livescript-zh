# 字面量

## 数字

`.4`是是无效的写法，它必须要有0作为前缀，例如：`0.4`。

*LiveScript:*

```ls
42
17.34
0.4
```

*JavaScript:*

```js
42;
17.34;
0.4;
```

下划线和后面追加的字母是被忽略的。

*LiveScript:*

```ls
64_000km
```

*JavaScript:*

```js
64000；
```

布尔值，Void，Null

它们就像CoffeeScript一样

*LiveScript:*

```ls
true
false
on
off
yes
no
```

*JavaScript:*

```js
true;
false;
true;
false;
true;
false;
```

在JavaScript中，`undefined`是可以被重定义的，所以更明智的选择是使用`void`这个操作符，它永远返回undefined的值。
`void`如果在顶层(*不是作为一个表达式*)，那么它不会编译成任何东西(*仅仅作为一个占位符*)-它必须要作为一个值来编译。

*LiveScript:*

```ls
void
x = void

null
```

*JavaScript:*

```js
var x;
// void compiles to nothing here!
x = void 8;

null;
```

## 字符串

你可以使用双引号或者单引号

*LiveScript:*

```ls
'a string'
"a string"
```

*JavaScript:*

```js
'a string';
"a string";
```

字符串用一个前置的反斜杠来代替引号。
用了反斜杠后，字符串不能含有`, ; ] ) }`或者空白

*LiveScript:*

```ls
\word
func \word, \word;
(func \word)
[\word]
{prop:\word}
```

*JavaScript:*

```js
'word';
func('word', 'word');
func('word');
['word'];
({
  prop: 'word'
});
```

双引号字符串允许内插(*interpolation*)。而单引号字符串则不对内插语法进行转换。简单的变量的内插可以不同大括号`{}`

*LiveScript:*

```ls
"The answer is #{2 + 2}"
'As #{is}'

variable = "world"
"Hello #variable"
```

*JavaScript:*

```js
var variable;
"The answer is " + (2 + 2);
'As #{is}';

variable = "world";
"Hello " + variable;
```

在有内插操作的字符串前面加上`%`，返回一个数组，这允许你自由地连接它们。

*LiveScript:*

```ls
%"#x #y"
```

*JavaScript:*

```js
[x, " ", y];
```

多行的字符串(*同样，双引号的字符串才允许内插*)：

*LiveScript:*

```ls
multiline = 'string can be multiline
            and go on and on
            beginning whitespace is
            ignored'
heredoc = '''
            string can be multiline
            with newlines
            and go on and on
            beginning whitespace is
            ignored
'''
```

*JavaScript:*

```js
var multiline, heredoc;
multiline = 'string can be multiline and go on and on beginning whitespace is ignored';

heredoc = 'string can be multiline\nwith newlines\nand go on and on\nbeginning whitespace is\nignored';
```

## 注释

单行的注释以`#`开头，它们在编译后的结果中不会保留

*LiveScript:*

```ls
# single line comment
```

*JavaScript:*

```js

```

多行的注释则会保留

*LiveScript:*

```ls
/* multiline comments
   use this format and are preserved
   in the output unlike single line ones
*/
```

*JavaScript:*

```js
/* multiline comments
   use this format and are preserved
   in the output unlike single line ones
*/
```

## 对象

大括号是可缺省的。

*LiveScript:*

```ls
obj = {prop: 1, thing: 'moo'}



person =
  age:      23
  eye-color: 'green'
  height:   180cm

oneline = color: 'blue', heat: 4
```

*JavaScript:*

```js
var obj, person, oneline;
obj = {
  prop: 1,
  thing: 'moo'
};
person = {
  age: 23,
  eyeColor: 'green',
  height: 180
};
oneline = {
  color: 'blue',
  heat: 4
};
```

动态键

*LiveScript:*

```ls
obj =
  "#variable": 234
  (person.eye-color): false
```

*JavaScript:*

```js
var ref$, obj;
obj = (ref$ = {}, ref$[variable + ""] = 234, ref$[person.eyeColor] = false, ref$);
```

属性设置的简写，如果你想对象的属性名与赋值的变量名相同，这会很简单

*LiveScript:*

```ls
x = 1
y = 2
obj = {x, y}
```

*JavaScript:*

```js
var x, y, obj;
x = 1;
y = 2;
obj = {
  x: x,
  y: y
};
```

标志符的简写，设置布尔值会简单

*LiveScript:*

```ls
{+debug, -live}
```

*JavaScript:*

```js
({
  debug: true,
  live: false
});
```

This，不需要用点`.`来获取属性

*LiveScript:*

```ls
this
@
@location
```

*JavaScript:*

```js
this;
this;
this.location;
```

## 正则表达式

用`/`来分隔的正则表达式

*LiveScript:*

```ls
/moo/gi
```

*JavaScript:*

```js
/moo/gi
```

用`//`来分隔的正则表达式 - 多行，可注释，可用空格

*LiveScript:*

```ls
//
| [!=]==?             # equality
| @@                  # constructor
| <\[(?:[\s\S]*?\]>)? # words
//g
```

*JavaScript:*

```js
/|[!=]==?|@@|<\[(?:[\s\S]*?\]>)?/g;
```

## 列表

通常列表的字面量是用中括号来分隔的。

*LiveScript:*

```ls
[1, person.age, 'French Fries']
```

*JavaScript:*

```js
[1, person.age, 'French Fries'];
```

如果前面的项不是可调用对象，那么逗号是不必须的、

*LiveScript:*

```ls
[1 2 3 true void \word 'hello there']
```

*JavaScript:*

```js
[1, 2, 3, true, void 8, 'word', 'hello there'];
```

列表可以通过缩进来隐式构建，他们需要至少2项，如果只有一项，你可以通过添加`...`来使它工作起来。

*LiveScript:*

```ls
my-list =
  32 + 1
  person.height
  'beautiful'

one-item =
  1
  ...
```

*JavaScript:*

```js
var myList, oneItem;
myList = [32 + 1, person.height, 'beautiful'];



oneItem = [1];
```

当使用隐式列表的时候，你可以使用星号`*`来去掉隐式结构的二义性如：隐式对象和隐式列表。星号并不是标示了列表中的一个项，而仅仅是放置在隐式结构的旁边，使得它免于和其它列表中的项混淆。

*LiveScript:*

```ls
tree =
  * 1
    * 2
      3
    4
  * 5
    6
    * 7
      8
      * 9
        10
    11

obj-list =
  * name: 'tessa'
    age:  23
  * name: 'kendall'
    age:  19


obj =
  * name: 'tessa'
    age:  23

obj-one-list =
  * name: 'tessa'
    age:  23
  ...
```

*JavaScript:*

```js
var tree, objList, obj, objOneList;
tree = [[1, [2, 3], 4], [5, 6, [7, 8, [9, 10]], 11]];









objList = [
  {
    name: 'tessa',
    age: 23
  }, {
    name: 'kendall',
    age: 19
  }
];
obj = {
  name: 'tessa',
  age: 23
};
objOneList = [{
  name: 'tessa',
  age: 23
}];
```

词的列表

*LiveScript:*

```ls
<[ list of words ]>
```

*JavaScript:*

```js
['list', 'of', 'words'];
```

## 范围

`to`意思是直到并且包含那个数字，`til`意思是直到但不包含那个数字。
你可以添加一个可缺省的`by`来定义范围的步数，
若果你缺省第一个数字，那么它被赋值为0

带有数字和字符的字面量：

*LiveScript:*

```ls
[1 to 5]       #=> [1, 2, 3, 4, 5]
[1 til 5]      #=> [1, 2, 3, 4]
[1 to 10 by 2] #=> [1, 3, 7, 9]
[4 to 1]       #=> [4, 3, 2, 1]
[to 5]         #=> [0, 1, 2, 3, 4, 5]
[\A to \D]     #=> ['A', 'B', 'C', D']
```

*JavaScript:*

```js
var i$;
[1, 2, 3, 4, 5];
[1, 2, 3, 4];
[1, 3, 5, 7, 9];
[4, 3, 2, 1];
[0, 1, 2, 3, 4, 5];
["A", "B", "C", "D"];
```

带有表达式的范围，如果你想要它往下计数(*例如，从一个大数字到小数字*)你必须显式地设置`by -1`。

*LiveScript:*

```ls
x = 4
[1 to x]       #=> [1, 2, 3, 4]
[x to 0 by -1] #=> [4, 3, 2, 1, 0]
```

*JavaScript:*

```js
var x, i$;
x = 4;
for (i$ = 1; i$ <= x; ++i$) {
  i$;
}
for (i$ = x; i$ >= 0; --i$) {
  i$;
}
```

## 杂项

标签(*在嵌套循环中有用*)

*LiveScript:*

```ls
:label 4 + 2
```

*JavaScript:*

```js
label: {
  4 + 2;
}
```

`constructor`的简写

*LiveScript:*

```ls
@@
@@x
x@@y
```

*JavaScript:*

```js
constructor;
constructor.x;
x.constructor.y;
```

省略号

*LiveScript:*

```ls
...
```

*JavaScript:*

```js
throw Error('unimplemented');
```
