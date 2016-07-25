# 面向对象编程

---
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

---
静态属性（构造函数拥有的属性）以在最高层给```this```添加属性的方式来定义。这些属性可通过获取构造函数来获取（简写为```@@```）。

*LiveScript*
```ls
class A
  @static-prop = 10
  get-static: ->
    @@static-prop + 2

A.static-prop #=> 10
a = new A
a.get-static! #=> 12
```

*JavaScript*
```js
var A, a;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  A.staticProp = 10;
  prototype.getStatic = function(){
    return constructor.staticProp + 2;
  };
  function A(){}
  return A;
}());
A.staticProp;
a = new A;
a.getStatic();
```

---
私有静态属性仅在函数体中作为常规变量存在。（注：JavaScript并没有实现私有实例属性，这正是LiveScript的一大亮点。）

*LiveScript*
```ls
class A
  secret = 10

  get-secret: ->
    secret

a = new A
a.get-secret! #=> 10
```

*JavaScript*
```js
var A, a;
A = (function(){
  A.displayName = 'A';
  var secret, prototype = A.prototype, constructor = A;
  secret = 10;
  prototype.getSecret = function(){
    return secret;
  };
  function A(){}
  return A;
}());
a = new A;
a.getSecret();
```

---
你可以定义绑定方法（使用```~>```，它将指向函数本体的```this```绑定到实例上。）

*LiveScript*
```ls
class A
  x: 10
  bound-func: (x) ~>
    @x
  reg-func: (x) ->
    @x

a = new A
obj =
  x: 1
  bound: a.bound-func
  reg: a.reg-func

obj.bound! #=> 10
obj.reg!   #=> 1
```

*JavaScript*
```js
var A, a, obj;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  prototype.x = 10;
  prototype.boundFunc = function(x){
    return this.x;
  };
  prototype.regFunc = function(x){
    return this.x;
  };
  function A(){
    this.boundFunc = bind$(this, 'boundFunc', prototype);
  }
  return A;
}());
a = new A;
obj = {
  x: 1,
  bound: a.boundFunc,
  reg: a.regFunc
};
obj.bound();
obj.reg();
function bind$(obj, key, target){
  return function(){ return (target || obj)[key].apply(obj, arguments) };
}
```

---
使用对象设置参数的语法糖，可以让你方便在构造函数与方法中设置属性。

*LiveScript*
```ls
class A
  (@x) ->

  f: (@y) ->
    @x + @y

a = new A 2
a.x   #=> 2
a.f 3 #=> 5
a.y   #=> 3
```

*JavaScript*
```js
var A, a;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  function A(x){
    this.x = x;
  }
  prototype.f = function(y){
    this.y = y;
    return this.x + this.y;
  };
  return A;
}());
a = new A(2);
a.x;
a.f(3);
a.y;
```

---
如果你将构造函数定义为绑定函数```~>```，在创建新实例的时候，可以免去使用```new```关键字的麻烦。

*LiveScript*
```ls
class A
  (@x) ~>

a = A 4
a.x #=> 4
```

*JavaScript*
```js
var A, a;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  function A(x){
    var this$ = this instanceof ctor$ ? this : new ctor$;
    this$.x = x;
    return this$;
  } function ctor$(){} ctor$.prototype = prototype;
  return A;
}());
a = A(4);
a.x;
```

---
对于更高等级的库和框架，可以将构造函数定义为constructor