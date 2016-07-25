# 面向对象编程

类是一种语法糖，本意为构造函数的定义及其原型的设置而设计。
构造函数被定义为函数文本，处于类定义的最高层。
其原型的属性也在最高层由对象文本来定义。

*LiveScript*
```livescript
class A
  (num) ->
    @x = num
  property: 1
  method: (y) ->
    @x + @property + y

a = new A 3
a.x        #=> 3
a.property #=> 1
a.method 6 #=> 10
```
*JavaScript*
```js
var A, a;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  function A(num){
    this.x = num;
  }
  prototype.property = 1;
  prototype.method = function(y){
    return this.x + this.property + y;
  };
  return A;
}());
a = new A(3);
a.x;
a.property;
a.method(6);
```
静态属性（构造函数拥有的属性）以在最高层给```this```添加属性的方式来定义。这些属性可通过获取构造函数来获取（简写为```@@```）。
