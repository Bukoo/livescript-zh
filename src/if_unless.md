# 条件结构

格式化一条`if`语句有好几种方法。（*注意，`if`语句实际上是而且也是被当成一条表达式来使用的*）。

标准形式：

*Livescript:*

```livescript
if 2 + 2 == 4
  'something'
else
  'something else'

if 2 + 2 == 4 then 'something' else 'something else'

if 2 + 2 == 4
then 'something'
else 'something else'
```

*Javascript:*

```javascript
if (2 + 2 === 4) {
  'something';
} else {
  'something else';
}
if (2 + 2 === 4) {
  'something';
} else {
  'something else';
}
if (2 + 2 === 4) {
  'something';
} else {
  'something else';
}
```

`else`是可选的，你还可以添加更多的`else if`。

*Livescript:*

```livescript
if 2 + 2 == 4
  'something'

if 2 + 2 == 6
  'something'
else if 2 + 2  == 5
  'something else'
else
  'the default'
```

*Javascript:*

```javascript
if (2 + 2 === 4) {
  'something';
}
if (2 + 2 === 6) {
  'something';
} else if (2 + 2 === 5) {
  'something else';
} else {
  'the default';
}
```

它（`if`语句）可以当做表达式使用：

*Livescript:*

```livescript
result = if 2 / 2 is 0
         then 'something'
         else 'something else'
```

*Javascript:*

```javascript
var result;
result = 2 / 2 === 0 ? 'something' : 'something else';
```

你还能以后缀的形式使用`if`语句——它的优先级比赋值低，这使得这个特性很有用：

*Livescript:*

```livescript
x = 10
x = 3 if 2 + 2 == 4
x #=> 3
```

*Javascript:*

```javascript
var x;
x = 10;
if (2 + 2 === 4) {
  x = 3;
}
x;
```

`unless`等价于`if not`。

*Livescript:*

```livescript
unless 2 + 2 == 5
  'something'

x = 10
x = 3 unless 2 + 2 == 5
```

*Javascript:*

```javascript
var x;
if (2 + 2 !== 5) {
  'something';
}
x = 10;
if (2 + 2 !== 5) {
  x = 3;
}
```

`that`隐式地引用判断条件的值。然而，它不会检查该值是否存在。 

*Livescript:*

```livescript
time = days: 365

half-year = that / 2 if time.days
#=> 182.5

if /^e(.*)/ == 'enter'
  that.1   #=> 'nter'

if half-year?
  that * 2 #=> 365
```

*Javascript:*

```javascript
var time, that, halfYear;
time = {
  days: 365
};
if (that = time.days) {
  halfYear = that / 2;
}
if (that = /^e(.*)/.exec('enter')) {
  that[1];
}
if ((that = halfYear) != null) {
  that * 2;
}
```
