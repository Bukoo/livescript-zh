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
对于更高等级的库和框架，可以定义构造函数为```（待定）```，将其作为外置函数。但这种做法不建议运用在常规开发中。

*LiveScript*
```ls
f = (@x) ->

class A
  constructor$$: f

a = new A 5
a.x #=> 5
```

*JavaScript*
```js
待定
```

---
使用```extends```关键字来实现继承。

*LiveScript*
```ls
class A
  ->
    @x = 1
  @static-prop = 8
  method: ->
    @x + 2

class B extends A
  ->
    @x = 10

B.static-prop #=> 8
b = new B
b.x       #=> 10
b.method! #=> 12
```

*JavaScript*
```js
var A, B, b;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  function A(){
    this.x = 1;
  }
  A.staticProp = 8;
  prototype.method = function(){
    return this.x + 2;
  };
  return A;
}());
B = (function(superclass){
  var prototype = extend$((import$(B, superclass).displayName = 'B', B), superclass).prototype, constructor = B;
  function B(){
    this.x = 10;
  }
  return B;
}(A));
B.staticProp;
b = new B;
b.x;
b.method();
function extend$(sub, sup){
  function fun(){} fun.prototype = (sub.superclass = sup).prototype;
  (sub.prototype = new fun).constructor = sub;
  if (typeof sup.extended == 'function') sup.extended(sub);
  return sub;
}
function import$(obj, src){
  var own = {}.hasOwnProperty;
  for (var key in src) if (own.call(src, key)) obj[key] = src[key];
  return obj;
}
```

---
```super```使继承来的格外方便。对于裸函数，```super```将是该函数可能存在的父函数的引用。如果你想以完整的参数列表来调用父函数，你可以使用```super ...```的语法。

*LiveScript*
```ls
class A
  ->
    @x = 1
  method: (num) ->
    @x + num

class B extends A
  ->
    @y = 2
    super!

  method: (num) ->
    @y + super ...

b = new B
b.y #=> 2
b.method 10 #=> 13
```

*JavaScript*
```js
var A, B, b;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  function A(){
    this.x = 1;
  }
  prototype.method = function(num){
    return this.x + num;
  };
  return A;
}());
B = (function(superclass){
  var prototype = extend$((import$(B, superclass).displayName = 'B', B), superclass).prototype, constructor = B;
  function B(){
    this.y = 2;
    B.superclass.call(this);
  }
  prototype.method = function(num){
    return this.y + superclass.prototype.method.apply(this, arguments);
  };
  return B;
}(A));
b = new B;
b.y;
b.method(10);
function extend$(sub, sup){
  function fun(){} fun.prototype = (sub.superclass = sup).prototype;
  (sub.prototype = new fun).constructor = sub;
  if (typeof sup.extended == 'function') sup.extended(sub);
  return sub;
}
function import$(obj, src){
  var own = {}.hasOwnProperty;
  for (var key in src) if (own.call(src, key)) obj[key] = src[key];
  return obj;
}
```

---
通过```implements```你可以使用mixins。一次只能继承一个类，但能mixin任何你想要的对象。如果你需要扩展一个类，而不是一个简单的对象，必须扩展类的原型。

*LiveScript*
```ls
Renameable =
  set-name: (@name) ->
  get-name: -> @name ? @id

class A implements Renameable
  ->
    @id = Math.random! * 1000

a = new A
a.get-name! #=> some random number
a.set-name 'moo'
a.get-name! #=> 'moo'
```

*JavaScript*
```js
var Renameable, A, a;
Renameable = {
  setName: function(name){
    this.name = name;
  },
  getName: function(){
    var ref$;
    return (ref$ = this.name) != null
      ? ref$
      : this.id;
  }
};
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  importAll$(prototype, arguments[0]);
  function A(){
    this.id = Math.random() * 1000;
  }
  return A;
}(Renameable));
a = new A;
a.getName();
a.setName('moo');
a.getName();
function importAll$(obj, src){
  for (var key in src) obj[key] = src[key];
  return obj;
}
```

---
要修改原型可以使用```::```操作符，如果要修改多个属性的话可以使用```::=```操作符。

*LiveScript*
```ls
class A
  prop: 10
  f: ->
    @prop

a = new A
b = new A
a.f! #=> 10

A::prop = 6
a.f! #=> 6
b.f! #=> 6

A ::=
  prop: 5
  f: ->
    @prop + 4
a.f! #=> 9
b.f! #=> 9
```

*JavaScript*
```js
var A, a, b, ref$;
A = (function(){
  A.displayName = 'A';
  var prototype = A.prototype, constructor = A;
  prototype.prop = 10;
  prototype.f = function(){
    return this.prop;
  };
  function A(){}
  return A;
}());
a = new A;
b = new A;
a.f();
A.prototype.prop = 6;
a.f();
b.f();
ref$ = A.prototype;
ref$.prop = 5;
ref$.f = function(){
  return this.prop + 4;
};
a.f();
b.f();
```

---
如果你不想继续支持之前的浏览器版本，而期望使用```Object.defineProperty```，可以使用一下的简写：

*LiveScript*
```ls
class Box
  dimensions:~
    -> @d
    ([width, height]) -> @d = "#{width}x#height"

b = new Box
b.dimensions = [10 5]
b.dimensions #=> '10x5'
```

*Javascript*
```js
var Box, b;
Box = (function(){
  Box.displayName = 'Box';
  var prototype = Box.prototype, constructor = Box;
  Object.defineProperty(prototype, 'dimensions', {
    get: function(){
      return this.d;
    },
    set: function(arg$){
      var width, height;
      width = arg$[0], height = arg$[1];
      this.d = width + "x" + height;
    },
    configurable: true,
    enumerable: true
  });
  function Box(){}
  return Box;
}());
b = new Box;
b.dimensions = [10, 5];
b.dimensions;
```