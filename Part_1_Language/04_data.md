# 第四章、数据结构：对象和数组

![chapter_picture_4][chapter_picture_4]

数值、布尔值和字符串，是构建数据结构的原子。而很多种信息都需要不止一种原子。*对象* 可以把值 —— 包括其他对象 —— 组合在一起，构成更复杂的结构。

目前我们创建的程序都限于只能对简单的数据类型进行操作。本章将介绍基本的数据结构。本章结束后，你会有足够的知识开始写有用的程序。

本章将完成一个几乎是实际的编程例子，在适当的时候引入相应概念。示例代码在前文中引入的函数和绑定的基础上构建。

## 数据集

为了处理数码数据块，我们首先得找到在机器内存中表示它们的方法。例如，我们想表示由数组 2、3、5、7和 11 组成的集合。

我们可以从字符串受到启发 —— 毕竟，字符串可以有任意长度，因此可以把大量数据塞给它们 —— 用 `"2 3 5 7 11"` 来表示。但这很不方便。你必须以某种方式提取出数码并将其转换回数字。

幸运的是，`JavaScript`提供了一种专门存储值序列的数据类型。它叫作 *数组*，写成包含在方括号中以逗号隔开的值的列表。

```js
let listOfNumbers = [2, 3, 5, 7, 11];
console.log(listOfNumbers[2]);
// → 5
console.log(listOfNumbers[0]);
// → 2
console.log(listOfNumbers[2 - 1]);
// → 3
```

获取数组中元素的记法，用的也是方括号。表达式后紧跟着一对方括号(其中是另一个表达式)，将会在左边表达式中查询对应于括号中表达式所给定的 *索引* 的元素。

数组的第一个索引是 0，而不是 1。因此，第一个元素用 `listOfNumbers[0]` 获取。基于 0 的计数在技术领域有着悠久的传统，在一定情况下很有道理，但是需要多多习惯。可以把索引想成：从数组开头数起，要略过的项的数量。

## 属性

在过去的章节中，我们看到过看似可疑的表达式，如 `myString.length` (获取字符串的长度) 和 `Math.max` (最大值函数)。这些是访问某些值 *属性* 的表达式。在第一种情况里，我们访问了值 `myString` 的 `length` 属性。在第二个中，我们访问了 `Math` 对象(一个数学相关的常量和函数的集合)的 `max` 方法。

几乎所有的`JavaScript`值都有属性。`null` 和 `undefined` 例外。如果你试着访问其中之一的属性，将报错。

```js
null.length;
// → TypeError: null has no properties
```

`JavaScript`中访问属性的 2 种典型方法是用点号和用方括号。`value.x` 和 `value[x]` 都会访问 `value` 的属性 —— 但不一定是同一个属性。区别在于 `x` 是如何解析的。当用点号时，点号后面的单词就是属性的字面名称。当用方括号时，括号中的表达式被求值以获得属性名称。`value.x` 获取的是 `value` 的名为 “x” 的属性，而 `value[x]` 会对 `x` 求值，并把这个结果作为属性名。

因此，如果你知道所感兴趣的属性叫做 *color*，就用 `value.color`。如果你想提取的属性，其名称由绑定 `i` 持有，则用 `value[i]`。属性名是字符串。可以是任意字符串，不过点号记法只适用于可作为合法绑定名称的属性名。所以，如果你想访问名为 *2* 或者 *John Doe* 的属性，就必须使用方括号：`value[2]` 或 `value["John Doe"]`。

数组中的元素作为数组的属性而存储，使用数字作为属性名。因为不能对数字使用点号记法，而且通常会用一个绑定来持有索引，你得使用方括号来获取它们。

数组的 `length` 属性告诉我们它有多少个元素。这个属性名是一个合法的绑定名称，并且我们事先知道它的名称，因此，要找到数组的长度，一般会用 `array.length`，因为这比写 `array["length"]` 更容易。

## 方法

除了 `length` 属性，字符串和数组对象都含有很多持有函数值的属性。

```js
let doh = "Doh";
console.log(typeof doh.toUpperCase);
// → function
console.log(doh.toUpperCase());
// → DOH
```

每个字符串都有 `toUpperCase` 属性。当调用时，它会返回该字符串所有字母都被转化为大写的一个拷贝。也有 `toLowerCase`，作用相反。

有趣的是，即使对 `toUpperCase` 的调用没有传递任何参数，该函数也能访问字符串 `"Doh"`，而我们调用的是它的属性。关于这一点的来龙去脉将在[第六章][link_chapter_6]描述。

包含函数值的属性通常被叫作它们所属值的 *方法*。在这里，“`toUpperCase` 是字符串的一个方法”。

这个例子演示了 2 个可以用来操作数组的方法：

```js
let sequence = [1, 2, 3];
sequence.push(4);
sequence.push(5);
console.log(sequence);
// → [1, 2, 3, 4, 5]
console.log(sequence.pop());
// → 5
console.log(sequence);
// → [1, 2, 3, 4]
```

`push` 方法向数组末端添加值，而 `pop` 方法则相反：删除数组中最后一个值并返回它。

这些名称是描述 *栈* 上的操作所用的传统术语。在编程中，栈是一种数据结构，允许你向其中压入值，再以相反的顺序弹出值，因此最后添加的内容会被首先移除。这些在编程中很普遍 —— 你可能还记得[上一章][link_chapter_3]中的函数调用栈，就是同样概念的一个实例。

## 对象

回到 weresquirrel 的故事。一系列的日志记录可以用一个数组来表示。但是记录不会只由一个数字或字符串组成 —— 每条记录需要存储一个活动列表和一个标识雅克是否变成了松鼠的布尔值。理想情况下，我们希望把这些信息组合到一个值里，然后把组合值放入日志记录的数组里。

*对象* 类型的值是任意属性的集合。创建对象的一种方法是用大括号作为表达式。

```js
let day1 = {
  squirrel: false,
  events: ["work", "touched tree", "pizza", "running"]
};
console.log(day1.squirrel);
// → false
console.log(day1.wolf);
// → undefined
day1.wolf = false;
console.log(day1.wolf);
// → false
```

在大括号中，有逗号隔开的属性列表。每个属性都有一个名称，随后是冒号和值。当对象写成多行时，像例子中那么进行缩进有助于提高可读性。属性名不是合法的绑定名或数字时，必须用引号引起来。

```js
let descriptions = {
  work: "Went to work",
  "touched tree": "Touched a tree"
};
```

这意味着大括号在`JavaScript`中有 *2* 种含义。在语句开头时，它们会开启一个语句块。在任何其他位置时，它们描述对象。幸运的是，以对象开头来开始一条语句几乎没什么用，所以二者之间的这个不明确之处并不是什么大问题。

读取不存在的属性会给你 `undefined` 值。

也可以用 `=` 操作符对属性表达式进行赋值。如果属性存在，则替换掉它的值，否则就给这个对象创建一个新的属性。

回到绑定的触须模型 —— 属性绑定也是类似的。它们 *抓住* 了值，但是其他绑定和属性也可能持有那些相同的值。可以把对象想成有任意多触须的章鱼，每一条触须都对应一个名称。

`delete` 操作符会砍掉这样一条章鱼的一根触须。它是一个一元操作符，当应用于一个属性访问表达式时，将从对象中删除对应名称的属性。这个操作不常用，但是是可能的。

```js
let anObject = {left: 1, right: 2};
console.log(anObject.left);
// → 1
delete anObject.left;
console.log(anObject.left);
// → undefined
console.log("left" in anObject);
// → false
console.log("right" in anObject);
// → true
```

二元操作符 `in` 作用于一个字符串和一个对象时，会告诉你这个对象是否有此属性。把属性设置为 `undefined` 和真正把它删掉的区别在于：第一种情况下，那个对象仍然 *拥有* 此属性(只是它的值没什么意义)，而第二种情况中，该属性就不复存在了，`in` 将返回 `false`。

要搞清楚一个对象拥有哪些属性，可以使用 `Object.keys` 函数。传给它一个对象，它会返回一个字符串数组 —— 该对象的属性名称。

```js
console.log(Object.keys({x: 0, y: 0, z: 2}));
// → ["x", "y", "z"]
```

有一个 `Object.assign` 函数，用来把一个对象的所有属性复制到另一个对象。

```js
let objectA = {a: 1, b: 2};
Object.assign(objectA, {b: 3, c: 4});
console.log(objectA);
// → {a: 1, b: 3, c: 4}
```

数组，是对象的一种特例，用来存储序列值。如果你对 `typeof []` 求值，会生成 `"object"`。你可以把它们看作长长的、扁平的章鱼：其所有的触手整齐排列，并以数字进行标注。

我们把雅克的日志表示为一个由对象所组成的数组。

```js
let journal = [
  {events: ["work", "touched tree", "pizza",
            "running", "television"],
   squirrel: false},
  {events: ["work", "ice cream", "cauliflower",
            "lasagna", "touched tree", "brushed teeth"],
   squirrel: false},
  {events: ["weekend", "cycling", "break", "peanuts",
            "beer"],
   squirrel: true},
  /* and so on... */
];
```

## 可变性

我们 *很快* 将开始真正的编程。首先，需要理解另外一条理论。

我们看到了：对象值可以被修改。前面章节中讨论的值类型，比如：数字、字符串和布尔值，都是 *不可变的* —— 不能更改这些类型的值。你可以组合它们、从中派生出新的值，但一旦你设置了一个指定的字符串值，这个值将永远保持不变。其中的文本是不会变的。假如你有一个包含 `"cat"` 的字符串，不可能有其他代码来更改这个字符串中的字符，使其拼写为 `"rat"`。

一个对象值的内容 *可以* 以改变其属性的方式被修改。

对于两个数字 120 和 120，我们可以认为它们精确地相等，不管它们是否指的是同样的比特。对于对象来说，相同对象的两个引用和含有相同属性的两个不同对象之间存在着差异。考虑如下代码：

```js
let object1 = {value: 10};
let object2 = object1;
let object3 = {value: 10};

console.log(object1 == object2);
// → true
console.log(object1 == object3);
// → false

object1.value = 15;
console.log(object2.value);
// → 15
console.log(object3.value);
// → 10
```

`object1` 和 `object2` 绑定抓取了 *相同的* 对象，这就是为什么改变 `object1` 也会改变 `object2` 的值。`object3` 指向了一个不同的对象，初始时含有与 `object1` 相同的属性但是相互独立。

绑定也可以是可变的或是不变的，但是这得跟它们的值的行为方式分开。即使数字值不能改变，你仍然可以用 `let` 绑定通过改变它所指向的值来追踪一个变化的数字。同样地，尽管一个对于对象的 `const` 绑定本身不能改变，会持续指向同样的对象，但是该对象的 *内容* 可能会改变。

```js
const score = {visitors: 0, home: 0};
// This is okay
score.visitors = 1;
// This isn't allowed
score = {visitors: 1, home: 1};
```

当你用`JavaScript`的 `==` 操作符来比较对象时，只有当两个对象精确地相同时才会返回 `true`。比较不同的对象将返回 `false`，即使他们有着完全一样的属性。`JavaScript`没有内置“深度”比较的操作：通过内容来比较对象，但是你可以自己写一个（这将是本章末尾的[习题][link_chapter_4#exercise_deep_compare]之一）。

## 狼人日志

雅克开启了他的`JavaScript`解析器，并设置了保存其日志所需的环境。

```js
let journal = [];

function addEntry(events, squirrel) {
  journal.push({events, squirrel});
}
```

注意：添加到日志中的对象看起来有点奇怪。它没有像 `events: events` 这样来声明属性，而是只给出了属性名。这是相同事情的一个快捷方式 —— 在大括号记法中，如果属性名后没有值，那么它的值将从同名的绑定中获取。

因此，每天晚上 10 点 —— 或第二天早晨的某个时候，在从书架的顶层爬下来之后 —— 雅克记录了这一天。

```js
addEntry(["work", "touched tree", "pizza", "running",
          "television"], false);
addEntry(["work", "ice cream", "cauliflower", "lasagna",
          "touched tree", "brushed teeth"], false);
addEntry(["weekend", "cycling", "break", "peanuts",
          "beer"], true);
```

一旦他有了足够的数据点，他就想用统计学来弄清楚哪些事项跟松鼠事件可能相关。

*相关性* 是统计变量之间依赖性的一种测量。统计变量不完全跟程序变量相同。在统计学中，通常会有一系列 *量度*，每个变量会针对每种量度进行测量。变量之间的相关性通常以 -1 到 1 范围内的一个值来表示。0 相关性意味着变量之间不相关。相关性为 1 表示两个变量完全相关 —— 只有你知道一个，你也就知道了另一个。-1 表示变量完全相关，但是方向相反 —— 当一个为真时，另一个为假。

要计算两个布尔变量之间相关性的量度，我们可以用 *Phi参数*(*ϕ*)。这个公式的输入是一个频率表格，它包含了变量的不同组合被观察到的次数。该公式的输出是描述相关性的 -1 到 1 之间的一个数字。

我们选取吃披萨这个事件，并把它如下放入一个频率表格中，其中每个数字表示当前组合在我们的测量中发生的次数：

![pizza-squirrel][pizza-squirrel]

如果把表格称作 *n*，我们可以用如下公式来计算 *ϕ*：

<img src="http://latex.codecogs.com/svg.latex?\phi&space;=&space;\frac{n_{11}n_{00}&space;-&space;n_{10}n_{01}}{\sqrt{n_{1·}n_{0·}n_{·1}n_{·0}}}" title="\phi = \frac{n_{11}n_{00} - n_{10}n_{01}}{\sqrt{n_{1·}n_{0·}n_{·1}n_{·0}}}" />

[chapter_picture_4]: ../assets/chapter_picture_4.jpg
[pizza-squirrel]: ../assets/pizza-squirrel.svg
[link_chapter_3]: ../Part_1_Language/03_functions.md
[link_chapter_4#exercise_deep_compare]: ../Part_1_Language/04_data.md#exercise_deep_compare
[link_chapter_6]: ../Part_1_Language/06_object.md
