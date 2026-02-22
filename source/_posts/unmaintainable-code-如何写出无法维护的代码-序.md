---
title: unmaintainable code 如何写出无法维护的代码 - 序
date: 2026-02-09 08:51:54
tags: 
    - "unmaintainable code"
    - "代码质量"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/code.jpg"
excerpt: "unmaintainable code 序 https://www.mindprod.com/jgloss/unmain.html "
---

(unmaintainable code)[https://www.mindprod.com/jgloss/unmain.html]

> If builders built buildings the way programmers write programs, then the first woodpecker that came along would destroy civilization.
> 如果建筑商建造建筑的方式是程序员编写程序，那么第一个来到啄木鸟会毁灭文明。
> Gerald Weinberg (1933-10-27 age:84) Weinberg’s Second Law 杰拉尔德·温伯格 （1933-10-27，年龄：84 岁） 温伯格第二定律

> Never ascribe to malice, that which can be explained by incompetence.
> 永远不要归咎于恶意，那可以用无能来解释的。
> Napoléon Bonaparte 拿破仑·波拿巴

This essay has been like rock candy, seed the string with sugar, soak in sugar water, soon it grew out of control.

这篇文章就像糖果一样，先把糖撒在绳子上，浸泡在糖水中，很快它就失控了。

In the interests of creating employment opportunities in the Java programming field, I am passing on these tips from the masters on how to write code that is so difficult to maintain, that the people who come after you will take years to make even the simplest changes. Further, if you follow all these rules religiously, you will even guarantee yourself a lifetime of employment, since no one but you has a hope in hell of maintaining the code. Then again, if you followed all these rules religiously, even you wouldn’t be able to maintain the code!

为了在 Java 编程领域创造就业机会，我传授这些大师们的技巧，教你如何编写极其难以维护的代码，以至于后人即使是最简单的改动也要花费多年时间。此外，如果你严格遵守所有这些规则，你甚至能保证终身就业，因为没有人 但你几乎没有希望维护代码。不过，如果你跟着去的话 严格遵守这些规则 ，就算是你也维护不了代码！

You don’t want to overdo this. Your code should not look hopelessly unmaintainable, just be that way. Otherwise it stands the risk of being rewritten or refactored.

你不想做得太过头。你的代码不应该看起来 完全无法维护，就这样吧 。否则它就有被重写或重构的风险。

## Copyright  版权

I would like to remind you this essay is copyrighted material. It is illegal to repost it without permission. I will usually give you that permission if you translate the essay into another language and if you provide a link back to the English-language original. I do this for three reasons.

我想提醒你，这篇文章是受版权保护的材料。未经许可转载是违法的。如果你把文章翻译成另一种语言，并且提供英文原文的链接，我通常会批准你。我这样做有三个原因。

1. That way any change I make to the essay is instantly reflected in any English-language copy anyone reads.
    这样我对文章的任何修改都会立刻反映在任何英文文稿中。
2. That way the formatting and images are preserved. Pirated copies usually screw up the formatting.
    这样格式和图片就能被保留。盗版通常会搞乱格式。
3. Google ad revenue from this essay is the main source of income from the website. It pays to keep me on the air.
    谷歌广告收入是该网站的主要收入来源。让我继续直播是有好处的。

Please report any copyright violations.

请举报任何版权侵权行为。

## General Principles  一般原则

To foil the maintenance programmer, you have to understand how he thinks. He has your giant program. He has no time to read it all, much less understand it. He wants to rapidly find the place to make his change, make it and get out and have no unexpected side effects from the change.

要阻止维护程序员，你必须了解他的思维方式。他有你的庞大程序。他没有时间全部读完，更别说理解了。他想快速找到地方去改变，成功后离开，避免改变带来的意外副作用。

He views your code through a toilet paper tube. He can only see a tiny piece of your program at a time. You want to make sure he can never get at the big picture from doing that. You want to make it as hard as possible for him to find the code he is looking for. But even more important, you want to make it as awkward as possible for him to safely `ignore` anything.

他通过卫生纸管查看你的代码。他只能看到你一小部分 一次做项目。你要确保他永远无法通过行动触及大局 那个。你要让他尽可能难找到他想要的代码。 但更重要的是，你要让他尽可能尴尬，安全地 `什么都忽略` 。

Programmers are lulled into complacency by conventions. By every once in a while, by subtly violating convention, you force him to read every line of your code with a magnifying glass.

程序员被惯例所麻痹，陷入自满。偶尔，你通过微妙地打破惯例，强迫他用放大镜仔细阅读你的代码。

You might get the idea that every language feature makes code unmaintainable — not so, only if properly misused.

你可能会误以为每个语言特性都让代码无法维护——其实并非如此，只有被误用时才会如此。

Refactoring is a most emotionally-satisfying activity. It is second only to sex. Your own inchoate intention suddenly shines through with blinding clarity.

重构是一项极具情感满足感的活动。仅次于性。你那模糊的意图突然清晰地闪耀出来