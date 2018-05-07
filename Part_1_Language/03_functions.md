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

## 绑定和作用域

每一个绑定都有 *作用域*，即：绑定可见的那部分程序。对于定义在任何函数或块之外的绑定，作用域就是整个程序 —— 你可以在任何地方引用它们。这些叫作 *全局绑定*。

但是作为函数参数而创建或者在函数内部声明的绑定，就只能在那个函数内被引用。这些叫作 *局部绑定*。每次函数被调用，这些绑定的新的实例将被创建。这就在函数间提供了一些隔离 —— 每个函数调用在其自身的小世界(它的局部环境)工作，并且不需要知道太多全局环境中发生的事情就能被理解。

用 `let` 和 `const` 声明的绑定，实际上对于其声明所在的 *块* 是局部的，因此如果你在循环中创建了其中之一，那么循环之前和之后的代码是 “看不见” 它的。在 2015 版本之前的`JavaScript`中，只有函数能创建作用域，所以用 `var` 关键字创建的旧风格的绑定，在其出现的整个函数中可见 —— 或者如果不在函数中，则全局作用域中可见。

```js
let x = 10;
if (true) {
  let y = 20;
  var z = 30;
  console.log(x + y + z);
  // → 60
}
// y is not visible here
console.log(x + z);
// → 40
```

每一个作用域可以在周围的作用域中 “查找”，因此例子中 `x` 在块中可见。例外是，当多个绑定有相同的名称时 —— 代码只能看见最内层的一个。例如，当 `halve` 函数内部的代码引用 `n` 时，它看到的是它 *自己的* `n`，而不是全局的 `n`。

```js
const halve = function(n) {
  return n / 2;
};

let n = 10;
console.log(halve(100));
// → 50
console.log(n);
// → 10
```

### 嵌套作用域

`JavaScript`不仅仅区分 *全局* 和 *局部* 绑定。块和函数能在其他块和函数中创建，形成了多层的局部性。

比如，这个函数 —— 输出制作一批鹰嘴豆泥所需的原料，其中含有另一个函数：

```js
const hummus = function(factor) {
  const ingredient = function(amount, unit, name) {
    let ingredientAmount = amount * factor;
    if (ingredientAmount > 1) {
      unit += "s";
    }
    console.log(`${ingredientAmount} ${unit} ${name}`);
  };
  ingredient(1, "can", "chickpeas");
  ingredient(0.25, "cup", "tahini");
  ingredient(0.25, "cup", "lemon juice");
  ingredient(1, "clove", "garlic");
  ingredient(2, "tablespoon", "olive oil");
  ingredient(0.5, "teaspoon", "cumin");
};
```

`ingredient` 函数中的代码可以看到来自外层函数的 `factor` 绑定。但是它的局部搬到，比如 `unit` 或者 `ingredientAmount`，在外层函数中是不可见的。

简言之，每一个局部作用域也可以看到所有包含它的局部作用域。块中可见的绑定集合，由这个块在程序中的位置所决定。所有来自 *周围* 块的绑定是可见的 —— 包括围绕在周围的块中的绑定和在程序顶层的那些。绑定可见性的这种方式叫作 *词法作用域*。

[chapter_picture_3]: ../assets/chapter_picture_3.jpg
