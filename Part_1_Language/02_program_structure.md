# 第二章、程序结构

![chapter_picture_2][chapter_picture_2]

本章开始，我们会进行真正的 *编程*。我们将加强对`JavaScript`语言的掌握，从目前我们所看到的词句碎片，直到我们可以写出有意义的散文。

## 表达式和语句

在[第一章][link_chapter_1]中，我们创造了值，并对其应用操作符以得到新的值。像这样创建值是任何`JavaScript`程序的基础。但是这个东西必须在一个更大的结构中才有用。这就是我们下一步要做的。

生成值的代码片段叫作 *表达式*。每一个字面量的值(比如 `22` 或 `"psychoanalysis"`)都是一个表达式。括号中的表达式也是一个表达式，对于二元操作符作用于 2 个表达式或一元操作符作用于 1 个的情况也是一样。

这展示了基于语言的接口的部分优点。表达式可以包含其他表达式，方式非常类似于人类语言中子句的嵌套：一个子句可以包含其自己的子句，以此类推。如此，我们就可以构建能描述任意复杂计算的表达式。

如果表达式对应句子片段，那么`JavaScript` *语句* 则对应于完整的句子。程序是一系列的语句。

最简单的语句，就是表达式后面接分号。这里是一个程序：

```js
1;
!false;
```

不过，这个程序没什么用。一个表达式的内容可以是仅仅生成一个值，这个值又被范围内的代码所用。一条语句是独立的，当它对这个世界有影响时才会起作用。它可以在屏幕上显示内容 —— 这就是所谓的“改变了世界” —— 或者它可以改变机器的内部状态，这会影响后续的语句。这些改变叫作 *副作用*。上述示例中的语句只是生成了值 `1` 和 `true`，随后立即扔掉这些值。这对世界没有任何影响。运行此程序，观察不到任何改变。

在某些情况下，`JavaScript`运行你省略语句后的分号。在其他情况下，则必须写上分号，否则下一行会被当作当前语句的一部分。何时可以安全地省略分号的规则稍微复杂且易错。所以，本书中每一条语句都会包含分号。我建议你也这么做，至少在你学习到更多关于省略分号的细微技巧之前。

## 绑定

一个程序如何持有内部状态？它怎么记录事物？我们已经看过如何从旧值生成新值，但是这并没有改变旧值，而且新值必须立即用掉(否则就又会消失)。为了获取并持有值，`JavaScript`提供了 *绑定* 或 *变量*。

```js
let caught = 5 * 5;
```

这是第二种语句。特殊字(*关键字*) `let` 表示本句将定义一个绑定。其后接着是绑定的名称，如果需要立即给它一个值，则再接上 `=` 操作符和一个表达式。

前面那条语句创建了一个叫 `caught` 的绑定，用它来获取并持有由 5 乘以 5 所生成的数字。

一个绑定定义之后，它的名称就可以作为表达式使用了。这样的表达式的值就是这个绑定当前所持有的值。例如：

```js
let ten = 10;
console.log(ten * ten);
// → 100
```

当一个绑定指向一个值时，并不意味着它和那个值永远捆绑在一起了。可以在任何时候用 `=` 操作符把它们与当前值解绑并使其指向一个新值。

```js
let mood = "light";
console.log(mood);
// → light
mood = "dark";
console.log(mood);
// → dark
```

你应该把绑定想象成触须，而不是盒子。它们不 *包含* 值，它们只是 *抓取* 值 —— 两个绑定可以指向相同的值。程序只能读取它仍然持有的值。当你需要记住某个事物，你可以生长一条触须来抓住它，或者让现存的一条触须重新附着给它。

我们来看另一个例子。为了记住 Luigi 欠你的美元数，你创建了一个绑定。当他还了 35$ 时，你就赋予这个绑定一个新值。

```js
let luigisDebt = 140;
luigisDebt = luigisDebt - 35;
console.log(luigisDebt);
// → 105
```

当定义了一个绑定而又不给它值时，触须没什么可抓，就会飘荡在空中。如果你请求一个空绑定的值，将会得到 `undefined`。

一个 `let` 可以定义多个绑定，多个定义必须以逗号隔开。

```js
let one = 1, two = 2;
console.log(one + two);
// → 3
```

`var` 和 `const` 也可以用来创建绑定，方式与 `let` 相同。

```js
var name = "Ayda";
const greeting = "Hello ";
console.log(greeting + name);
// → Hello Ayda
```

第一个，`var` (“variable”，变量的缩写)，是 2015 年之前的`JavaScript`定义绑定的方式。在[下一章][link_chapter_3]我们会继续介绍它与 `let` 的不同。目前来说，记住它几乎是在做同样的事情，不过本书中我们很少用它，因为它有一些令人困惑的特性。

关键字 `const` 代表 *constant* (常量)。它定义了一个常量绑定，不能再指向一个新值。这对于如下情况的绑定很有用：把值赋予名称，以方便后续引用它们，但这些值永远不会改变。

## 绑定名称

绑定名称可以是任意字词，保留为其他用途的(比如：`let`)除外。数字可以作为绑定名称的一部分。比如，`catch22` 是一个合法的名称，但是名称不能以数字开头。绑定名称可以包含美元符号(`$`)或下划线(`_`)，但不能是其他标点或特殊字符。

具有特殊意义的字词，比如 `const`，叫作 *关键字*，它们不能被用作绑定名称。也有非常多的字词被“保留”，以在`JavaScript`的未来版本中使用，这些字词也不能被用作绑定名称。关键字和保留字的完整列表相当长。

```js
break case catch class const continue debugger default
delete do else enum export extends false finally for
function if implements import interface in instanceof let
new package private protected public return static super
switch this throw true try typeof var void while with yield
```

别担心记不住这些。当创建绑定出现意外的语法错误时，看看你是否在尝试定义一个保留字。

## 环境

特定时刻，绑定及其值的集合，叫作 *环境*。当程序启动时，这个环境不为空。它总是包含语言标准的部分绑定，大多数时候，它还有一些绑定来提供与外界系统的交互。比如，在浏览器中，有函数可以与当前加载的网站进行交互，并获取鼠标和键盘的事件。

## 函数

默认环境提供的很多值，类型都是 *函数*。一个函数是封装在一个值中的程序片段。这样的值可以被 *应用* 以运行所封装的程序。例如，在浏览器环境中，`prompt` 绑定持有一个函数：显示一个对话框，请求用户输入。像这样使用它：

```js
prompt("Enter passcode");
```

![prompt][prompt_img]

执行一个函数，叫作 *函数调用* 或 *函数应用*。你可以在生成一个函数值的表达式后加上括号来调用一个函数。通常，会直接使用持有函数的绑定名称。括号中的值被传入函数中。在上述例子中，`prompt` 函数用我们传给它的字符串作为对话框中显示的文字。传给函数的值叫作 *参数*。不同的函数可能需要不同数量或不同类型的参数。

在现代 Web 编程中不太会用到 `prompt` 函数，主要是无法控制对话框的显示样式，不过对于玩具程序和实验很有帮助。

## `console.log` 函数

在例子中，我用了 `console.log` 来输出值。大多数`JavaScript`系统(包括所有现代 Web 浏览器和 `Node.js`)都提供一个 `console.log` 函数来把它的参数写到某一文字输出设备。在浏览器中，这个输出是在`JavaScript` 控制台中。这部分浏览器界面默认是隐藏的，不过大部分浏览器可以通过按 **F12**，或者 Mac 中按下 **Command-Option-I** 来打开它。如果还不行，就在菜单中找 “开发者工具” 或者类似的一项。

尽管绑定名称不能包含句号 (`.`)，可`console.log` 中确实有一个。这是因为 `console.log` 不是一个简单的绑定。它实际上是一个表达式：从 `console` 绑定所持有的值中获取 `log` 属性。我们将在[第四章][link_chapter_4]中弄清楚这是什么意思。

## 返回值

显示一个对话框或把文字写到屏幕上，都是 *副作用*。很多函数因其产生的副作用而非常有用。函数也可以生成值，这种情况下不需要产生副作用也很有用。例如，`Math.max` 函数以任意数目的数字为参数并给出其中的最大值。

```js
console.log(Math.max(2, 4));
// → 4
```

当一个函数生成一个值，我们称之为 *返回* 了那个值。`JavaScript`中任何生成一个值的内容都是一个表达式，这意味着函数调用可以用于更大的表达式中。这里是 `Math.min` (与 `Math.max` 作用相反) 作为加法表达式一部分的一个调用：

```js
console.log(Math.min(2, 4) + 100);
// → 102
```

[下一章][link_chapter_3]解释如何编写自己的函数。

## 控制流

当你的程序不止一条语句时，语句会像故事一样从上到下执行。下面这个示例程序有 2 条语句。第一条让用户输入一个数字，第二条随后执行，会显示那个数字的平方。

```js
let theNumber = Number(prompt("Pick a number"));
console.log("Your number is the square root of " +
            theNumber * theNumber);
```

`Number` 函数会把一个值转换为数字。我们需要这个转换，是因为 `prompt` 的结果是字符串，而我们需要一个数字。类似的函数还有 `String` 和 `Boolean`，会分别把值转换成相应的类型。

直线型控制流的大致示意图如下：

![straight-line control flow][controlflow-straight]

## 条件执行

不是所有程序都是“大直道”。举例来说，我们也许想要创建一条岔路：程序根据条件来选择合适的分支。这叫作 *条件执行*。

![if control flow][controlflow-if]

`JavaScript`中使用 `if` 关键字来创建条件执行。简单情况下，我们希望一些代码当且仅当一个特定条件成立时执行。比如，我们希望仅在输入确实是一个数字时再显示它的平方。

```js
let theNumber = Number(prompt("Pick a number"));
if (!isNaN(theNumber)) {
  console.log("Your number is the square root of " +
              theNumber * theNumber);
}
```

做了这个修改之后，如果你输入 “parrat”，就不会显示任何内容。

`if` 关键字根据布尔表达式的值来执行或略过一条语句。这个判断表达式写在关键字后面的括号中，随后是要执行的语句。

`isNaN` 是`JavaScript`的一个标准函数，仅当传给它的参数是 `NaN` 时返回 `true`。`Number` 函数刚好在传入一个不能代表合法数字的字符串时返回 `NaN`。如此一来，那个条件的含义是：除非 `theNumber` 不是数字，否则就执行如下语句。

本例中，`if` 下面的语句被封装进大括号(`{`和`}`)中。它们可以把任意数量的语句组合成一条单一的语句，称为 *块*。这个情况下可以省略掉大括号，因为其中只包含一条语句。为了避免思考是否需要它们，大多数`JavaScript`程序员在每一个封装语句中都会用到它们。本书中我们也主要遵循这个惯例，偶尔单行的情况除外。

```js
if (1 + 1 == 2) console.log("It's true");
// → It's true
```

通常，你不仅有当条件为真时执行的代码，还有处理其他情况的代码。这条可选的路径由图中第二个箭头表示。`else` 关键字可以和 `if` 一起来创建两条独立、可选的执行路径。

```js
let theNumber = Number(prompt("Pick a number"));
if (!isNaN(theNumber)) {
  console.log("Your number is the square root of " +
              theNumber * theNumber);
} else {
  console.log("Hey. Why didn't you give me a number?");
}
```

如果有超过 2 条路径可选，可以把多个 `if`/`else` 对“串”在一起。比如：

```js
let num = Number(prompt("Pick a number"));

if (num < 10) {
  console.log("Small");
} else if (num < 100) {
  console.log("Medium");
} else {
  console.log("Large");
}
```

这个程序首先检查 `num` 是否小于 10。如果是，则选择那条路径，显示 `"Small"` 并完成。如果不是，则选择 `else` 分支，这个分支又包含了第二个 `if`。如果第二个条件(`<` 100)成立，就说明那个数字介于 10 和 100 之间，则会显示 `"Medium"`。如果不成立，那么选择第二个也即最后一个 `else` 分支。

这个程序的模式大致如下：

![nested-if control flow][controlflow-nested-if]

## `while` 和 `do` 循环

考虑一个输入从 0 到 12 之间所有偶数的程序。一种方式可以如下书写：

```js
console.log(0);
console.log(2);
console.log(4);
console.log(6);
console.log(8);
console.log(10);
console.log(12);
```

这可以达到目的，但写程序是为了用 *更少* 的内容来搞定，而不是更多。假如我们需要 1000 以内的所有偶数，这种方法是不切实际的。我们需要的是一种能重复某些代码的方法。这种控制流叫作 *循环*。

![loop control flow][controlflow-loop]

循环控制流使得我们可以回到程序中曾经到达的某一点，并以当前程序状态再进行重复。如果我们把这个机制与一个计数的绑定相结合，可以这样写：

```js
let number = 0;
while (number <= 12) {
  console.log(number);
  number = number + 2;
}
// → 0
// → 2
//   … etcetera
```

以关键字 `while` 开头的语句会创建一个循环。关键字 `while` 之后是包在括号中的表达式，随后是一条语句，跟 `if` 很像。只要表达式的值在转换成布尔值时为 `true`，这个循环就会一直进入那条语句。

`number` 绑定展示了一个绑定用来追踪程序进程的方式。每一次循环重复时，`number` 的值就会在之前值的基础上加 2。每一次重复开始，都会将其与数字 12 相比，以判断程序的工作是否已完成。

作为真实有用的一个例子，现在我们写一个程序来计算并显示 2<sup>10</sup> (2 的 10 次方)。我们用了 2 个绑定：一个追踪结果，一个记录对结果乘以 2 的次数。这个循环会检测第二个绑定是否已达到 10，如果没有，就相应更新两个绑定。

```js
let result = 1;
let counter = 0;
while (counter < 10) {
  result = result * 2;
  counter = counter + 1;
}
console.log(result);
// → 1024
```

计数器也可以从 `1` 开始然后检查 `<= 10`，但是，由于一些原因([第四章][link_chapter_4#array_indexing]会解释)，最好是习惯于从 0 开始计数。

`do` 循环是一个与 `while` 循环相似的控制结构。它们只有一点不同：`do` 循环总会执行循环体至少一次，并且在第一次执行之后检测是否应该停止。为了反映这一点，检测出现在循环体之后：

```js
let yourName;
do {
  yourName = prompt("Who are you?");
} while (!yourName);
console.log(yourName);
```

这个程序会让你输入一个名字。它会一遍又一遍地询问，直到获取的值不为空字符串。使用 `!` 操作符，会在对值取反之前将其转换为布尔值，除 `""` 之外的所有字符串都转换为 `true`。这意味着该循环会一直运行，直到你提供了一个非空的名称。

## 代码缩进

在之前的例子中，我在语句之前添加了空格，作为更大语句的一部分。这不是必须的：没有它们，计算机也能识别程序。实际上，甚至程序中的换行也是可选的。只要你喜欢，你可以把程序写成长长的一行。

块中的缩进，是为了使代码的结构更加清晰。在有新的块在其他块中开始的代码中，很难看清一个块在哪儿结束、另一个在哪儿开始。通过适当的缩进，程序的可见形状就对应着其中各个块的形状。我喜欢用 2 个空格来标记每个块，不过也可以有不同的“口味” —— 一些人用 4 个空格，而另一些人用制表符。重要的是，每一个新块添加了同样数量的空格。

```js
if (false != true) {
  console.log("That makes sense.");
  if (1 < 2) {
    console.log("No surprise there.");
  }
}
```

大多数代码编辑器会自动对新行进行适当的缩进。

## `for` 循环

很多循环都遵循上面 `while` 例子中的模式。首先，创建一个“计数器”绑定来追踪循环的进程。然后是一个 `while` 循环，其测试表达式通常会检测计数器是否已经到达它最终的值。在循环体最后，更新计数器。

因为这个模式太常见了，`JavaScript`和类似的语言提供了一个略微简短而更全面的形式：`for` 循环。

```js
for (let number = 0; number <= 12; number = number + 2) {
  console.log(number);
}
// → 0
// → 2
// … etcetera
```

这个程序完全等价于之前的打印偶数的例子。唯一的改变在于：所有与循环“状态”相关的语句都被组合在一起，放于 `for` 之后。

`for` 关键字后面的括号必须包含 2 个分号。第一个分号之前的部分 *初始化* 循环，通常会定义一个绑定。第二个部分是 *检测* 循环是否要继续的表达式。最后一部分在每一次迭代之后 *更新* 循环的状态。大多数情况下，这要比 `while` 结构更短更清晰。

这里是计算 2<sup>10</sup> 的代码，使用 `for` 而不是 `while`：

```js
let result = 1;
for (let counter = 0; counter < 10; counter = counter + 1) {
  result = result * 2;
}
console.log(result);
// → 1024
```

## 退出循环

拥有生成 `false` 的循环条件，不是结束循环的唯一方式。有一条特殊的语句 `break`，会立即跳出当前循环。

这个程序展示了 `break` 语句。它会找到大于等于 20 且能被 7 整除的第一个数字。

```js
for (let current = 20;; current = current + 1) {
  if (current % 7 == 0) {
    console.log(current);
    break;
  }
}
// → 21
```

使用求余操作符(`%`)可以轻松地判断一个数字是否能被另一个数整除。如果能，则他们相除的余数为 0。

例子中的 `for` 结构没有检测循环终止的部分。这意味着循环将永远不会停止，除非其中的 `break` 语句被执行。

如果你删掉那条 `break` 语句，或者不小心写了一个总是生成 `true` 的条件，你的程序将陷入 *无限循环*。陷入无限循环的程序将永不停止地运行，通常来说这不是什么好事。

`continue` 关键字跟 `break` 类似，也会影响循环的进程。当在循环体中遇到 `continue`，将会跳出循环体，然后继续循环的下一次迭代。

## 更新绑定的简短方法

程序经常需要基于一个绑定之前的值来“更新”这个绑定，尤其是在循环中。

```js
counter = counter + 1;
```

对此，`JavaScript`提供了一个快捷方式：

```js
counter += 1;
```

类似的快捷方式也适用于其他操作符，比如 `result *= 2` 来使 `result` 翻倍，或者 `counter -= 1` 来倒数。

这使得我们可以把计数的例子再缩短一点。

```js
for (let number = 0; number <= 12; number += 2) {
  console.log(number);
}
```

对于 `counter += 1` 和 `counter -= 1`，甚至还有更短的写法：`counter++` 和 `counter--`。

## 用 `switch` 分发值

经常会有如下的代码：

```js
if (x == "value1") action1();
else if (x == "value2") action2();
else if (x == "value3") action3();
else defaultAction();
```

有一个 `switch` 结构专门用于以一种更直接的方式来表达这样的“分发”。不过，`JavaScript`关于这个的语法(继承自 `C/Java` 编程语言)有些尴尬 —— 串联的 `if` 语句也许看起来更好。这里是一个例子：

```js
switch (prompt("What is the weather like?")) {
  case "rainy":
    console.log("Remember to bring an umbrella.");
    break;
  case "sunny":
    console.log("Dress lightly.");
  case "cloudy":
    console.log("Go outside.");
    break;
  default:
    console.log("Unknown weather type!");
    break;
}
```

你可以在 `switch` 块中放置任意数量的 `case` 标签。程序将从对应于 `switch` 值的标签处开始执行，或者如果没有匹配项时，执行 `default` 标签。它会持续执行，甚至跨越其他标签，直到遇到 `break` 语句。在一些情况下，比如例子中 `"sunny"` 的情况，这可以用来在不同情况间共享某些代码(此程序建议在晴天和多云天气都外出：`"Go outside."`)。但是要小心 —— 很容易忘记写 `break`，这会导致程序执行了你并不想执行的代码。

## 字母大写

绑定名称不能包含空格，但是用多个单词来清晰地描述绑定所代表的含义经常非常有帮助。有相当多的选择用来以多个单词写绑定名称：

```js
fuzzylittleturtle
fuzzy_little_turtle
FuzzyLittleTurtle
fuzzyLittleTurtle
```

第一种方式阅读起来非常困难。我相当喜欢下划线的样式，尽管敲起来会有点痛苦。标准的`JavaScript`函数以及大多数`JavaScript`程序员，都遵循最下面那种样式 —— 除了第一个外，大写每个单词的首字母。习惯这样的小事不是什么难事，而且混用命名方式来编码会造成难以阅读，所以我们也遵循这个惯例。

在一些情况下，比如 `Number` 函数，绑定的首字母也是大写的。这用来标记这个函数为构造器。构造器是什么，将在[第六章][link_chapter_6]弄清楚。目前来说，重要的是不要因这种不一致而感到烦恼。

[chapter_picture_2]: ../assets/chapter_picture_2.jpg
[link_chapter_1]: ../Part_1_Language/01_values.md
[link_chapter_3]: ../Part_1_Language/03_functions.md
[link_chapter_4]: ../Part_1_Language/04_data.md
[link_chapter_4#array_indexing]: ../Part_1_Language/04_data.md#array_indexing
[link_chapter_6]: ../Part_1_Language/06_object.md
[prompt_img]: ../assets/prompt.png
[controlflow-straight]: ../assets/controlflow-straight.svg
[controlflow-if]: ../assets/controlflow-if.svg
[controlflow-nested-if]: ../assets/controlflow-nested-if.svg
[controlflow-loop]: ../assets/controlflow-loop.svg
