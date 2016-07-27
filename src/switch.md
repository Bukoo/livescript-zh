# 选择结构

`break`将会被自动插入到每个`case`的末尾。你可以一个`case`里使用多个条件。

*Livescript:*

```livescript
switch 6
case 1    then \hello
case 2, 4 then \boom
case 6
  'here it is'
default \something
```

*Javascript:*

```javascript
switch (6) {
case 1:
  'hello';
  break;
case 2:
case 4:
  'boom';
  break;
case 6:
  'here it is';
  break;
default:
  'something';
}
```

如果不指定`switch`的值，它将会默认为`true`。（*编译成值为`false`，这样它只用一个`!`就能把表达式转化成布尔值*）

*Livescript:*

```livescript
switch
case 5 == 6
  \never
case false
  'also never'
case 6 / 2 is 3
  'here'
```

*Javascript:*

```javascript
switch (false) {
case 5 !== 6:
  'never';
  break;
case !false:
  'also never';
  break;
case 6 / 2 !== 3:
  'here';
}
```

你可以使用`fallthrough`来阻止`break`的自动插入。`fallthrough`必须是`case`块的最后一条表达式。另外，你可以把`switch`语句当做表达式使用。

*Livescript:*

```livescript
result = switch 6
case 6
  something = 5
  fallthrough
case 4
  'this is it'

result #=> 'this is it'
```

*Javascript:*

```javascript
var something, result;
result = (function(){
  switch (6) {
  case 6:
    something = 5;
    // fallthrough
  case 4:
    return 'this is it';
  }
}());
result;
```

`|`是`case`的别名，`=>`是`then`的别名。`| otherwise`和`| _`是`default`的别名。

*Livescript:*

```livescript
switch 'moto'
| "some thing"     => \hello
| \explosion \bomb => \boom
| <[ the moto ? ]> => 'here it is'
| otherwise        => \something
```

*Javascript:*

```javascript
switch ('moto') {
case "some thing":
  'hello';
  break;
case 'explosion':
case 'bomb':
  'boom';
  break;
case 'the':
case 'moto':
case '?':
  'here it is';
  break;
default:
  'something';
}
```

你可以在`switch`语句里使用`that`（这里的`that`就是`switch`后面的表达式的值）：

*Livescript:*

```livescript
switch num
| 2         => console.log that
| otherwise => console.log that
```

*Javascript:*

```javascript
var that;
switch (that = num) {
case 2:
  console.log(that);
  break;
default:
  console.log(that);
}
```

如果在箭头（*例如`->`*）、`:`、`=`后是`case`(或者它的别名`|`)，编译时将会隐式地添加一个`switch`。

*Livescript:*

```livescript
func = (param) ->
  | param.length < 5 => param.length
  | otherwise        => param.slice 3

func 'hello' #=> lo

state = | 2 + 2 is 5 => 'I love Big Brother'
        | _          => 'I love Julia'
```

*Javascript:*

```javascript
var func, state;
func = function(param){
  switch (false) {
  case !(param.length < 5):
    return param.length;
  default:
    return param.slice(3);
  }
};
func('hello');
state = (function(){
  switch (false) {
  case 2 + 2 !== 5:
    return 'I love Big Brother';
  default:
    return 'I love Julia';
  }
}());
```

你也可以使用`CoffeeScript`风格的`switch`语句。

*Livescript:*

```livescript
day = \Sun
switch day
  when "Mon" then 'go to work'
  when "Tue" then 'go to a movie'
  when "Thu" then 'go drinking'
  when "Fri", "Sat"
      'go dancing'
  when "Sun" then 'drink more'
  else 'go to work'
```

*Javascript:*

```javascript
var day;
day = 'Sun';
switch (day) {
case "Mon":
  'go to work';
  break;
case "Tue":
  'go to a movie';
  break;
case "Thu":
  'go drinking';
  break;
case "Fri":
case "Sat":
  'go dancing';
  break;
case "Sun":
  'drink more';
  break;
default:
  'go to work';
}
```