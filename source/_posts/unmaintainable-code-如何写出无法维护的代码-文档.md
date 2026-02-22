---
title: unmaintainable code 如何写出无法维护的代码 - 文档
date: 2026-02-10 10:12:21
tags: 
    - "unmaintainable code"
    - "代码质量"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/code.jpg"
excerpt: "unmaintainable code 文档 https://www.mindprod.com/jgloss/unmain.html"
---

## Documentation  文档

> Any fool can tell the truth, but it requires a man of some sense to know how to lie well.
> 任何傻瓜都能说实话，但要懂得撒谎，就得有点头脑。
> Samuel Butler 塞缪尔·巴特勒

> Incorrect documentation is often worse than no documentation.
> 错误的文档往往比没有文档更糟糕。
> Bertrand Meyer 贝特朗·迈耶

Since the computer ignores comments and documentation, you can lie outrageously and do everything in your power to befuddle the poor maintenance programmer.

由于电脑会无视评论和文档，你可以大肆撒谎，竭尽全力让可怜的维护程序员困惑。

### Lie in the comments 在注释撒谎

: You don’t have to actively lie, just fail to keep comments as up to date with the code.

你不必刻意撒谎，只要不让注释和保持代码同步更新即可。

### Document the obvious  记录显而易见的事情

: Pepper the code with comments like `/* add 1 to i */` however, never document wooly stuff like the overall purpose of the package or method.

： 在代码中添加诸如 `/* 加 1 到 i * /` 的注释，但切勿记录包或方法的整体目的。

### Document How Not Why 记录文档但不写为什么

: Document only the details of what a program does, not what it is attempting to accomplish. That way, if there is a bug, the fixer will have no clue what the code should be doing.

只记录程序具体做了什么，而不是它试图实现什么。这样一来，如果有 bug，修复者就不知道代码应该做什么。

### Avoid Documenting the `Obvious` 避免记录显而易见的事情

: If, for example, you were writing an airline reservation system, make sure there are at least 25 places in the code that need to be modified if you were to add another airline. Never document where they are. People who come after you have no business modifying your code without thoroughly understanding every line of it.

例如，如果你在编写航空预订系统，确保代码中至少有25个需要修改的地方，以便添加另一家航空公司。绝不要记录它们的位置。在你之后的人，没有充分理解你的代码每行，都没有权利修改你的代码。

### On the Proper Use of Documentation Templates 关于文档模板的正确使用

: Consider function documentation prototypes used to allow automated documentation of the code. These prototypes should be copied from one function (or method or class) to another, but never fill in the fields. If for some reason you are forced to fill in the fields make sure that all parameters are named the same for all functions and all cautions are the same but, of course, not related to the current function at all.

考虑函数文档原型，用于实现代码的自动化文档。这些原型应从一个函数（或方法或类）复制到另一个函数，但绝不填充字段。如果因为某些原因被迫填写字段，确保所有参数的名称都相同，所有注意事项也相同，但当然，这些参数与当前函数无关。

### On the Proper Use of Design Documents 关于设计文档的正确使用

: When implementing a very complicated algorithm, use the classic software engineering principles of doing a sound design before beginning coding. Write an extremely detailed design document that describes each step in a very complicated algorithm. The more detailed this document is, the better.

：在实现非常复杂的算法时，采用经典软件工程原则，先做声音设计再开始编码。写一份极其详细的设计文档，描述一个非常复杂算法中的每一步。这份文件越详细越好。

In fact, the design doc should break the algorithm down into a hierarchy of structured steps, described in a hierarchy of auto-numbered individual paragraphs in the document. Use headings at least 5 deep. Make sure that when you are done, you have broken the structure down so completely that there are over 500 such auto-numbered paragraphs. For example, one paragraph might be: (this is a real example)

事实上，设计文档应将算法拆分为结构化步骤的层级结构，文档中以自动编号的单个段落层级描述。航向至少要有5层深。确保完成后，你已经把结构拆得非常完整，超过500个这样的自动编号段落。例如，一段可能是：（这是一个真实的例子）

1.2.4.6.3.13 — Display all impacts for activity where selected mitigations can apply (short pseudocode omitted).

1.2.4.6.3.13 — 显示所有可适用特定缓解措施的活动影响（省略简短伪代码）。

then… (and this is the kicker) when you write the code, for each of these paragraphs you write a corresponding

然后 ......（这才是关键）写代码时，每段都要写对应的段落

```c++
Act1_2_4_6_3_13()
```

Do not document these functions. After all, that’s what the design document is for!

不要记录这些功能。毕竟，设计文档就是为此而设！

Since the design doc is auto-numbered, it will be extremely difficult to keep it up to date with changes in the code (because the function names, of course, are static, not auto-numbered.) This isn’t a problem for you because you will not try to keep the document up to date. In fact, do everything you can to destroy all traces of the document.

由于设计文档是自动编号的，要保持代码更新非常困难（因为函数名当然是静态的，不是自动编号的）。这对你来说不是问题，因为你不会努力保持文档的更新。事实上，尽一切可能销毁所有文件痕迹。

Those who come after you should only be able to find one or two contradictory, early drafts of the design document hidden on some dusty shelving in the back room near the dead 286 computers.

后来跟进你的人应该只能在后屋里某个布满灰尘的架子上，靠近那台死去的286台电脑里，找到一两个相互矛盾的早期设计草案。

### Units of Measure  计量单位

: Never document the units of measure of any variable, input, output or parameter, e. g. feet, metres, cartons. This is not so important in bean counting, but it is very important in engineering work. As a corollary, never document the units of measure of any conversion constants, or how the values were derived. It is mild cheating, but very effective, to salt the code with some incorrect units of measure in the comments. If you are feeling particularly malicious, make up your own unit of measure; name it after yourself or some obscure person and never define it. If somebody challenges you, tell them you did so that you could use integer rather than floating point arithmetic.

：永远不要记录任何变量的度量单位， 输入、输出或参数，例如英尺、米、纸箱。这在 算豆子，但在工程工作中非常重要。作为推论，永远不会 记录任何转换常数的计量单位，或其值的具体情况 衍生。在代码中加盐虽然有点小作弊，但非常有效 评论区的计量单位不正确。如果你感觉特别特别 恶意，自己制定计量单位; 用你自己或某个鲜为人知的人来命名，永远不要给它下定义。如果有人质疑你，告诉他们你是因为这样可以用整数而不是浮点数运算。

### Gotchas  陷阱

: `Never` document gotchas in the code. If you suspect there may be a bug in a class, keep it to yourself. If you have ideas about how the code should be reorganised or rewritten, for heaven’s sake, do not write them down. Remember the words of Thumper in the movie Bambi "If you can’t say anything nice, don’t say anything at all". What if the programmer who wrote that code saw your comments? What if the owner of the company saw them? What if a customer did? You could get yourself fired. An anonymous comment that says This needs to be fixed! can do wonders, especially if it’s not clear what the comment refers to. Keep it vague and nobody will feel personally criticised.

绝不要在代码中记录陷阱。如果你怀疑可能有 课堂里有 bug，自己留着。如果你对代码应该是什么样子有想法 重组或重写的，拜托，千万别写下来。记住 电影《小鹿斑比》中跳跳者的话 ：“如果你不能说好话，那就什么都别说。” 如果写作的程序员会怎样 那个代码看到了你的评论吗？如果公司的老板看到了怎么办？如果 是客户做的吗？你可能会被解雇。一条匿名评论说 这必须被解决！ 这能带来奇效，尤其是当评论不清楚具体指什么时。保持模糊，没人会觉得被针对。

### Documenting Variables  变量文档

: `Never` put a comment on a variable declaration. Facts about how the variable is used, its bounds, its legal values, its implied/displayed number of decimal points, its units of measure, its display format, its data entry rules (e.g. total fill, must enter), when its value can be trusted etc. should be gleaned from the procedural code. If your boss forces you to write comments, lard method bodies with them, but never comment a variable declaration, not even a temporary!

：`永远不要`在变量声明上添加评论。关于变量的使用方式、界限、法律值、隐含/显示的小数点数、计量单位、显示格式、数据录入规则（如总填充、必须输入）、何时可信值等事实，应从程序代码中获得。如果你的老板强迫你写注释，就用它们做 lard 方法的主体，但绝不要评论变量声明，哪怕是临时声明！

### Disparage In the Comments 评论区的贬低

: Discourage any attempt to use external maintenance contractors by peppering your code with insulting references to other leading software companies, especially anyone who might be contracted to do the work, e. g.:

：通过在代码中大量使用侮辱性词语来阻止任何使用外部维护承包商的尝试，尤其是针对任何可能受雇执行维护工作的公司，例如：

```c++
/** The optimised inner loop.
 * This stuff is too clever for the dullard at Software Services Inc., who would
 * probably use 50 times as memory & time using the dumb routines in <math.h>.
 */
class clever_SSInc
   {
   ...
   }
```

If possible, put insulting stuff in syntactically significant parts of the code, as well as just the comments so that management will probably break the code if they try to sanitise it before sending it out for maintenance.

：阻止任何尝试使用外部 维护承包商通过在你的代码中充斥侮辱性引用他人 领先的软件公司，尤其是那些可能承包完成这项工作的企业， 例如： 如果可能的话，在代码中语法重要部分放侮辱性内容，比如 其实只是注释，这样管理层如果尝试，可能会破坏代码 在送去维护前消毒。

### COMMENT AS IF IT WERE CØBØL ON PUNCH CARDS 像在穿孔卡上评论科博尔一样

: Always refuse to accept advances in the development environment arena, especially SCIDs. Disbelieve rumors that all function and variable declarations are never more than one click away and always assume that code developed in Visual Studio 6.0 will be maintained by someone using edlin or vi. Insist on Draconian commenting rules to bury the source code proper.

：永远拒绝 接受开发环境领域的进步，尤其是 SCIDs。不要相信那些关于所有函数和变量声明都不超过一次点击距离的传言，并且总是假设在 Visual Studio 6.0 开发的代码会由使用 edlin 或 vi 的人维护。坚持使用严苛的注释规则来掩盖源代码。

### Monty Python Comments 喜剧式注释

(揭秘Python命名由来：为什么编程语言要以蟒蛇命名？)[https://www.oryoy.com/news/jie-mi-python-ming-ming-you-lai-wei-shen-me-bian-cheng-yu-yan-yao-yi-mang-she-ming-ming.html]

: On a method called makeSnafucated insert only the Javadoc `/* make snafucated */`. Never define what snafucated means anywhere. Only a fool does not already know, with complete certainty, what snafucated means. For classic examples of this technique, consult the Sun AWT (Advanced Windowing Toolkit) Javadoc.

： 在一个叫 makeSnafucated 的方法上，只插入 Javadoc `/* make snafucated */`。永远不要在任何地方定义 “snafuced”是什么意思。只有傻瓜才不会完全确定“snafucated”是什么意思。关于该技术的经典示例，可参考 Sun AWT（Advanced Windowing Toolkit）Javadoc。

### Obsolete Code  过时代码

Be sure to comment out unused code instead of deleting it and relying on version control to bring it back if necessary. In no way document whether the new code was intended to supplement or completely replace the old code, or whether the old code worked at all, what was wrong with it, why it was replaced etc. Comment it out with a lead `/*` and trail `*/` rather than a `//` on each line. That way it might more easily be mistaken for live code and partially maintained.
一定要注释掉未使用的代码，而不是删除它， 依赖版本控制在必要时恢复。绝不能记录 新代码旨在补充或完全取代旧代码，或者 旧代码是否能正常工作，问题出在哪里，为什么被替换等等。 用引言`/*` 和尾`*/` 来注释，而不是每行都加注释。这样更容易被误认为是实时代码，从而部分维护。