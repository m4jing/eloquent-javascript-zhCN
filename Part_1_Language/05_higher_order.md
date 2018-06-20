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

[chapter_picture_5]: ../assets/chapter_picture_5.jpg
