# 属性访问

标准方式：

```ls
[1 2 3][1]     #=> 2
{a: 1, b: 2}.b #=> 2
```

```js
[1, 2, 3][1];
({
  a: 1,
  b: 2
}).b;
```

Dot Access - dot operators can accept many more things other than identifiers as their right operand, including numbers, strings, parentheses, brackets, and braces.

点访问 - 点访问运算符可以接受

```ls
x = "hello world": [4 [5 boom: 6]]
x.'hello world'.1.[0] #=> 5
```

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

