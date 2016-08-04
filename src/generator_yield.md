# Generators and Yield

你可以在LiveScript代码中使用生成器和`yield`了！一个简单的例子：

``` livescript
function* f
    yield "foo"

g = ->*
    yield from f!
    yield "bar"

h = g!
h.next!.value + h.next!.value #=> "foobar"
```

``` javascript
var g, h;
function* f(){
  return (yield "foo");
}
g = function*(){
  (yield* f());
  return (yield "bar");
};
h = g();
h.next().value + h.next().value;
```

你可以通过在`function`关键字或者在LiveScript的箭头符号后面加`*`来创建生成器。这种方法对于LiveScript的各种箭头符号都是有效的。

`yield`的使用和JavaScript中是一样的，`yield from`和JavaScript中`yield *`是相同的。

使用node 0.11运行使用了生成器和`yield`的代码，需要使用`--harmony`标识。如果直接使用`lsc`来运行，需要使用命令`lsc file.ls --nodejs --harmony`来将harmony标识传递给node。
