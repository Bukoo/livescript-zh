# 赋值

LiveScript中的基本赋值方式跟你了解的应该是差不多的，比如：`variable = value`，但不需要有变量声明。跟CoffeeScript不一样的是，如果你想修改外层作用域的变量，你需要使用`:=`操作符。

*LiveScript*
```ls

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
```ls
const x = 10
x = 0
```

会得到结果：`redeclaration of constant "x" on line 2`。

可是，如果是对象被声明为常量的话，你还是能修改他们的属性的。通过使用`-k`或者`--const`这两个命令行标识，你可以强制把所有变量编译成常量。

## 操作符

复合赋值运算符：

（`?`，`||`或者`&&`可以作为任何复合赋值操作的前缀）

```ls
x = 2    #=> 2
x += 2   #=> 4
x -= 1   #=> 3
x *= 3   #=> 9
x /= 3   #=> 3
x %= 3   #=> 0
x %%= 3  #=> 0
x <?= -1 #=> -1
x >?= 2  #=> 2
x **= 2  #=> 4
x ^= 2   #=> 16

x ?= 10
x        #=> 16

x ||= 5  #=> 16
x &&= 5  #=> 5

x &&+= 3 #=> 8
x ?*= 2
x        #=> 8

xs = [1 2]
xs ++= [3]
xs #=> [1 2 3]
```

```js
var x, ref$, xs;
x = 2;
x += 2;
x -= 1;
x *= 3;
x /= 3;
x %= 3;
x = ((x) % (ref$ = 3) + ref$) % ref$;
x <= (ref$ = -1) || (x = ref$);
x >= 2 || (x = 2);
x = Math.pow(x, 2);
x = Math.pow(x, 2);

x == null && (x = 10);
x;

x || (x = 5);
x && (x = 5);

x && (x += 3);
x == null && (x *= 2);
x;

xs = [1, 2];
xs = xs.concat([3]);
xs;
```

一元赋值运算符：

```ls
y = \45
+  = y   #=> 45   (转换成number类型)
!! = y   #=> true (转换成boolean类型)
-~-~ = y #=> 3    (intcasting bicrement(这个实在不会翻译))
```

```js
var y;
y = '45';
y = +y;
y = !!y;
y = -~-~y
```

Sock赋值（不知该如何翻译Sock）- 仅当右操作数存在的时候才进行赋值操作

```ls
age = 21
x? = age
x #=> 21


x? = years
x #=> 21
```

```js
var age, x;
age = 21;
if (age != null) {
  x = age;
}
x;
if (typeof years != 'undefined' && years !== null) {
  x = years;
}
x;
```

## 解构

解构是一种强大的从变量里面提取值的方式。你可以通过赋值给数据解构来提取变量的值，而不是赋值给一个简单的变量。例如：

```ls
[first, second] = [1, 2]
first  #=> 1
second #=> 2
```

```js
var ref$, first, second;
ref$ = [1, 2], first = ref$[0], second = ref$[1];
first;
second;
```

你也可以使用splat运算符（splat不会翻译）

```ls
[head, ...tail] = [1 to 5]
head #=> 1
tail #=> [2,3,4,5]

[first, ...middle, last] = [1 to 5]
first  #=> 1
middle #=> [2,3,4]
last   #=> 5
```

```js
var ref$, head, tail, first, i$, middle, last, slice$ = [].slice;
ref$ = [1, 2, 3, 4, 5], head = ref$[0], tail = slice$.call(ref$, 1);
head;
tail;
ref$ = [1, 2, 3, 4, 5], first = ref$[0], middle = 1 < (i$ = ref$.length - 1) ? slice$.call(ref$, 1, i$) : (i$ = 1, []), last = ref$[i$];
first;
middle;
last;
```

对象也可以进行解构赋值！

```ls
{name, age} = {weight: 110, name: 'emma', age: 20}
name #=> 'emma'
age  #=> 20
```

```js
var ref$, name, age;
ref$ = {
  weight: 110,
  name: 'emma',
  age: 20
}, name = ref$.name, age = ref$.age;
name;
age;
```

在解构的时候，你也可以使用`:label`来命名实体，以及任意地嵌套解构操作。

```ls
[[x, ...xs]:list1, [y, ...ys]:list2] = [[1,2,3],[4,5,6]]
x     #=> 1
xs    #=> [2,3]
list1 #=> [1,2,3]
y     #=> 4
ys    #=> [5,6]
list2 #=> [4,5,6]
```

```js
var ref$, list1, x, xs, list2, y, ys, slice$ = [].slice;
ref$ = [[1, 2, 3], [4, 5, 6]], list1 = ref$[0], x = list1[0], xs = slice$.call(list1, 1), list2 = ref$[1], y = list2[0], ys = slice$.call(list2, 1);
x;
xs;
list1;
y;
ys;
list2;
```

## 子结构

利用子结构可以很简单地设置数组和对象的属性。

```ls
mitch =
  age:    21
  height: 180cm
  pets:    [\dog, \goldfish]

phile = {}
phile{height, pets} = mitch
phile.height #=> 180
phile.pets   #=> ['dog', 'goldfish']
```

```js
var mitch, phile;
mitch = {
  age: 21,
  height: 180,
  pets: ['dog', 'goldfish']
};
phile = {};
phile.height = mitch.height, phile.pets = mitch.pets;
phile.height;
phile.pets;
```