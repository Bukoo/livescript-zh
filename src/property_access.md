# 属性访问

标准方式：
*LiveScript*
```ls
[1 2 3][1]     #=> 2
{a: 1, b: 2}.b #=> 2
```

*JavaScript*
```js
[1, 2, 3][1];
({
  a: 1,
  b: 2
}).b;
```

点访问 - 点访问运算符可以接受标识符以外的更多其他符号作为它的右操作数，包括数字，字符串，圆括号，中括号和花括号。

*LiveScript*
```ls
x = "hello world": [4 [5 boom: 6]]
x.'hello world'.1.[0] #=> 5
```

*JavaScript*
```js
var x;
x = {
  "hello world": [
    4, [
      5, {
        boom: 6
      }
    ]
  ]
};
x['hello world'][1][0];
```

使用`.=`符号进行属性访问。

*LiveScript*
```ls
document.title .= to-upper-case! #=> LIVESCRIPT ...
```

*JavaScript*
```js
document.title = document.title.toUpperCase();
```

数组slice和splice

*LiveScript*
```ls
list = [1 2 3 4 5]
list[2, 4]         #=> [3,5]
list[1 to 3]       #=> [2,3,4]
list[1 til 3]      #=> [2,3]
list[1 til 4 by 2] #=> [2,4]
list[1 til 3] = [7 8]
list               #=> [1,7,8,4,5]
```

*JavaScript*
```js
var list, ref$;
list = [1, 2, 3, 4, 5];
[list[2], list[4]];
[list[1], list[2], list[3]];
[list[1], list[2]];
[list[1], list[3]];
ref$ = [7, 8], list[1] = ref$[0], list[2] = ref$[1];
list;
```

对象slice：

*LiveScript*
```ls
obj = one: 1, two: 2
obj{first: one, two} #=> {first: 1, two: 2}
```

*JavaScript*
```js
var obj;
obj = {
  one: 1,
  two: 2
};
({
  first: obj.one,
  two: obj.two
});
```

列表的长度用`*`表示。

*LiveScript*
```ls
list = [1 2 3 4 5]
list[*] = 6
list        #=> [1,2,3,4,5,6]
list[*-1]   #=> 6
```

*JavaScript*
```js
var list;
list = [1, 2, 3, 4, 5];
list[list.length] = 6;
list;
list[list.length - 1];
```

半自动的有效性校验符号`.{}`，`.[]`能确保属性是对象或者数组

*LiveScript*
```ls
x = "hello world": [4 [5 boom: 6]]
x.[]'hello world'.1.{}1.boom #=> 6


x.[]arr.{}1.y = 9
x.arr.1.y #=> 9
```

*JavaScript*
```js
var x, ref$;
x = { "hello world": [ 4, [ 5, { boom: 6 } ] ] };

((ref$ = (x['hello world'] || (x['hello world'] = []))[1])[1] || (ref$[1] = {})).boom;
((ref$ = x.arr || (x.arr = []))[1] || (ref$[1] = {})).y = 9;
```

绑定访问运算符`.~`能得到绑定到对象本身的对象的方法。`.`运算符会自动插入，所以你可以直接使用`~`运算符。

*LiveScript*

```ls
obj =
  x: 5
  add: (y) -> @x + y

target =
  x: 600
  not-bound: obj.add
  bound: obj~add

target.not-bound 5 #=> 605
target.bound 5     #=> 10
```

*JavaScript*
```js
var obj, target;
obj = {
  x: 5,
  add: function(y){
    return this.x + y;
  }
};
target = {
  x: 600,
  notBound: obj.add,
  bound: bind$(obj, 'add')
};
target.bound(5);
function bind$(obj, key, target){
  return function(){ return (target || obj)[key].apply(obj, arguments) };
}
```

## 级联

级联就是被访问的元素，它不会返回访问操作所得到的值。

漂亮的链式访问：

*LiveScript*
```ls
a = [2 7 1 8]
  ..push 3
  ..shift!
  ..sort!
a #=> [1,3,7,8]

document.query-selector \h1
  ..style
    ..color = \red
    ..font-size = \large
  ..inner-HTML = 'LIVESCRIPT!'
```

*JavaScript*
```js
var x$, a, y$;
x$ = a = [2, 7, 1, 8];
x$.push(3);
x$.shift();
x$.sort();
a;

x$ = document.querySelector('h1');
y$ = x$.style;
y$.color = 'red';
y$.fontSize = 'large';
x$.innerHTML = 'LIVESCRIPT!';
```

级联是可调用的，可以包含任意的代码。

*LiveScript*
```ls
console.log
  x = 1
  y = 2
  .. x, y
# prints `1 2` to the console
```

*JavaScript*
```js
var x$, x, y;
x$ = console.log;
x = 1;
y = 2;
x$(x, y);
```

使用`with`来指定级联的前置表达式部分。

*LiveScript*
```ls
x = with {a: 1, b: 2}
  ..a = 7
  ..b += 9
x #=>  {a: 7, b: 11}
```

*JavaScript*
```js
var x, x$;
x = (x$ = {
  a: 1,
  b: 2
}, x$.a = 7, x$.b += 9, x$);
x;
```
