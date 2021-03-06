# 第五章 高阶函数

> “有两种构建软件设计的方式：一种是让它如此简单，以至于明显没有缺陷；另一种是让它如此复杂，以至于没有明显的缺陷。”
>
> -- C.A.R. Hoare, 1980 ACM 图灵奖讲座

![chapter_picture_5][chapter_picture_5]

大型程序非常昂贵，不仅仅是因为构建所要花的时间。程序大小几乎总是牵扯到复杂度，而复杂度让程序员迷糊。困惑的程序员反过来会在程序中引入错误(*bug*)。大规模的程序为这些 bug 提供了大量的隐藏空间，使得它们难被找寻。

我们简单回顾一下简介中最后两个示例程序。第一个是自包含的，有 6 行的长度。

```js
let total = 0, count = 1;
while (count <= 10) {
  total += count;
  count += 1;
}
console.log(total);
```

第二个依赖两个外部函数，只有 1 行。

```js
console.log(sum(range(1, 10)));
```

哪一个更可能包含 bug ？

如果我们算上 `sum` 和 `range` 的大小，第二个程序也很大 —— 甚至比第一个还大。但是，我依然坚称它更可能是正确的那个。

它更可能正确是因为答案是以一个与要解决的问题相对应的词汇来表达的。求一系列数字的和跟循环和计数器没太大关系，而是关于序列和求和的。

这个词汇(函数 `sum` 和 `range`)的定义仍然涉及到循环、计数器和其他不可避免的细节。但是因为它们表达着比作为整体的程序更简单的概念，它们更容易写正确。

## 抽象

在编程中，这类词汇通常称为 *抽象*。抽象隐藏了细节，并使得我们可以在一个更高(或更抽象)的层级上来讨论问题。

作为类比，比较如下两种豌豆汤的烹饪法：

> 按每人 1 杯的量把干豌豆倒进一个容器中。加水直至豌豆被完全覆盖。保持豌豆在水中至少 12 小时。把豌豆从水中取出并放入一个烹饪平底锅。添加每人 4 杯的水。盖住平底锅，并煨炖 2 个小时。取每人半个的洋葱。用刀子将其切片。把它加到豌豆中。去每人一根的芹菜。用刀子将其切段。把它加到豌豆中。取每人一个胡萝卜。将其切片。用刀子！把它加到豌豆中。再烹饪 10 分钟。

第二种方法：

> 每人：1 杯切开的干豌豆，半个切开的洋葱，一根芹菜和一个胡萝卜。
>
> 浸泡豌豆 12 小时。放入 4 杯水（每人）并煨炖 2小时。切开并添加蔬菜。再烹饪 10 分钟。

第二种更简短、更易于理解。不过你确实需要理解一些烹饪相关的词 —— *浸泡*，*煨炖*，*切开*，以及我猜的 *蔬菜*。

在编程的时候，我们不能依赖我们需要的所有词语都已经在字典中了。如果这样，你就有可能陷入第一种烹饪法的模式中 —— 解决计算机需要执行的具体步骤，一个接一个地，而对于它们所表达的更高级的概念却视而不见。

在编程中，注意到正在一个太低的抽象层级上工作，是一个很有用的技能。

## 抽象重复

正如我们目前已经看到的那样，简单的函数就是一种构建抽象的好方法。但有时候它们也不够。

对程序来说，做某事指定次数是很常见的。你可以写一个 `for` 循环来解决，像这样：

```js
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

我们可以把“做某事 *N* 次”抽象成一个函数吗？没问题，很容易写出一个调用 `console.log` *N* 次的函数。

```js
function repeatLog(n) {
  for (let i = 0; i < n; i++) {
    console.log(i);
  }
}
```

但是如果我们想做其他事情而不是打印出数字呢？因为“做某事”可以用一个函数表示，而函数也仅仅是值，我们可以把动作作为一个函数值传递进去。

```js
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}

repeat(3, console.log);
// → 0
// → 1
// → 2
```

不需要传递一个预先定义好的函数给 `repeat`。通常来说，相反，你希望在这个点上创建一个函数。

```js
let labels = [];
repeat(5, i => {
  labels.push(`Unit ${i + 1}`);
})
console.log(labels);
// → ["Unit 1", "Unit 2", "Unit 3", "Unit 4", "Unit 5"]
```

这被组织得有点像 `for` 循环 —— 首先描述循环的类型，然后是循环体。不过，这里的循环体被写成一个函数值，并放置在对 `repeat` 的调用的括号中。这就是为什么必须用大括号 *和* 括号来关闭。在像这个例子的情况里，循环体就是一个单一的表达式，也可以省略掉大括号，把循环写成一行。

## 高阶函数

在其他函数上进行操作的函数，以它们为参数或将其返回，叫做 *高阶函数*。我们已经看过了，函数就是常规的值，这样的函数存在的事实就没有特别值得注意的了。这个术语来自数学，其中的函数和其他值是严格区分的。

高阶函数使得我们可以对 *动作* 进行抽象，而不仅仅是值。有多种出现的形式。比如，你可以拥有创建新函数的函数。

```js
function greaterThan(n) {
  return m => m > n;
}
let greaterThan10 = greaterThan(10);
console.log(greaterThan10(11));
// → true
```

也可以有改变其他函数的函数。

```js
function noisy(f) {
  return (...args) => {
    console.log("calling with", args);
    let result = f(...args);
    console.log("called with", args, ", returned", result);
    return result;
  };
}
noisy(Math.min)(3, 2, 1);
// → calling with [3, 2, 1]
// → called with [3, 2, 1], returned 1
```

甚至可以写函数来提供新型的控制流。

```js
function unless(test, then) {
  if (!test) then();
}
repeat(3, n => {
  unless(n % 2 == 1, () => {
    console.log(n, 'is even');
  })
});
// → 0 'is even'
// → 2 'is even'
```

有一个内置的数组方法 `forEach`，作为一个高阶函数提供了类似于 `for/of` 循环的功能。

```js
["A", "B"].forEach(l => console.log(l));
// → A
// → B
```

## 脚本数据集

高阶函数非常有用的一个领域，就是数据处理。为了处理数据，我们需要一些真实数据。本章会用到一个关于脚本的数据集 —— 像拉丁语、西里尔语或阿拉伯语这样的书写系统。

回想一下[第一章][link_chapter_1]中的 `Unicode`，把一个数字赋予书写语言中每个字符的系统。这些字符的大多数都与一个特定的脚本相关联。该标准包含了 140 种不同的脚本。其中 81 种现如今仍在使用，另 59 种已成为历史。

[chapter_picture_5]: ../assets/chapter_picture_5.jpg
[link_chapter_1]: ../Part_1_Language/01_values.md
