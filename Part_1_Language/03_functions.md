# 第三章、函数

> 人们认为计算机科学是天才们的艺术，但事实上正相反，只不过是很多人在彼此基础上做事情而已，就像一堵小石头砌成的墙。
>
> -- Donald Knuth

![chapter_picture_3][chapter_picture_3]

函数是`JavaScript`编程的基本组成部分。把一段程序封装到一个值的概念有很多用途。它给了我们构建更大程序、减少重复、将名称与子程序关联以及让这些子程序互相独立的一种方式。

函数最显而易见的应用就是定义新的词汇。在散文中创造新单词通常并不是好的方式。但是在编程里，这是很有必要的。

典型的说英语的成年人大约有 20,000 个单词的词汇量。几乎没有编程语言内建有 20,000 条命令。并且 *确实* 可用的词汇也是更加精确地定义的，因此不及人类语言灵活。故而，我们通常 *不得不* 引入新的概念，以避免过多的重复。

## 定义函数

一个函数定义，就是一个常规的绑定，此绑定的值是一个函数。比如，这段代码定义了 `square` 来指向一个生成指定数字平方的函数。

```js
const square = function(x) {
  return x * x;
};

console.log(square(12));
// → 144
```

函数由以关键字 `function` 开头的表达式创建。函数有一系列 *参数* (当前情况，只有 `x`)和 *函数体* (其中包含当程序被调用时要运行的语句)。这种方式创建的函数的函数体，必须放在大括号中，即使它只包含一条语句。

一个函数可以有多个参数，或者完全没有参数。在下面的例子中，`makeNoise` 没有任何参数，而 `power` 有 2 个：

```js
const makeNoise = function() {
  console.log("Pling!");
};

makeNoise();
// → Pling!

const power = function(base, exponent) {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
};

console.log(power(2, 10));
// → 1024
```

一些函数会生成一个值，比如 `power` 和 `square`，一些则不会，比如 `makeNoise`，它唯一的结果就是其副作用。`return` 语句决定了函数的返回值。当控制流遇到这样一条语句，就会立即跳出当前函数，并将其返回值传给调用此函数的代码。`return` 关键字后不跟表达式，会使函数返回 `undefined`。对于根本没有 `return` 语句的函数，比如 `makeNoise`，同样是返回 `undefined`。

[chapter_picture_3]: ../assets/chapter_picture_3.jpg
