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
