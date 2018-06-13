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

如果现在你正把书放下，绞尽脑汁回想十年级数学课 —— 先别急！我不想用无穷无尽难懂的标记来折磨你 —— 目前就只有这一个公式。而且，对于这一个，我们所有要做的就是：把它转为`JavaScript`。

*n*<sub>01</sub> 表示测量中第一个变量(变成松鼠)为假(0)而第二个变量(披萨)为真(1)时的数量。在披萨表格中，*n*<sub>01</sub> 是 9。

*n*<sub>1·</sub> 指的是第一个变量为真的所有测量的和，该示例表格中就是 5。类似地，*n*<sub>·0</sub> 指的是第二个变量为假的所有测量的和。

因此，对于这个披萨表格，分割线以上的部分(被除数)就是 1×76−4×9 = 40，下面的部分(除数)则是 5×85×10×80 的平方根，或者 √340000。最终结果 *ϕ* ≈ 0.069，非常微小。吃披萨看起来对于变形没有多大的影响。

## 计算相关性

在`JavaScript`中，我们可以用一个 4 元素的数组(`[76, 9, 4, 1]`)来表示一个 2&times;2 的表格。也可以用其他的表示法，比如一个包含两个 2 元素数组的数组(`[[76, 9], [4, 1]]`)或一个含有像 `"11"` 和 `"01"` 这样属性名的对象，但那个扁平的数组很简单，并使得获取表格的表达式很简洁。我们将数组的索引解析为两位的二进制数：其中最左边(最重要)的数字指的是松鼠变量，最右边(不重要)的数字表示事件变量。比如，二进制数 `10` 指的是雅克的确变成了一只松鼠、但是事件(比如：“披萨”)没有发生的情况。这发生了 4 次。因为二进制的 `10` 对应十进制的 2，我们将保存这个数字到数组中索引为 2 的位置。

这是一个计算此类数组的 *ϕ* 参数的函数：

```js
function phi(table) {
  return (table[3] * table[0] - table[2] * table[1]) /
    Math.sqrt((table[2] + table[3]) *
              (table[0] + table[1]) *
              (table[1] + table[3]) *
              (table[0] + table[2]));
}

console.log(phi([76, 9, 4, 1]));
// → 0.068599434
```

这是 *ϕ* 公式到`JavaScript`的直译。`Math.sqrt` 是求平方根函数，有`JavaScript`标准环境的 `Math` 对象提供。我们得对表格中两个项相加来得到像 *n*<sub>1·</sub> 这样的项，因为行或列的和在我们的数据结构中并没有直接存储。

雅克保存了 3 个月的日志。最终的数据集可在本章的[编程沙盒][sandbox_4]中获取：它被保存到可下载[文件][download_journal]的 `JOURNAL` 绑定中。

为了从日志中提取出某一特定事件的 2&times;2 表格，我们需要循环所有的条目，并且标记涉及到松鼠变形时、该事件发生了多少次。

```js
function tableFor(event, journal) {
  let table = [0, 0, 0, 0];
  for (let i = 0; i < journal.length; i++) {
    let entry = journal[i], index = 0;
    if (entry.events.includes(event)) index += 1;
    if (entry.squirrel) index += 2;
    table[index] += 1;
  }
  return table;
}

console.log(tableFor("pizza", JOURNAL));
// → [76, 9, 4, 1]
```

数组有一个 `includes` 方法，用来检测一个指定值是否存在于数组中。该函数利用了这一点来确定感兴趣的事件名是否是指定日中事件列表的一部分。

`tableFor` 中的循环体会检查每一条记录是否包含所感兴趣的事件以及此事件是否伴随松鼠事件发生，由此判定每一条日志记录落在表格的哪个方块中。该循环随后在表格的相应方块中加一。

现在我们有了计算独立的相关性的工具。唯一剩下的步骤就是：找到所记录的每一种事件所对应的相关性，并查看是否有些什么线索。

## 数组循环

在 `tableFor` 函数中，有一个这样的循环：

```js
for (let i = 0; i < JOURNAL.length; i++) {
  let entry = JOURNAL[i];
  // Do something with entry
}
```

这种循环在典型的`JavaScript`中很常见 —— 一次一个元素地遍历数组是经常出现的，为此你需要执行一个计数器来遍历数组的长度，并依次从中取出每个元素。

在现代`JavaScript`中，有一个更简洁的方式来写这样的循环。

```js
for (let entry of JOURNAL) {
  console.log(`${entry.events.length} events.`);
}
```

当 `for` 循环看起来这样，一个变量定义之后跟着 `of`，它会循环遍历 `of` 之后所给定的值的所有元素。不仅对数组适用，对字符串和其他一些数据结构也适用。我们将在[第六章][link_chapter_6]讨论它 *如何* 工作。

## 最终分析

我们需要对数据集中出现的每一张事件类型计算其相关性。为此，我们首先得 *找到* 每种事件类型。

```js
function journalEvents(journal) {
  let events = [];
  for (let entry of journal) {
    for (let event of entry.events) {
      if (!events.includes(event)) {
        events.push(event);
      }
    }
  }
  return events;
}

console.log(journalEvents(JOURNAL));
// → ["carrot", "exercise", "weekend", "bread", …]
```

通过遍历所有的事件，并将还不存在于 `events` 数组中的那些添加进来，该函数收集了每一种事件类型。

此时，我们可以查看所有的相关性。

```js
for (let event of journalEvents(JOURNAL)) {
  console.log(event + ":", phi(tableFor(event, JOURNAL)));
}
// → carrot:   0.0140970969
// → exercise: 0.0685994341
// → weekend:  0.1371988681
// → bread:   -0.0757554019
// → pudding: -0.0648203724
// and so on...
```

大部分相关性看起来都接近于 0。吃胡萝卜、面包或布丁很明显没能触发松鼠变形事件。它 *确实* 更多地发生在周末。我们来过滤一下结果，只显示相关性大于 0.1 或小于 -0.1 的部分。

```js
for (let event of journalEvents(JOURNAL)) {
  let correlation = phi(tableFor(event, JOURNAL));
  if (correlation > 0.1 || correlation < -0.1) {
    console.log(event + ":", correlation);
  }
}
// → weekend:        0.1371988681
// → brushed teeth: -0.3805211953
// → candy:          0.1296407447
// → work:          -0.1371988681
// → spaghetti:      0.2425356250
// → reading:        0.1106828054
// → peanuts:        0.5902679812
```

啊哈！有两个因素，其相关性明显强于其他因素。吃花生对于变成松鼠有着很强的正向效应，而刷牙则有很大的负效应。

有意思。我们来尝试一下。

```js
for (let entry of JOURNAL) {
  if (entry.events.includes("peanuts") &&
     !entry.events.includes("brushed teeth")) {
    entry.events.push("peanut teeth");
  }
}
console.log(phi(tableFor("peanut teeth", JOURNAL)));
// → 1
```

这是个明显的结果。现象恰好发生于当雅克吃了花生但没有刷牙的时候。如果他不懒于对待牙科卫生，就不会遭遇这种痛苦。

知道了这个，雅克完全停止吃花生，并发现再也没有发生过变形了。

很多年里，对雅克来说，事情都很顺利。但在某个时候，他丢掉了工作。因为他住在一个糟糕的乡村，没有工作就意味着没有医疗服务，他不得不受雇于一个马戏团并表演 *无敌松鼠人*，每次表演前都要在嘴巴里塞满花生酱。

一天，厌烦于这样可鄙的存在，雅克没有变回人形，沿着马戏团帐篷的缝隙跳跃着，随后消失于森林中。再也没有人见过他。

## 深入数组

结束本章之前，我想再给你介绍一些对象相关的概念。首先介绍一些有用的数组方法。

我们在[本章前面部分][link_chapter_4#array_methods]已经看到 `push` 和 `pop` 了：在数字末端添加和删除元素。相应的在数组前面添加和删除元素的方法为 `unshift` 和 `shift`。

```js
let todoList = [];
function remember(task) {
  todoList.push(task);
}
function getTask() {
  return todoList.shift();
}
function rememberUrgently(task) {
  todoList.unshift(task);
}
```

这段程序管理着任务队列。通过调用 `remember("groceries")` 向队列的末端添加任务，当准备做些事情的时候，就调用 `getTask()` 来从队列中获取(并删除)前面的项。`rememberUrgently` 函数也会添加任务，但是它会添加到队列的前面而不是后面。

要搜索一个特定值，数组提供了 `indexOf` 方法。它会从头到尾遍历数组，并在请求的值被找到时返回当前索引 —— 或者在未找到时返回 -1。要从末端而不是开头搜索，有一个类似的方法 `lastIndexOf`。

```js
console.log([1, 2, 3, 2, 1].indexOf(2));
// → 1
console.log([1, 2, 3, 2, 1].lastIndexOf(2));
// → 3
```

`indexOf` 和 `lastIndexOf` 都接受可选的用于指示从哪儿开始搜索的第二个参数。

另一个基础的数组方法是 `slice`，它接受起始和末尾的索引为参数并返回仅由它们之间的元素所组成的一个数组。起始索引包括在内，而末尾索引则不包括。

```js
console.log([0, 1, 2, 3, 4].slice(2, 4));
// → [2, 3]
console.log([0, 1, 2, 3, 4].slice(2));
// → [2, 3, 4]
```

当末尾索引没有提供时，`slice` 会获取起始索引之后的所有元素。也可以省略起始索引来拷贝整个数组。

`concat` 方法用于粘合数组以生成一个新数组，类似于字符串的 `+` 操作符。如下例子演示了 `concat` 和 `slice` 实战。它以一个数组和一个索引为参数，并返回原数组的一个拷贝，其中指定索引对应的元素被删除掉。

```js
function remove(array, index) {
  return array.slice(0, index)
    .concat(array.slice(index + 1));
}
console.log(remove(["a", "b", "c", "d", "e"], 2));
// → ["a", "b", "d", "e"]
```

如果传给 `concat` 一个不是数组的参数，这个值将被添加到新的数组，就好像它是一个单元素的数组。

## 字符串及其性质

我们可以从字符串值中读取像 `length` 和 `toUpperCase` 这样的属性。但是如果你尝试添加一个新的属性，是不会生效的。

```js
let kim = "Kim";
kim.age = 88;
console.log(kim.age);
// → undefined
```

字符串、数字和布尔类型的值不是对象，如果你尝试对其设置新的属性，尽管语言不会抱怨，但实际上并没有保存这些属性。如前所述，这些值是不可变的，无法更改。

不过这些类型确实有内置的属性。每个字符串值都有很多方法。一些非常有用的像是 `slice` 和 `indexOf`，与数组的同名方法类似。

```js
console.log("coconuts".slice(4, 7));
// → nut
console.log("coconut".indexOf("u"));
// → 5
```

有一个区别：字符串的 `indexOf` 可以搜索包含多个字符的字符串，而对应的数组方法只能查询单一元素。

```js
console.log("one two three".indexOf("ee"));
// → 11
```

`trim` 方法用来删除字符串开头和结尾的空白字符(空格，换行，制表符和类似的字符)。

```js
console.log("  okay \n ".trim());
// → okay
```

[上一章][link_chapter_3]的 `zeroPad` 函数也存在一个方法。这就是 `padStart`：以目标长度和填充字符为参数。

```js
console.log(String(6).padStart(3, "0"));
// → 006
```

可以用 `split` 将一个字符串以另一个字符串为分隔符分隔开，然后用 `join` 再把它聚合在一起。

```js
let sentence = "Secretarybirds specialize in stomping";
let words = sentence.split(" ");
console.log(words);
// → ["Secretarybirds", "specialize", "in", "stomping"]
console.log(words.join(". "));
// → Secretarybirds. specialize. in. stomping
```

可以用 `repeat` 方法对字符串进行重复：它会创建一个新的字符串，包含着原始字符串的多个拷贝并粘合在一起。

```js
console.log("LA".repeat(3));
// → LALALA
```

我们已经看过了字符串类型的 `length` 属性。获取字符串中的单个字符看起来跟获取数组元素类似(将在[第五章][link_chapter_5]继续讨论)。

```js
let string = "abc";
console.log(string.length);
// → 3
console.log(string[1]);
// → b
```

## rest 参数

函数可以接受任意数量的参数，是非常有用的。比如，`Math.max` 可以计算 *所有* 传入的参数中的最大值。

要写出这样的函数，可以在函数的最后一个参数前放上三个点，像这样：

```js
function max(...numbers) {
  let result = -Infinity;
  for (let number of numbers) {
    if (number > result) result = number;
  }
  return result;
}
console.log(max(4, 1, 9, -2));
// → 9
```

当这样的函数被调用时，*rest 参数* 被绑定到一个包含所有更多参数的数组。如果在此之前还有其他参数，这些值不包含在内。当像 `max` 这样作为第一个参数时，它将持有所有参数。

也可以用类似的“三个点”标记法来用参数的数组 *调用* 函数。

```js
let numbers = [5, 1, 7];
console.log(max(...numbers));
// → 7
```

这会“展开”该数组到函数调用中，将其元素作为单独的参数传入。也可以像这样来和其他参数一起引入一个数组，比如 `max(9, ...numbers, 2)`。

类似地，方括号的数组标记法也允许三点操作符来展开另一个数组到新的数组中：

```js
let words = ["never", "fully"];
console.log(["will", ...words, "understand"]);
// → ["will", "never", "fully", "understand"]
```

## `Math` 对象

正如所见，`Math` 是一个跟数字相关的应用函数包，比如 `Math.max` (最大值)，`Math.min` （最小值）和 `Math.sqrt` (平方根)。

`Math` 对象用作组合一系列相关功能的容器。仅仅有一个 `Math` 对象，并且几乎不会直接用它的值本身。相反，它提供了一个 *命名空间*，这样所有这些函数和值就不需要是全局绑定了。

太多的全局绑定会 *污染* 命名空间。越多名称被占用，就越有可能意外地覆盖某些现存的绑定。比如，在你的程序中想取个名称叫 `max` 也不是不可能。因为`JavaScript`内置的 `max` 函数被安全地塞进了 `Math` 对象，我们不必担心它被覆盖的问题。

当你在定义一个名称已被占用的绑定时，很多语言会阻止你，或者至少会发出警告。`JavaScript`对于用 `let` 或 `const` 声明的绑定也会如此，但是对于标准的绑定就不会这样，对用 `var` 或 `function` 声明的绑定也不会。

回到 `Math` 对象。如果你需要做三角运算，`Math` 可以帮上忙。它包含了 `cos` (余弦)，`sin` (正弦)和 `tan` (正切)，以及他们各自的反函数 `acos`、`asin` 和 `atan`。数字 π (pi) —— 或者`JavaScript`中最接近的数字 —— 可用 `Math.PI` 表示。这里有一个古老的编程传统：常量的名称全部大写。

```js
function randomPointOnCircle(radius) {
  let angle = Math.random() * 2 * Math.PI;
  return {x: radius * Math.cos(angle),
          y: radius * Math.sin(angle)};
}
console.log(randomPointOnCircle(2));
// → {x: 0.3667, y: 1.966}
```

如果你不熟悉正弦和余弦，也不用担心。在[第十四章][link_chapter_14#sin_cos]中用到它们的时候，我会解释的。

上面的例子用到了 `Math.random`。这是一个每次调用时返回 0 (包含)和 1 (不包含)之间的一个伪随机数的函数。

```js
console.log(Math.random());
// → 0.36993729369714856
console.log(Math.random());
// → 0.727367032552138
console.log(Math.random());
// → 0.40180766698904335
```

尽管计算机是确定性的机器 —— 它们总是对相同的输入做出同样方式的反应 —— 也有可能让它们生成看似随机的数字。为此，机器保存了某个隐藏值，无论何时你请求一个新的随机数，它都会对这个隐藏值进行复杂的运算以创造一个新的值。它保存了这个新的值，并返回派生于它的某个数字。通过这种方式，它就能以 *看似* 随机的方式生成全新的、难以预测的数字。

如果我们需要一个随机的整数而不是小数，我们可以对 `Math.random` 的结果使用 `Math.floor`(会向下取整到最接近的整数)。

```js
console.log(Math.floor(Math.random() * 10));
// → 2
```

对随机数乘以 10 会生成大于等于 0 且小于 10 的一个数字。因为 `Math.floor` 向下取整，这个表达式将以相同的概率生成 0 到 9 之间的任一数字。

还有一些函数如 `Math.ceil` (就像“天花板”，它会向上取整)，`Math.round` (取最接近的整数)和 `Math.abs`，用来获取一个数字的绝对值：对负数取反而整数则保持不变。

## 解构

我们先回到 `phi` 函数。

```js
function phi(table) {
  return (table[3] * table[0] - table[2] * table[1]) /
    Math.sqrt((table[2] + table[3]) *
              (table[0] + table[1]) *
              (table[1] + table[3]) *
              (table[0] + table[2]));
}
```

这个函数难以阅读，其中一个原因就是我们有一个指向数组的绑定，而我们更希望有针对数组 *元素* 的绑定，也就是 `let n00 = table[0]` 等等。幸运的是，`JavaScript`中有个简洁的方式来实现。

```js
function phi([n00, n01, n10, n11]) {
  return (n11 * n00 - n10 * n01) /
    Math.sqrt((n10 + n11) * (n00 + n01) *
              (n01 + n11) * (n00 + n10));
}
```

这对于用 `let`、`var` 和 `const` 创建的绑定也适用。如果你知道正绑定的值是一个数组，就可以用方括号来“探查”该值并绑定其内容。

同样的技巧也可以用于对象，此时使用大括号而不是方括号。

```js
let {name} = {name: "Faraji", age: 23};
console.log(name);
// → Faraji
```

注意！如果你尝试结构 `null` 或 `undefined`，将会报错，正如你尝试直接访问这些值的属性一样。

## `JSON`

因为属性只是抓取值而不是包含值，对象和数组在计算机内存中存储为比特序列，它持有着内容的 *地址* —— 内存中的地点。因此，一个包含另一个数组在内的数组(至少)由一块保存内层数组的内存区域和另一块保存外层数组的区域，它包含着(还有其他内容)表示内层数组位置的二进制数。

如果想把数据保存到文件中供后续使用，或通过网络将它传输到另一台计算机，就必须通过某种方式把这些繁杂的内存地址转换为可以存储或发送的描述。我猜你 *会* 把整个计算机内存和感兴趣的值的地址发送过去，但这似乎不是最好的方式。

我们可以 *序列化* 数据。这意味着它被转换成一个扁平的描述。一种流行的序列化格式叫做 *JSON* (发音同“Jason”)，代表`JavaScript`对象标记。它作为一种 Web 数据存储和交换格式被广泛使用，甚至在非 `JavaScript` 语言中。

JSON 看起来很像是用 `JavaScript` 的方式来写数组和对象，只是有一些限制。所有属性名必须用双引号括起来，并且只允许简单数据表达式 —— 不能有函数调用、绑定或其他任何涉及实际运算的内容。JSON 中不允许有注释。

用 JSON 表示的日志，看起来像这样：

```json
{
  "squirrel": false,
  "events": ["work", "touched tree", "pizza", "running"]
}
```

`JavaScript`提供了函数 `JSON.stringify` 和 `JSON.parse`，用来在数据和这种格式间相互转换。第一个以`JavaScript`值为参数，并返回一个 JSON 编码的字符串。第二个以这样的一个字符串为参数并将其转换为它所编码的值。

```js
let string = JSON.stringify({squirrel: false,
                             events: ["weekend"]});
console.log(string);
// → {"squirrel":false, "events":["weekend"]}
console.log(JSON.parse(string).events);
// → ["weekend"]
```

## 总结

对象和数组(一种特殊的对象)提供了把多个值组合成单一值的方式。概念上讲，这使得我们可以把一堆相关的东西放进一个袋子里，然后带着这个袋子四处走动，而不是用胳膊紧抱所有单个的东西。

`JavaScript`中的大多数值都有属性，`null` 和 `undefined` 除外。通过 `value.prop` 或 `value["prop"]` 来访问属性。对象常常用名称来标识属性，并且存储着几乎固定的属性集合。而数组通常包含变化数量的概念上相同的值，并且用数字(从 0 开始)作为属性的名称。

数组中 *确实* 有一些命名的属性，比如 `length` 和很多方法。方法是以属性的方式存在的函数，并(通常)作用于拥有该属性的值。

可以用一种特殊的 `for` 循环来迭代数组 —— `for (let element of array)`。

## 练习

### 序列的和

本书的[简介][link_chapter_0]中提到了下面这种优雅的计算数字序列和的方式。

```js
console.log(sum(range(1, 10)));
```

[chapter_picture_4]: ../assets/chapter_picture_4.jpg
[pizza-squirrel]: ../assets/pizza-squirrel.svg
[link_chapter_0]: ../Part_0_Introduction/00_intro.md
[link_chapter_3]: ../Part_1_Language/03_functions.md
[link_chapter_4#exercise_deep_compare]: ../Part_1_Language/04_data.md#exercise_deep_compare
[link_chapter_4#array_methods]: ../Part_1_Language/04_data.md#array_methods
[link_chapter_5]: ../Part_1_Language/05_higher_order.md
[link_chapter_6]: ../Part_1_Language/06_object.md
[link_chapter_14#sin_cos]: ../Part_2_Browser/14_dom.md#sin_cos
[sandbox_4]: https://eloquentjavascript.net/code#4
[download_journal]: https://eloquentjavascript.net/code/journal.js
