# 概述

LiveScript是一门编译到JavaScript的语言。它和JavaScript之间有着很直接的映射关系，又能让你写出有具有较高表达能力、无重复的代码。而且，LiveScript增加了很多特性来支持函数式编程，同时它在面向对象编程和命令式编程上也有很多提升。

---

LiveScript是[Coco](https://github.com/satyr/coco)的一份fork，是CoffeeScript的间接后代，和CoffeeScript有很多兼容的地方。

1.5.0： [zip](https://github.com/gkz/LiveScript/zipball/1.5.0) ，[tar.gz](https://github.com/gkz/LiveScript/tarball/1.5.0)，[View project on Github](https://github.com/gkz/LiveScript)，[Star](https://github.com/gkz/LiveScript/)

## 例子

*LiveScript:*

```ls
# 列出隐式的对象是很简单的
table1 =
  * id: 1
    name: 'george'
  * id: 2
    name: 'mike'
  * id: 3
    name: 'donald'

table2 =
  * id: 2
    age: 21
  * id: 1
    age: 20
  * id: 3
    age: 26

# 隐式的访问和赋值
up-case-name = (.name .= to-upper-case!)

# List comprehensions, 解构(destructuring), 管道(piping)
[{id:id1, name, age} for {id:id1, name} in table1
                     for {id:id2, age} in table2
                     when id1 is id2]
|> sort-by (.id) # using 'sort-by' from prelude.ls
|> each up-case-name # using 'each' from prelude.ls
|> JSON.stringify
#=>
#[{"id":1,"name":"GEORGE","age":20},
# {"id":2,"name":"MIKE",  "age":21},
# {"id":3,"name":"DONALD","age":26}]















# 操作符作为函数，管道
map (.age), table2 |> fold1 (+)
#=> 67 ('fold1' and 'map' from prelude.ls)

```

*JavaScript:*

```js
var table1, table2, upCaseName, id1, name, id2, age;
table1 = [
  { id: 1,
    name: 'george' },
  { id: 2,
    name: 'mike' },
  { id: 3,
    name: 'donald' }
];
table2 = [
  { id: 2,
    age: 21 },
  { id: 1,
    age: 20 },
  { id: 3,
    age: 26 }
];
upCaseName = function(it){
  return it.name = it.name.toUpperCase();
};
JSON.stringify(
each(upCaseName)(
sortBy(function(it){
  return it.id;
})(
(function(){
  var i$, ref$, len$, ref1$, j$, len1$, ref2$, results$ = [];
  for (i$ = 0, len$ = (ref$ = table1).length; i$ < len$; ++i$) {
    ref1$ = ref$[i$], id1 = ref1$.id, name = ref1$.name;
    for (j$ = 0, len1$ = (ref1$ = table2).length; j$ < len1$; ++j$) {
      ref2$ = ref1$[j$], id2 = ref2$.id, age = ref2$.age;
      if (id1 === id2) {
        results$.push({
          id: id1,
          name: name,
          age: age
        });
      }
    }
  }
  return results$;
}()))));
fold1(curry$(function(x$, y$){
  return x$ + y$;
}))(
map(function(it){
  return it.age;
}, table2));
function curry$(f, bound){
  var context,
  _curry = function(args) {
    return f.length > 1 ? function(){
      var params = args ? args.concat() : [];
      context = bound ? context || this : this;
      return params.push.apply(params, arguments) <
          f.length && arguments.length ?
        _curry.call(context, params) : f.apply(context, params);
    } : f;
  };
  return _curry();
}
```

非嵌套的回调写法和无需括号的链接(*chaining*)

*LiveScript:*

```ls
<- $ 'h1' .on 'click'
alert 'boom!'
```

*JavaScript:*

```js
$('h1').on('click', function(){
  return alert('boom!');
});
```
