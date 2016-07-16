# 循环结构
`for`循环有三种基本形式：一种遍历一定范围的数，另一种遍历一个列表中的项，还有一种遍历一个对象中的关键字和值。

首先，我们来看看遍历一定范围的数的`for`循环。它有着这样的结构：`for (let) (VAR) (from NUM) (to|til NUM) (by NUM) (when COND)`——几乎每一项都是可选的。

`let`会把循环体包裹在一个立即调用的函数表达式内，当你在循环里创建函数，并且希望当函数调用时，函数中的循环变量是当前——而非最终——的值时，它会非常有用。它还意味着你的循环内部的变量不会暴露给包围循环的作用域，这也很有用。

`from`表示计数的起点，缺省为`0`。

`to`和`til`表示计数的终点，前者包括端点，而后者不包括。

`by`表示计数的步长，缺省为`1`。

`when`（*又名`case`,`|`*）用于附加选择条件，只有当`COND`为真时才会进入循环体。

如果循环被当做表达式使用，它的值将会是一个列表。

*Livescript:*

```livescript
for i from 1 to 10 by 3
  i
```

*Javascript:*

```javascript
var i$, i, ref$, len$, val, key;
for (i$ = 1; i$ <= 10; i$ += 3) {
  i = i$;
  i;
}
```

`for ... in`循环遍历一个列表。它有着这样的结构：`for (let) (VAL-VAR)(, INDEX-VAR) in EXP (by NUM) (when COND)`——和刚才一样，几乎所有项都是可选的。

`let`，`by`，和`when`的作用和之前一样。

`VAL-VAR`表示列表中当前项的值，而`INDEX-VAR`表示列表中当前项的索引。它们都是可选的。

`EXP`的值应当是一个列表。

*Livescript:*

```livescript
for x in [1 2 3]
  x

xs = for let x, i in [1 to 10] by 2 when x % 3 == 0
  -> i + x
xs[0]! #=> 5
xs[1]! #=> 17
```

*Javascript:*

```javascript
var i$, ref$, len$, x, xs, res$;
for (i$ = 0, len$ = (ref$ = [1, 2, 3]).length; i$ < len$; ++i$) {
  x = ref$[i$];
  x;
}
res$ = [];
for (i$ = 0, len$ = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10].length; i$ < len$; i$ += 2) {
  if ([1, 2, 3, 4, 5, 6, 7, 8, 9, 10][i$] % 3 === 0) {
    res$.push((fn$.call(this, i$, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10][i$])));
  }
}
xs = res$;
xs[0]();
xs[1]();
function fn$(i, x){
  return function(){
    return i + x;
  };
}
```

`for ... of`循环遍历一个对象。它有着这样的结构：`for (own) (let) (KEY-VAR)(, VAL-VAR) of EXP (when COND)`——和刚才一样，几乎所有项都是可选的。

`let`和`when`的作用和之前一样。

`own`使用`hasOwnProperty`函数来检测属性，不会遍历在原型链上更高层的属性。

`KEY-VAR`表示属性的关键字，`VAL-VAR`表示属性的值。它们都是可选的。

`EXP`的值应当是一个对象。

*Livescript:*

```livescript
for k, v of {a: 1, b: 2}
  "#k#v"

xs = for own let key, value of {a: 1, b: 2, c: 3, d: 4} when value % 2 == 0
  -> key + value
xs[0]! #=> 'b2'
xs[1]! #=> 'd4'
```

*Javascript:*

```javascript
var k, ref$, v, xs, res$, i$, own$ = {}.hasOwnProperty;
for (k in ref$ = {
  a: 1,
  b: 2
}) {
  v = ref$[k];
  k + "" + v;
}
res$ = [];
for (i$ in ref$ = {
  a: 1,
  b: 2,
  c: 3,
  d: 4
}) if (own$.call(ref$, i$)) {
  if (ref$[i$] % 2 === 0) {
    res$.push((fn$.call(this, i$, ref$[i$])));
  }
}
xs = res$;
xs[0]();
xs[1]();
function fn$(key, value){
  return function(){
    return key + value;
  };
}
```

普通的嵌套循环的值将会是一个包含若干列表的列表。

*Livescript:*

```livescript
result = for x to 3
  for y to 2
    x + y
result #=> [[0, 1, 2], [1, 2, 3], [2, 3, 4], [3, 4, 5]]
```

*Javascript:*

```javascript
var result, res$, i$, x, lresult$, j$, y;
res$ = [];
for (i$ = 0; i$ <= 3; ++i$) {
  x = i$;
  lresult$ = [];
  for (j$ = 0; j$ <= 2; ++j$) {
    y = j$;
    lresult$.push(x + y);
  }
  res$.push(lresult$);
}
result = res$;
result;
```

你可以省略`in`/`of`循环中的一个变量，或者都省略。

*Livescript:*

```livescript
res = for , i in [1 2 3]
  i
res #=> [0, 1, 2]

for til 3 then func!
# 调用func函数3次

[6 for til 3] #=> [6, 6, 6]
```

*Javascript:*

```javascript
var res, res$, i$, len$, i;
res$ = [];
for (i$ = 0, len$ = [1, 2, 3].length; i$ < len$; ++i$) {
  i = i$;
  res$.push(i);
}
res = res$;
res;
for (i$ = 0; i$ < 3; ++i$) {
  func();
}
for (i$ = 0; i$ < 3; ++i$) {
  6;
}
```

列表推导（*list comprehensions*）会产生一个列表。嵌套的列表推导产生一个扁平的列表（*flattened list*）。

*Livescript:*

```livescript
[x + 1 for x to 10 by 2 when x isnt 4]
#=> [1,3,7,9,11]

["#x#y" for x in [\a \b] for y in [1 2]]
#=> ['a1','a2','b1','b2']
```

*Javascript:*

```javascript
var i$, x, ref$, len$, j$, ref1$, len1$, y;
for (i$ = 0; i$ <= 10; i$ += 2) {
  x = i$;
  if (x !== 4) {
    x + 1;
  }
}
for (i$ = 0, len$ = (ref$ = ['a', 'b']).length; i$ < len$; ++i$) {
  x = ref$[i$];
  for (j$ = 0, len1$ = (ref1$ = [1, 2]).length; j$ < len1$; ++j$) {
    y = ref1$[j$];
    x + "" + y;
  }
}
```

你可以使用空格来更好地格式化推导。

*Livescript:*

```livescript
[{id:id1, name, age} for {id:id1, name} in table1
                     for {id:id2, age} in table2
                     when id1 is id2]
```

*Javascript:*

```javascript
var i$, ref$, len$, ref1$, id1, name, j$, len1$, ref2$, id2, age;
for (i$ = 0, len$ = (ref$ = table1).length; i$ < len$; ++i$) {
  ref1$ = ref$[i$], id1 = ref1$.id, name = ref1$.name;
  for (j$ = 0, len1$ = (ref1$ = table2).length; j$ < len1$; ++j$) {
    ref2$ = ref1$[j$], id2 = ref2$.id, age = ref2$.age;
    if (id1 === id2) {
      ({
        id: id1,
        name: name,
        age: age
      });
    }
  }
}
```

你可以使用级联（*cascade*）来隐式指向被映射的值。

*Livescript:*

```livescript
[.. + 1 for [1 2 3]] #=> [2, 3, 4]

list-of-obj =
  * name: 'Alice'
    age: 23
  * name: 'Betty'
    age: 26

[..name for list-of-obj] #=> ['Alice', 'Betty']
```

*Javascript:*

```javascript
var i$, x$, ref$, len$, listOfObj, y$;
for (i$ = 0, len$ = (ref$ = [1, 2, 3]).length; i$ < len$; ++i$) {
  x$ = ref$[i$];
  x$ + 1;
}
listOfObj = [
  { name: 'Alice',
    age: 23 },
  { name: 'Betty',
    age: 26 }
];
for (i$ = 0, len$ = listOfObj.length; i$ < len$; ++i$) {
  y$ = listOfObj[i$];
  y$.name;
}
```

对象推导（*object comprehensions*）产生一个对象。

*Livescript:*

```livescript
{[key, val * 2] for key, val of {a: 1, b: 2}}
#=> {a: 2, b: 4}
```

*Javascript:*

```javascript
var key, ref$, val;
for (key in ref$ = {
  a: 1,
  b: 2
}) {
  val = ref$[key];
  [key, val * 2];
}
```

`while`循环

*Livescript:*

```livescript
i = 0
list = [1 to 10]
while n < 9
  n = list[++i]
```

*Javascript:*

```javascript
var i, list, n;
i = 0;
list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
while (n < 9) {
  n = list[++i];
}
```

`until`等价于`while not`。

`while`/`until`循环也可以接受（*accept*）`when`筛选，并且有一个可选的`else`从句，当条件不满足时执行。

*Livescript:*

```livescript
i = 1
list = [1 to 10]
until i > 7 when n isnt 99
  n = list[++i]
else
  10
```

*Javascript:*

```javascript
var i, list, yet$, n;
i = 1;
list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
for (yet$ = true; !(i > 7);) {
  yet$ = false;
  if (n !== 99) {
    n = list[++i];
  }
} if (yet$) {
  10;
}
```

Do while循环：

*Livescript:*

```livescript
i = 0
list = [1 to 10]
do
  i++
while list[i] < 9
```

*Javascript:*

```javascript
var i, list;
i = 0;
list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
do {
  i++;
} while (list[i] < 9);
```

while循环也可以接受一句更新从句。

*Livescript:*

```livescript
i = 0
list = [1 to 10]
while list[i] < 9, i++ then i
```

*Javascript:*

```javascript
var i, list;
i = 0;
list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
for (; list[i] < 9; i++) {
  i;
}
```

while true循环：

*Livescript:*

```livescript
i = 0
loop
  \ha
  break if ++i > 20

i = 0
for ever
  \ha
  if ++i > 20
     break
```

*Javascript:*

```javascript
var i;
i = 0;
for (;;) {
  'ha';
  if (++i > 20) {
    break;
  }
}
i = 0;
for (;;) {
  'ha';
  if (++i > 20) {
    break;
  }
}
```