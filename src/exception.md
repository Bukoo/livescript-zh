# 异常

你可以使用`throw`来抛出异常。

*LiveScript*
```ls
throw new Error 'an error has occurred!'
```

*JavaScript*
```js
throw new Error('an error has occurred!');
```

你可以使用`try`，`catch`，`finally`代码块来捕获和处理异常。`catch`和`finally`都是可选的。

`try`代码块是需要的。如果异常发生了，`catch`代码块会被执行。一个异常对象会被传进来。如果你没有指定异常对象的变量名，默认会是`e`。与JavaScript不同，异常变量的作用域是最接近它的函数，而不是`catch`代码块。你可以自己解构异常变量。

*LiveScript*
```ls
try
  ...

try
  ...
catch
  2 + 2
e.message

x = try
  ...
catch {message}
  message

x #=> unimplemented
```

*JavaScript*
```js
var e, x, message;
try {
  throw Error('unimplemented');
} catch (e$) {}
try {
  throw Error('unimplemented');
} catch (e$) {
  e = e$;
  2 + 2;
}
e.message;
x = (function(){
  try {
    throw Error('unimplemented');
  } catch (e$) {
    message = e$.message;
    return message;
  }
}());
x;
```

`finally`代码块在`try`或者`catch`代码块执行之后执行。

*LiveScript*
```ls
try
  ...
catch
  handle-exception e
finally
  do-something!

try
  ...
finally
  do-something!
```

*JavaScript*
```js
var e;
try {
  throw Error('unimplemented');
} catch (e$) {
  e = e$;
  handleException(e);
} finally {
  doSomething();
}
try {
  throw Error('unimplemented');
} finally {
  doSomething();
}
```

