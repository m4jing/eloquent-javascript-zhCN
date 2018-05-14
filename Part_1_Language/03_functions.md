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

## 作为值的函数

函数绑定通常只是作为一段特殊的程序的名称。这样的绑定只定义一次且不再改变。这样就很容易混淆函数和它的名称。

但是这两者是不同的。一个函数值可以做所有其他值能做的事情 —— 你可以在任意表达式中使用它，不仅仅是调用它。也可以把一个函数值保存到一个新的绑定，将其作为参数传给一个函数，等等。同样地，持有一个函数的绑定也仅仅是一个常规的绑定，如果不是常量的话，可以对其赋一个新值，像这样：

```js
let launchMissiles = function() {
  missileSystem.launch("now");
};
if (safeMode) {
  launchMissiles = function() {/* do nothing */};
}
```

在[第五章][link_chapter_5]，我们将讨论通过把函数传入其他函数所能做的有趣的事情。

## 声明记法

有一个略微简短的方式来创建函数绑定。当 `function` 关键字用在语句的开头时，会有些不同。

```js
function square(x) {
  return x * x;
}
```

这是一个函数 *声明*。这条语句定义了 `square` 绑定并使其指向特定的函数。这样写起来稍微简单一点，并且不需要在函数后加分号。

这种形式的函数定义有一个微妙之处。

```js
console.log("The future says:", future());

function future() {
  return "You'll never have flying cars";
}
```

这段代码正常工作，即使函数是在调用代码 *下面* 定义的。函数声明不属于常规自上而下控制流的一部分。从概念上说，它们被转移到作用域的顶端，并可被那个作用域中的所有代码使用。有时候这很有用，因为我们可以自由地、以看起来有意义的方式组织代码，而不用担心必须在使用之前定义所有的函数。

## 箭头函数

有第三种函数的记法，看起来和其他的非常不一样。它不用 `function` 关键字，而是用由等号和大于号组成的箭头(`=>`)。不要跟“大于等于”(`>=`)操作符混淆。

```js
const power = (base, exponent) => {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
};
```

箭头在参数列表 *之后*，随后是函数体。看起来像是“这个输入(参数)产生了这个结果(函数体)”。

当只有一个参数时，参数列表周围的括号可以省略。如果函数体是一个表达式而不是大括号中的一个块，这个表达式会被此函数返回。因此，这两个 `square` 的定义是一样的：

```js
const square1 = (x) => { return x * x; };
const square2 = x => x * x;
```

当箭头函数完全没有参数时，它的参数列表就是一对空括号。

```js
const horn = () => {
  console.log("Toot");
};
```

没有足够的理由需要在语言中同时拥有箭头函数和 `function` 表达式。除了一些小细节(我们将在[第六章][link_chapter_6]讨论)，它们是一样的。箭头函数在 2015 年被添加，主要是为了以简洁的方式来写函数表达式。我们在[第五章][link_chapter_5]中会大量用到它们。

## 调用栈

函数间的控制流动有些复杂。我们来仔细查看一下。这里是一个有一些函数调用的简单程序：

```js
function greet(who) {
  console.log("Hello " + who);
}
greet("Harry");
console.log("Bye");
```

这个程序的流程大概像这样：对 `greet` 的调用使得控制跳到该函数的开头(第 2 行)。这个函数调用了 `console.log`，它会接管控制并完成任务，然后把控制权返回到第 2 行。此时抵达了 `greet` 函数的末尾，所以它会返回到调用它的地方，也就是第 4 行。接下来的那一行又调用了 `console.log`。在这次调用返回后，程序结束。

我们可以用下图展示控制流：

```
not in function
   in greet
        in console.log
   in greet
not in function
   in console.log
not in function
```

因为函数在返回时必须跳回到调用它的地方，计算机就得记住本次调用发生时的上下文。一种情况里，`console.log` 结束时要回到 `greet` 函数。另一种情况下，它回到了程序的结尾。

计算机存储这个上下文的地方就叫作 *调用栈*。每当一个函数被调用，当前的上下文就被存储在当前“栈”之上。当函数返回时，就删除栈顶层的上下文，并以此继续执行。

存储这个栈需要耗费计算机内存空间。当栈变得太大时，计算机将无法正常工作并报错“栈空间不足”或“太多循环”。下面的代码通过问计算机极其困难的问题演示了这一点，这将会导致两个函数间的无限循环。当然了，如果计算机有无限大的栈，那它 *将* 是无限的。现实是，我们将耗尽空间，或者“爆栈”。

```js
function chicken() {
  return egg();
}
function egg() {
  return chicken();
}
console.log(chicken() + " came first.");
// → ??
```

## 可选参数

如下代码是允许的，运行起来毫无问题：

```js
function square(x) { return x * x; }
console.log(square(4, true, "hedgehog"));
// → 16
```

我们定义的 `square` 只有 1 个参数。但是在调用时却用了 3 个，语言正常执行。多余的参数被忽略并计算出第一个的平方。

`JavaScript` 对于你传进函数的参数个数是非常大度的。如果你传的太多，多余的就会被忽略。如果传的太少，缺失的参数将被赋值为 `undefined`。

这样做的缺点是 —— 很可能你会给函数传入错误数量的参数，而没有人会告诉你。

优点是 —— 这个行为使得函数可以以不同数量的参数被调用。比如这个 `minus` 函数 —— 尝试模仿 `-` 操作符：对于 1 或 2 个参数都工作。

```js
function minus(a, b) {
  if (b === undefined) return -a;
  else return a - b;
}

console.log(minus(10));
// → -10
console.log(minus(10, 5));
// → 5
```

如果在一个参数之后写上 `=` 操作符和一个表达式，这个表达式的值会代替此未提供的参数。

比如，这个版本的 `power` 使其第二个参数可选。如果你没有提供它或者传入 `undefined` 值，它将默认为 2，此时其行为跟 `square` 很像。

```js
function power(base, exponent = 2) {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
}

console.log(power(4));
// → 16
console.log(power(2, 6));
// → 64
```

在[下一章][link_chapter_4#rest_parameters]，我们会看到在函数体中获取所传入的完整参数列表的一种方式。这很有用，因为它使得函数可以接收任意数量的参数。比如，`console.log` 就是这么做的 —— 它会输出传给它的所有值。

```js
console.log("C", "O", 2);
// → C O 2
```

## 闭包

把函数当作值的能力，结合每次函数被调用时局部绑定都会重建的事实，会引发一个有趣的问题。函数调用不再活跃时，它所创建的局部绑定会发生什么？

下面的代码展示了这样的一个例子。它定义了一个函数 `wrapValue`，会创建一个局部绑定。随后，它会返回一个访问并返回这个局部绑定的函数。

```js
function wrapValue(n) {
  let local = n;
  return () => local;
}

let wrap1 = wrapValue(1);
let wrap2 = wrapValue(2);
console.log(wrap1());
// → 1
console.log(wrap2());
// → 2
```

这是允许的，并正如你希望的那样工作 —— 绑定的各个实例依然可以被访问。这个情形很好地说明了局部绑定在每一次调用时都会重建、并且不同的调用不会互相干扰各自的局部绑定的事实。

这个特性 —— 能够引用作用域中局部绑定的特定实例，叫作 *闭包*。一个封装了某些局部绑定的函数叫作 *一个* 闭包。这不仅让你不必再担心绑定的生命周期，而且也让创造性地使用函数成为可能。

稍微调整，我们可以把之前的例子转换为创建与任意数目相乘的函数的一种方式。

```js
function multiplier(factor) {
  return number => number * factor;
}

let twice = multiplier(2);
console.log(twice(5));
// → 10
```

`wrapValue` 例子中显式的 `local` 绑定不是必需的，因为参数本身就是一个局部绑定。

像这样思考程序需要一些练习。一个好的思想模型是 —— 把函数想成既包括函数体中的代码，也包括它们被创建时所处的环境。当调用时，函数体看到的是原始的环境，而不是调用发生时的环境。

在这个例子中，`multiplier` 被调用并创建了一个环境，在这个环境中，`factor` 参数被绑定为 2。它所返回的保存在 `twice` 中的函数值，会记住这个环境。因此，当它被调用时，就会对其参数乘以 2。

## 递归

函数调用自身是完全可以的，只要它不过于如此以至于栈溢出。调用自身的函数叫作 *递归*。递归使得一些函数可以写成不同的形式。比如，`power` 的另一个实现：

```js
function power(base, exponent) {
  if (exponent == 0) {
    return 1;
  } else {
    return base * power(base, exponent - 1);
  }
}

console.log(power(2, 3));
// → 8
```

这跟数学家定义乘方的方式非常接近了，比循环变量更清晰地描述了这个概念。函数多次用更小的指数调用自身，以实现重复相乘。

但是这种实现有一个问题：在典型的`JavaScript`实现中，它比循环的版本差不多慢 3 倍。运行一个简单的循环通常要比多次调用一个函数更加合算。

速度与简洁之间的困境非常有意思。你可以把它看成一种对人类友好和对机器友好之间的统一体。几乎任何程序都能通过更大、更复杂的方式使其更快。程序员必须适当权衡以作决定。

在 `power` 函数的情况中，不太简洁的版本(循环)依然非常简单、易读。将其替换为递归版本并不那么合理。通常，为了使程序更直观而放弃一部分效率，是很有帮助的。

过分担心效率也可能造成干扰。这是使程序设计变得复杂的另一个因素，而且当你在做一些本来就很困难的事情时，要担心的多余的事情会使人麻痹。

因此，总是以写出正确且易于理解的程序开始。如果你担心它太慢 —— 其实通常并没有，因为大部分代码不会太频繁执行而消耗大量时间 —— 你可以事后评估，并按需优化。

递归不总是一种相对于循环来说低效的选择。一些问题用递归比循环要简单的多。大多数时候，这些问题需要探索或者处理多个“分支”，每个分支可能继续扩展成更多的分支。

考虑如果难题：从数字 1 开始，反复进行加 5 或者乘以 3，这样可以生成无限多的新数字。如何写出一个程序：对于给定数字，试着找到能生成该数字的这样的加法和乘法的序列。

例如，数字 13 可以通过先乘以 3 再两次加 5 来获得，而数字 15 则拥有无法达到。

这里是一个用递归的解法：

```js
function findSolution(target) {
  function find(current, history) {
    if (current == target) {
      return history;
    } else if (current > target) {
      return null;
    } else {
      return find(current + 5, `(${history} + 5)`) ||
             find(current * 3, `(${history} * 3)`);
    }
  }
  return find(1, "1");
}

console.log(findSolution(24));
// → (((1 * 3) + 5) * 3)
```

注意，这个程序并不一定找到了操作的 *最短* 序列。找到任意序列即可。

现在看不明白程序如何工作也没关系。我们来仔细分析它，因为递归式思想需要专门练习。

内层的 `find` 函数进行实际的递归。它有 2 个参数：当前数字和一个记录如何到达这个数字的字符串。如果它找到一个解法，就返回一个字符串来显示如何到达目标的。如果找不到解法，就返回 `null`。

为此，该函数执行 3 种操作之一。如果当前数字就是目标数字，则当前历史就是一种能到达目标的方式，因此它会被返回。如果当前数字比目标大，进一步探索这个分支是没有意义的，因为加法和乘法都只会让数字更大，因为会返回 `null`。最后，如果仍然小于目标数字，函数会从当前数字开始调用自身两次来尝试可能的路径，一次是加法，另一次是乘法。如果第一次调用返回值不为 `null`，它就被返回。否则，第二次调用将被返回，不管它是生成字符串还是 `null`。

为了更好地理解这个函数如何产生了我们追寻的效果，我们来看看在搜索关于数字 13 的解法时对 `find` 的所有调用。

```js
find(1, "1")
  find(6, "(1 + 5)")
    find(11, "((1 + 5) + 5)")
      find(16, "(((1 + 5) + 5) + 5)")
        too big
      find(33, "(((1 + 5) + 5) * 3)")
        too big
    find(18, "((1 + 5) * 3)")
      too big
  find(3, "(1 * 3)")
    find(8, "((1 * 3) + 5)")
      find(13, "(((1 * 3) + 5) + 5)")
        found!
```

缩进指示了调用栈的深度。第一次 `find` 被调用时，它调用自身以 `(1 + 5)` 开始来探寻解法。这个调用会进一步递归来探索 *每一个* 后续生成小于或等于目标数字的解法。因为它没有找到命中目标的加法，会返回 `null` 给第一次调用。此时，`||` 操作符会导致探索 `(1 * 3)` 的调用发生。这个搜索运气更好 —— 它的第一次递归调用，通过 *又一次* 的递归调用，命中了目标数字。最内侧的那一次调用返回一个字符串，中间调用的 `||` 操作符会把这个字符串传递下去，最终返回了那个解决方案。

## 增长的函数

有两种几乎是自然的引入函数到程序中的方式。

第一种是：你发现正在重复书写类似的代码。我们倾向于不这么干。更多的代码，意味着更多的错误隐藏的空间，以及尝试理解程序的人要阅读的更多的材料。所以，我们收集重复的功能，为其找到一个好名字，并把它放到一个函数中。

第二种是：你发现你需要某些还没写的功能，而听上去这个功能自身理应成为一个函数。你会从函数命名开始，然后会写函数体。甚至可能在定义函数本身之前，你已经开始写使用这个函数的代码了。

为函数找到一个好名字有多困难，很好地反映了你正尝试封装的概念有多清晰。我们来看一个例子。

我们想写一个程序来打印两个数字(农场上奶牛和小鸡的数量)，并在数字后面加上 `Cows` 和 `Chickens` 字样，每个数字都用 0 来填充使得它们总是 3 个数字的长度。

```js
007 Cows
011 Chickens
```

这需要一个有 2 个参数(奶牛的数量和小鸡的数量)的函数。我们开始编码吧。

```js
function printFarmInventory(cows, chickens) {
  let cowString = String(cows);
  while (cowString.length < 3) {
    cowString = "0" + cowString;
  }
  console.log(`${cowString} Cows`);
  let chickenString = String(chickens);
  while (chickenString.length < 3) {
    chickenString = "0" + chickenString;
  }
  console.log(`${chickenString} Chickens`);
}
printFarmInventory(7, 11);
```

字符串表达式后写上 `.length` 将告诉我们此字符串的长度。因此，`while` 循环持续在数字字符串之前加 0，直到字符长度为 3。

任务完成了！但是当我们准备把代码(以及一张大发票)发给农场主时，她打电话过来说她已经开始养猪了，问能不能扩展软件，把猪也打印出来？

我们当然可以。但是正当我们再一次复制、黏贴那 4 行代码时，我们停下来仔细想了想。必须有一种更好的方法。这里是第一次尝试：

```js
function printZeroPaddedWithLabel(number, label) {
  let numberString = String(number);
  while (numberString.length < 3) {
    numberString = "0" + numberString;
  }
  console.log(`${numberString} ${label}`);
}

function printFarmInventory(cows, chickens, pigs) {
  printZeroPaddedWithLabel(cows, "Cows");
  printZeroPaddedWithLabel(chickens, "Chickens");
  printZeroPaddedWithLabel(pigs, "Pigs");
}

printFarmInventory(7, 11, 3);
```

搞定！但是这个名称，`printZeroPaddedWithLabel`，有点令人尴尬。它把 3 件事混在了一起 —— 打印，填充 0，以及添加标签 —— 都放在了同一个函数中。

不再大规模地提取出程序中重复的部分了，我们来凸显单一的 *概念*。

```js
function zeroPad(number, width) {
  let string = String(number);
  while (string.length < width) {
    string = "0" + string;
  }
  return string;
}

function printFarmInventory(cows, chickens, pigs) {
  console.log(`${zeroPad(cows, 3)} Cows`);
  console.log(`${zeroPad(chickens, 3)} Chickens`);
  console.log(`${zeroPad(pigs, 3)} Pigs`);
}

printFarmInventory(7, 16, 3);
```

一个有着像 `zeroPad` 这样优雅简洁名称的函数，使得阅读代码的人更容易搞清楚程序都做了什么。而且这样的一个函数在更多情况下(不仅是这个特定的程序)都很有用。比如，你可以用它来辅助打印精美排列的数字表格。

我们的函数 *应该* 多智能和通用？我们可以写任何东西，从一个只能填充数字为 3 个字符宽度的超简单函数，到一个能处理小数、负数、小数点的排列、用不同字符填充等等的复杂的、普适的数字格式化系统。

一个有用的原则是：不要过度添加，除非你确信你会用到它。对于你碰到的每一个功能写一个通用的“框架”是很诱人的。不要冲动！你并不会搞定什么实际的工作，而只是在写你从来不会用到的代码。

## 函数与副作用

函数可大致分为利用其副作用的和利用其返回值的。(尽管同时具有副作用和返回值是完全有可能的。)

在农场例子中，第一个辅助函数 `printZeroPaddedWithLabel` 因其副作用而被调用：它会打印一行信息。第二版的 `zeroPad` 因其返回值而被调用。第二种在更多的情景中比第一种有用，这并非偶然。创建值的函数比直接产生副作用的函数更易于以新的方式组合起来。

*纯* 函数是一种特殊的生成值的函数，不仅没有副作用，而且也不依赖于来自其它代码的副作用 —— 比如，它不会读取值可能变化的全局绑定。纯函数有着优秀的属性 —— 当以同样的参数调用时，它总是生成同样的值(而且不会做其它任何事情)。这种函数的调用，可以用它的返回值来替换掉而不会改变代码的含义。如果不确定一个纯函数是否正常运行，可以通过调用它来测试，然后就知道了：如果在那个上下文中它正常运行，则在任何上下文中都能正常运行。非纯函数的测试则一般需要更多准备工作。

不过，也没必要因写了非纯函数而感觉糟糕，或者大动干戈地从代码里清除掉它们。副作用经常很有用。比如，没办法写出一个纯函数版的 `console.log`，而 `console.log` 本身非常好用。一些操作在使用副作用时更易于以有效的方式来表达，所以，计算速度也是一个使用非纯函数的理由。

## 总结

本章教了你如何写出自己的函数。`function` 关键字，在作为表达式使用时，会创建一个函数值。当作为语句使用时，可以用来声明绑定并将一个函数作为值赋给它。箭头函数是创建函数的另一个方式。

```js
// 定义 f 持有一个函数值
const f = function(a) {
  console.log(a + 2);
};

// 声明 g 为一个函数
function g(a, b) {
  return a * b * 3.5;
}

// 简明的函数值
let h = a => a % 3;
```

理解函数的关键方面，是理解作用域。每个块都会创建一个新的作用域。在一个特定的作用域中声明的参数和绑定是局部的，对外部不可见。用 `var` 声明的绑定行为就不一样了 —— 它们会出现在最近的函数作用域或者全局作用域中。

把程序要执行的任务分解为不同的函数非常有用。不需要过多重复，函数会把代码组成能执行指定任务的片段，以此协助组织程序。

[chapter_picture_3]: ../assets/chapter_picture_3.jpg
[link_chapter_4#rest_parameters]: ../Part_1_Language/04_data.md#rest_parameters
[link_chapter_5]: ../Part_1_Language/05_higher_order.md
[link_chapter_6]: ../Part_1_Language/06_object.md
