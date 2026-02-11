---
title: unmaintainable code 如何写出无法维护的代码 - 程序设计
date: 2026-02-11 09:17:58
tags: 
    - "unmaintainable code"
    - "代码质量"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/code.jpg"
excerpt: "unmaintainable code 程序设计 https://www.mindprod.com/jgloss/unmain.html "
---

## Program Design  程序设计

> The cardinal rule of writing unmaintainable code is to specify each fact in as many places as possible and in as many ways as possible.
> 编写不可维护代码的基本规则是尽可能多地、以尽可能多的方式指定每个事实。
> Roedy 罗迪

The key to writing maintainable code is to specify each fact about the application in only one place. To change your mind, you need change it in only one place and you are guaranteed the entire program will still work. Therefore, the key to writing unmaintainable code is to specify a fact over and over, in as many places as possible, in as many variant ways as possible. Happily, languages like Java go out of their way to make writing this sort of unmaintainable code easy. For example, it is almost impossible to change the type of a widely used variable because all the casts and conversion functions will no longer work and the types of the associated temporary variables will no longer be appropriate. Further, if the variable is displayed on the screen, all the associated display and data entry code has to be tracked down and manually modified. The Algol family of languages which include C and Java treat storing data in an array, Hashtable, flat file and database with totally different syntax. In languages like Abundance and to some extent Smalltalk, the syntax is identical; just the declaration changes. Take advantage of Java’s ineptitude. Put data you know will grow too large for RAM (Random Access Memory), for now into an array. That way the maintenance programmer will have a horrendous task converting from array to file access later. Similarly place tiny files in databases so the maintenance programmer can have the fun of converting them to array access when it comes time to performance tune.

编写可维护代码的关键是将关于应用程序的每个事实都集中在一个地方。要改变主意，只需在一个地方修改，就能保证整个程序依然有效。因此，编写不可维护代码的关键是反复指定一个事实，尽可能多地在各种地方，以各种变体方式。幸运的是，像 Java 这样的语言会尽力让写这种难以维护的代码变得简单。例如，几乎不可能更改一个广泛使用的变量的类型，因为所有铸造和转换函数都不再工作，相关临时变量的类型也不再适用。此外，如果变量显示在屏幕上，所有相关的显示和数据录入代码都必须被追踪并手动修改。包括 C 和 Java 在内的 Algol 语言家族将数据存储在数组、哈希表、平面文件和数据库中 ，完全是 不同的语法。在像 Abundance 和某种程度上的 Smalltalk 这样的语言中，语法 是相同的;只是声明内容变了。利用 Java 的笨拙。 暂时把你知道会对内存 （Random Access Memory）来说太大的数据放进一个数组。这样维护程序员以后从数组转换到文件访问会非常麻烦。同样地，把小文件放进数据库，让维护程序员在性能调优时可以把它们转换成数组访问。

### Java Casts 类型转换

: Java’s casting scheme is a gift from the Gods. You can use it without guilt since the language requires it. Every time you retrieve an object from a Collection you must cast it back to its original type. Thus the type of the variable may be specified in dozens of places. If the type later changes, all the casts must be changed to match. The compiler may or may not detect if the hapless maintenance programmer fails to catch them all (or changes one too many). In a similar way, all matching casts to (short) need to be changed to (int) if the type of a variable changes from short to int. There is a movement afoot in invent a generic cast operator (cast) and a generic conversion operator (convert) that would require no maintenance when the type of variable changes. Make sure this heresy never makes it into the language specification. Vote no on RFE 114691 and on genericity which would eliminate the need for many casts.

：Java 的类型转换方案是神赐的礼物。你可以使用 因为语言要求，所以没有内疚感。每次你取回一个物体 你必须从集合中将其铸造回原始类型。因此 的类型 变量可以在数十个地方指定。如果类型后来发生变化，所有 必须更换铸件以匹配。编译器可能会检测到，也可能不会检测到...... 维护程序员没能全部抓到（或者改了太多）。在 同理，所有匹配到 （short） 的投射，如果变量类型从变换，则需要将所有匹配的投射改为 （int）。 短到智。 目前正在推动发明通用铸算符 （铸造 ）和通用转换算符 （转换 ）的趋势，这些算子在变量类型变化时无需维护。确保这种异端永远不要出现在语言规范中。反对 RFE 的反对 114691 和泛泛性投票，这样就不用再多投了一次票。

### Exploit Java’s Redundancy 利用 Java 的冗余

: Java insists you specify the type of every variable twice. Java programmers are so used to this redundancy they won’t notice if you make the two types slightly different, as in this example:

： Java 要求你指定每个 的类型 变量为两次。Java 程序员已经习惯了这种冗余，他们不会 注意如果你把这两种类型稍微不同，比如这个例子：

```java
// note subtle spelling change
Bubblegum b = new Bubblegom();
```

Unfortunately the popularity of the ++ operator makes it harder to get away with pseudo-redundant code like this:

不幸的是，++作符的流行使得像这样伪冗余代码变得更难被利用：

```java
// note subtle spelling change
swimmer = swimner + 1;
```

### Never Validate  永远不要验证

: Never check input data for any kind of correctness or discrepancies. It will demonstrate that you absolutely trust the company’s equipment as well as that you are a perfect team player who trusts all project partners and system operators. Always return reasonable values even when data inputs are questionable or erroneous.

切勿检查输入数据是否有任何正确性或差异。这会表明你完全信任公司的设备，同时你是一个信任所有项目合作伙伴和系统的完美团队成员。即使数据输入存在疑问或错误，也应始终返回合理的数值。

### Be polite, Never Assert 要有礼貌，绝不Assert

: Avoid the assert() mechanism because it could turn a three-day debug fest into a ten minute one.

避免使用 assert() 机制，因为这可能会让三天的调试环节变成十分钟的调试。

### Avoid Encapsulation  避免封装

: In the interests of efficiency, avoid encapsulation. Callers of a method need all the external clues they can get to remind them how the method works inside.

：为了效率，避免封装。调用方法的人需要尽可能多的外部线索来提醒他们方法内部的工作原理。

### Clone & Modify  克隆与修改

: In the name of efficiency, use cut/paste/clone/modify. This works much faster than using many small reusable modules. This is especially useful in shops that measure your progress by the number of lines of code you’ve written.

： 为了效率，使用剪切/粘贴/克隆/修改。这比使用许多小型可重复使用模块快得多。这在那些通过你写了多少行代码来衡量进度的店里尤其有用。

### Use Static Arrays  使用静态数组

: If a module in a library needs an array to hold an image, just define a static array. Nobody will ever have an image bigger than 512 × 512, so a fixed-size array is OK. For best precision, make it an array of doubles. Bonus effect for hiding a 2 Meg static array which causes the program to exceed the memory of the client’s machine and thrash like crazy even if they never use your routine.

：如果库中的模块需要数组来存储图像，只需定义静态数组即可。没有人会看到比512×512更大的图像，所以固定大小的数组是可以接受的。为了获得最佳精度，可以让它变成双倍数组。隐藏2兆静态数组的额外效果，这会让程序超过客户端机器的内存，即使他们从未使用你的例程也会疯狂抖动。

### Dummy Interfaces  虚拟接口

: Write an empty interface called something like WrittenByMe and make all of your classes implement it. Then, write wrapper classes for any of Java’s built-in classes that you use. The idea is to make sure that every single object in your program implements this interface. Finally, write all methods so that both their arguments and return types are WrittenByMe. This makes it nearly impossible to figure out what some methods do, and introduces all sorts of entertaining casting requirements. For a further extension, have each team member have his/her own personal interface (e.g., WrittenByJoe); any class worked on by a programmer gets to implement his/her interface. You can then arbitrarily refer to objects by any one of a large number of meaningless interfaces!

写一个空接口，叫做类似这样的 WrittenByMe，让你所有的课程都实现它。然后，为你使用的 Java 内置类写封装类。关键是确保程序中的每一个对象都实现了这个接口。最后，写所有方法使其参数和返回类型均为 WrittenByMe。这几乎让人无法弄清某些方法的作用，并引入了各种有趣的选角要求。进一步扩展，让
每个团队成员拥有自己的个人界面（例如，WrittenByJoe）;程序员完成的任何类都可以实现他/她的接口。然后你可以任意地用大量无意义的接口来指代对象！

### Giant Listeners  全能监听器

: Never create separate Listeners for each Component. Always have one listener for every button in your project and simply use massive if…else statements to test for which button was pressed.

：切勿为每个组件单独创建监听器。项目中的每个按钮都要有一个监听器，然后用 massive if...否则 语句用来测试按下了哪个按钮。

### Too Much of A Good Thing™ 好事™太多了

: Go

```java
myPanel.add( getMyButton() );

private JButton getMyButton()
   {
   return myButton;
   }
```

That one probably did not even seem funny. Don’t worry. It will some day.

那句话可能一点都不好笑。别担心。总有一天会的。

### Friendly Friend  友好的朋友

: Use as often as possible the friend-declaration in C++. Combine this with handing the pointer of the creating class to a created class. Now you don’t need to fritter away your time in thinking about interfaces. Additionally you should use the keywords private and protected to prove that your classes are well encapsulated.

：尽可能多地在 C++。结合使用指针 创建类到已创建的类。现在你不用再浪费时间了 在思考接口时。此外，你还应使用关键词 私密且受保护 ，证明你的课程内容被很好地收纳。

### Use Three Dimensional Arrays 使用三维数组

: Lots of them. Move data between the arrays in convoluted ways, say, filling the columns in arrayB with the rows from arrayA. Doing it with an offset of 1, for no apparent reason, is a nice touch. Makes the maintenance programmer nervous.

：很多。以复杂的方式在数组之间移动数据，比如用 arrayA 的行填充 arrayB 的列。无缘无故地用 1 的偏移量来做，是个不错的细节。这让维护程序员感到紧张。

### Mix and Match  混搭

: Use both accessor methods and public variables. That way, you can change an object’s variable without the overhead of calling the accessor, but still claim that the class is a Java Bean. This has the additional advantage of frustrating the maintenence programmer who adds a logging function to try to figure out who is changing the value.

：同时使用访问方法和公共变量。这样，你 可以更改对象变量，而无需调用该访问者， 但仍声称该类是 Java Bean。这还有一个额外的好处，就是会让维护程序员感到沮丧，因为他们添加了日志功能来试图找出是谁在更改数值。

### Wrap, wrap, wrap  包裹，包裹，包裹

: Whenever you have to use methods in code you did not write, insulate your code from that other dirty code by at least one layer of wrapper. After all, the other author might some time in the future recklessly rename every method. Then where would you be? You could of course, if he did such a thing, insulate your code from the changes by writing a wrapper or you could let VAJ (Visual Age for Java) handle the global rename. However, this is the perfect excuse to preemptively cut him off at the pass with a wrapper layer of indirection, before he does anything idiotic. One of Java’s main faults is that there is no way to solve many simple problems without dummy wrapper methods that do nothing but call another method of the same name, or a closely related name. This means it is possible to write wrappers four-levels deep that do absolutely nothing, and almost no one will notice. To maximise the obscuration, at each level, rename the methods, selecting random synonyms from a thesaurus. This gives the illusion something of note is happening. Further, the renaming helps ensure the lack of consistent project terminology. To ensure no one attempts to prune your levels back to a reasonable number, invoke some of your code bypassing the wrappers at each of the levels.

： 每当你必须在代码中使用方法时，你就没有 写出来，至少用一层包装来保护你的代码和那些脏代码。毕竟，另一位作者未来可能会鲁莽地重新命名所有方法。那你会在哪里？当然，如果他真的这么做了，你也可以通过写一个包装器来隔离代码免受更改的影响，或者让 VAJ（Visual Age for Java）来处理全局重命名。不过，这正是完美的 借口在通道处用一层间接包装封堵他， 在他做出愚蠢的事之前 。Java 的一个主要缺点是，没有虚假包装方法，无法解决许多简单问题，这些方法只会调用同名或密切相关的方法。这意味着你可以写出四层深度的封装器，完全没有任何作用，几乎没人会注意到。为了最大化模糊效果，在每个层级中重新命名方法，随机从同义词库中选择同义词。这给人一种有值得注意的事情正在发生的错觉。此外，更名有助于确保项目术语缺乏一致性。为了确保没有人试图把你的关卡修剪回合理的数量，可以在每个关卡调用部分代码绕过封装。

### Wrap Wrap Wrap Some More 包裹，包裹，再包裹

: Make sure all API (Application Programming Interface) functions are wrapped at least 6 to 8 times, with function definitions in separate source files. Using #defines to make handy shortcuts to these functions also helps.

确保所有 API（ 应用程序 P 字形 ） 函数至少被包裹 6 到 8 次，函数定义分别放在不同的源文件中。使用 #defines 来设置这些功能的便捷快捷方式也很有帮助。

### No Secrets!  没有秘密

: Declare every method and variable public. After all, somebody, sometime might want to use it. Once a method has been declared public, it can’t very well be retracted, now can it? This makes it very difficult to later change the way anything works under the covers. It also has the delightful side effect of obscuring what a class is for. If the boss asks if you are out of your mind, tell him you are following the classic principles of transparent interfaces.

：将每个方法和变量声明为公共。毕竟，总有人，某个时候，可能会想用它。一旦方法被声明为公开，就很难被撤回，对吧？这让以后很难改变被窝里的运作方式。这还有一个令人愉快的副作用，就是模糊了职业的用途。如果老板问你是不是疯了，就告诉他你遵循的是透明界面的经典原则。

### The Kama Sutra  《爱经》

: This technique has the added advantage of driving any users or documenters of the package to distraction as well as the maintenance programmers. Create a dozen overloaded variants of the same method that differ in only the most minute detail. I think it was Oscar Wilde who observed that positions 47 and 115 of the Kama Sutra were the same except in 115 the woman had her fingers crossed. Users of the package then have to carefully peruse the long list of methods to figure out just which variant to use. The technique also balloons the documentation and thus ensures it will more likely be out of date. If the boss asks why you are doing this, explain it is solely for the convenience of the users. Again for the full effect, clone any common logic and sit back and wait for it the copies to gradually get out of sync.

： 这种技术还有一个额外好处，就是会让软件包的用户或文档作者以及维护程序员分心。制作十几种同样方法的超载变体，它们仅在最细微的细节上有所不同。我记得是奥斯卡·王尔德观察到，《爱经》第47和第115段其实是一样的，只是第115段那位女士交叉了手指。软件包的用户随后需要仔细浏览一长串方法，确定该使用哪种变体。这种技术还会膨胀文档，确保文档更可能过时。如果老板问你为什么这么做，就说明这纯粹是为了用户的方便。同样，为了达到完整效果，克隆任何通用逻辑，坐等复制品逐渐不同步。

### Permute and Baffle  变换与挡板

: Reverse the parameters on a method called drawRectangle(height, width) to drawRectangle(width, height) without making any change whatsoever to the name of the method. Then a few releases later, reverse it back again. The maintenance programmers can’t tell by quickly looking at any call if it has been adjusted yet. Generalisations are left as an exercise for the reader.

：将名为 drawRectangle（height， width） 的方法参数反转为 drawRectangle（width， height），且不更改方法名称。然后几次释放后，再倒转回去。维护程序员无法快速查看任何通话是否已经调整。泛化部分留给读者练习。

### Theme and Variations  主题与变奏

: Instead of using a parameter to a single method, create as many separate methods as you can. For example instead of setAlignment(int alignment) where alignment is an enumerated constant, for left, right, center, create three methods: setLeftAlignment, setRightAlignment and setCenterAlignment. Of course, for the full effect, you must clone the common logic to make it hard to keep in sync.

：不再使用单一方法的参数， 尽可能多地创建独立的方法。例如，对于左、右、中心，替代 setAlignment（int alignment） 中，alignment 是枚举常数，而是创建三个方法：setLeftAlignment、setRightAlignment 和 setCenterAlignment。当然，为了达到最大效果，你必须克隆通用逻辑，这样才能让它难以保持同步。

### Static Is Good  静态是好事

: Make as many of your variables as possible static. If you don’t need more than one instance of the class in this program, no one else ever will either. Again, if other coders in the project complain, tell them about the execution speed improvement you’re getting.

：尽可能多的变量保持静态。如果 这个项目不需要多于一个课程实例，其他人也不需要。同样，如果项目中的其他程序员抱怨，告诉他们你得到了的执行速度提升。

### Cargill’s Quandry  Cargill困境

: Take advantage of Cargill’s quandary (I think this was his): "any design problem can be solved by adding an additional level of indirection, except for too many levels of indirection." Decompose OO (Object Oriented) programs until it becomes nearly impossible to find a method which actually updates program state. Better yet, arrange all such occurrences to be activated as callbacks from by traversing pointer forests which are known to contain every function pointer used within the entire system. Arrange for the forest traversals to be activated as side-effects from releasing reference counted objects previously created via deep copies which aren’t really all that deep.

：利用Cargill困境（我认为是这样） 这是他的 `)：`“任何设计问题都可以通过增加一层来解决 间接的，除了间接层面过多。”分解 OO（Object Oriented）程序，直到几乎找不到真正更新程序状态的方法。更好的是，通过遍历包含整个系统中所有函数指针的指针森林，安排所有此类事件作为回调激活。安排森林穿越作为释放之前通过深度副本创建的参考计数对象的副作用来激活，这些深度副本其实并不算太深。

### Packratting  包装狂潮

: Keep all of your unused and outdated methods and variables around in your code. After all — if you needed to use it once in 1976, who knows if you will want to use it again sometime? Sure the program’s changed since then, but it might just as easily change back, you "don’t want to have to reinvent the wheel" (supervisors love talk like that). If you have left the comments on those methods and variables untouched, and sufficiently cryptic, anyone maintaining the code will be too scared to touch them.

保留所有未使用和过时的方法和变量 在你的代码里。毕竟——如果你在 1976 年用过一次，谁知道你以后还会想用它呢？虽然项目后来变了，但也可能随时变回去，你“不想重新发明轮子”（主管们喜欢这样说）。如果你对这些方法和变量的注释保持原样，且足够隐晦，维护代码的人会害怕去碰它们。

### And That’s Final  这就是最终决定

: Make all of your leaf classes final. After all, you’re done with the project — certainly no one else could possibly improve on your work by extending your classes. And it might even be a security flaw — after all, isn’t java.lang.String final for just this reason? If other coders in your project complain, tell them about the execution speed improvement you’re getting.

：让你所有的叶子职业都定型。毕竟， 你已经完成了这个项目——当然，没有人能通过延长你的课程来提升你的作品。这甚至可能是安全漏洞——毕竟，java.lang.String 不正是因为这个原因才是最终版的吗？如果项目中的其他程序员抱怨，告诉他们你获得了的执行速度提升。

### Eschew The Interface  避免使用界面

: In Java, disdain the interface. If your supervisors complain, tell them that Java interfaces force you to cut-and-paste code between different classes that implement the same interface the same way and they know how hard that would be to maintain. Instead, do as the Java AWT (Advanced Windowing Toolkit) designers did — put lots of functionality in your classes that can only be used by classes that inherit from them and use lots of instanceof checks in your methods. This way, if someone wants to reuse your code, they have to extend your classes. If they want to reuse your code from two different classes — tough luck, they can’t extend both of them at once! If an interface is unavoidable, make an all-purpose one and name it something like ImplementableIface. Another gem from academia is to append Impl to the names of classes that implement interfaces. This can be used to great advantage, e.g. with classes that implement Runnable.

在 Java 中，可以轻视界面。如果你的主管 抱怨他们，告诉他们 Java 界面强制你在不同类之间复制粘贴代码，而这些类实现相同界面，他们知道维护这有多难。相反，像 Java AWT（ 高级 W 输入 Toolkit）设计者那样——在你的类中加入大量只能被继承类使用的功能，并在方法中使用大量实例检查。这样，如果有人想复用你的代码，就必须扩展你的类。如果他们想重复使用你两个不同类别的代码——那就倒霉了，他们不能同时扩展两个类别！如果某个界面不可避免，就做一个通用的，并命名类似 ImplementableIface 之类。 学术界的另一个宝藏是附录 实现到实现接口的类名称中。这可以被极大利用，例如在实现 Runnable 的类中。

### Avoid Layouts  避免布局

: Never use layouts. That way when the maintenance programmer adds one more field he will have to manually adjust the absolute coordinates of every other thing displayed on the screen. If your boss forces you to use a layout, use a single giant GridBagLayout and hard code in absolute grid coordinates.

：千万别用布局。这样当维护程序员添加一个字段时，他就得手动调整屏幕上显示的每个内容的绝对坐标。如果你的老板强迫你用布局，就用一个巨大的 GridBagLayout，再用绝对网格坐标硬编码。

### Environment variables  环境变量

: If you have to write classes for some other programmer to use, put environment-checking code (getenv() in C++ / System.getProperty() in Java ) in your classes’ nameless static initializers, and pass all your arguments to the classes this way, rather than in the constructor methods. The advantage is that the initializer methods get called as soon as the class program binaries get loaded, even before any of the classes get instantiated, so they will usually get executed before the program main(). In other words, there will be no way for the rest of the program to modify these parameters before they get read into your classes — the users better have set up all their environment variables just the way you had them!

：如果你必须为别人写课程 程序员用的方法是，在类的无名静态初始化器中放置环境检查代码（getenv（） 在 C++ 中 / System.getProperty（） 在 Java 中），并将所有参数通过这种方式传递给类，而不是在构造函数方法中。优点是，初始化方法在类程序二进制文件加载后立即调用，甚至在任何类实例化之前，因此通常会在程序主程序（）之前执行。换句话说，程序的其他部分在这些参数被读入你的类之前，根本无法修改它们——用户最好已经按照你设置的方式设置了所有环境变量！

### Table Driven Logic  表驱动业务逻辑

: Eschew any form of table-driven logic. It starts out innocently enough, but soon leads to end users proofreading and then shudder, even modifying the tables for themselves.

摒弃任何形式的表驱动逻辑。一开始 这看似无害，但很快会促使最终用户进行校对，然后 打了个寒颤 ，甚至自己动手改装桌子。

### Modify Mom’s Fields  修改父级的字段

: In Java, all primitives passed as parameters are effectively read-only because they are passed by value. The callee can modify the parameters, but that has no effect on the caller’s variables. In contrast all objects passed are read-write. The reference is passed by value, which means the object itself is effectively passed by reference. The callee can do whatever it wants to the fields in your object. Never document whether a method actually modifies the fields in each of the passed parameters. Name your methods to suggest they only look at the fields when they actually change them.

在 Java 中，所有作为参数传递的原语实际上都是只读的，因为它们是通过值传递的。被调用方可以修改参数，但这对调用方的变量没有影响。相比之下，所有传递的对象都是读写的。引用是通过值传递的，这意味着对象本身实际上是通过引用传递的。被被调用者可以对你对象中的字段做任何它想做的事。切勿记录方法是否实际修改了每个传递参数的字段。请指出你的方法，建议他们只有在实际更改字段时才查看。

### The Magic of Global Variables 全局变量的魔力

: Instead of using exceptions to handle error processing, have your error message routine set a global variable. Then make sure that every long-running loop in the system checks this global flag and terminates if an error occurs. Add another global variable to signal when a user presses the 'reset' button. Of course, all the major loops in the system also have to check this second flag. Hide a few loops that don’t terminate on demand.

：而不是使用例外来处理错误 处理中，让你的错误消息例程设置一个全局变量。那就确保 系统中每个长时间运行的环路都会检查该全局标志，并且如果 发生了错误。添加另一个全局变量来表示用户按下 “重置”按钮。当然，系统中所有主要环路也必须 检查这个第二个旗帜。隐藏几个不会按需终止的循环。

### Globals, We Can’t Stress These Enough! 全局，我们必须强调这一点！

: If God didn’t want us to use global variables, he wouldn’t have invented them. Rather than disappoint God, use and set as many global variables as possible. Each function should use and set at least two of them, even if there’s no reason to do this. After all, any good maintenance programmer will soon figure out this is an exercise in detective work and she’ll be happy for the exercise that separates real maintenance programmers from the dabblers.

：如果上帝不想让我们使用全局变量，他就不会发明它们。与其让上帝失望，不如尽可能多地使用并设置全局变量。每个函数至少应该使用并设置其中两个，即使没有必要这样做。毕竟，任何优秀的维护程序员很快就会发现这是一场侦探工作，她会为这场区分真正维护程序员和粗略作者感到高兴。

### Globals, One More Time, Boys 全局，再来一次，伙计们

: Global variables save you from having to specify arguments in functions. Take full advantage of this. Elect one or more of these global variables to specify what kinds of processes to do on the others. Maintenance programmers foolishly assume that C functions will not have side effects. Make sure they squirrel results and internal state information away in global variables.

：全局变量省去了函数中指定参数的省略。充分利用这一点。选择一个或多个全局变量来指定对其他变量执行哪些流程。维护程序员愚蠢地认为 C 函数不会有副作用。确保他们把结果和内部状态信息存进全局变量。

### Side Effects  副作用

: In C, functions are supposed to be idempotent, (without side effects). I hope that hint is sufficient.

在 C 中，函数应当是幂级的，（无副作用）。希望这个提示足够了。

### Backing Out  退出

: Within the body of a loop, assume that the loop action is successful and immediately update all pointer variables. If an exception is later detected on that loop action, back out the pointer advancements as side effects of a conditional expression following the loop body.

：在循环正体内，假设循环动作成功，立即更新所有指针变量。如果在该循环动作中后来检测到异常，则将指针推进作为循环正体后条件表达式的副作用退回。

### Local Variables  局部变量

: Never use local variables. Whenever you feel the temptation to use one, make it into an instance or static variable instead to unselfishly share it with all the other methods of the class. This will save you work later when other methods need similar declarations. C++ programmers can go a step further by making all variables global.

：绝不要使用局部变量。每当你感到诱惑时 如果要用一个，可以把它做成实例或静态变量，这样可以无私地分享 它与该类中的所有其他方法一起进行。这将节省你以后的工作量 方法需要类似的声明。C++ 程序员还可以更进一步，将所有变量设置为全局。

### Reduce, Reuse, Recycle  减少、重复使用、回收

: If you have to define a structure to hold data for callbacks, always call the structure PRIVDATA. Every module can define it’s own PRIVDATA. In VC++, this has the advantage of confusing the debugger so that if you have a PRIVDATA variable and try to expand it in the watch window, it doesn’t know which PRIVDATA you mean, so it just picks one.

如果你需要定义一个结构来存储回调数据，请务必调用 PRIVDATA。每个模块都可以定义自己的 PRIVDATA。在 VC++ 中，这种设计的优点是会让调试器感到困惑，比如如果你有一个 PRIVDATA 变量，试图在 watch 窗口中展开它，它不知道你指的是哪个 PRIVDATA，所以它就直接选一个。

### Configuration Files  配置文件

: These usually have the form keyword=value. The values are loaded into Java variables at load time. The most obvious obfuscation technique is to use slightly different names for the keywords and the Java variables. Use configuration files even for constants that never change at run time. Parameter file variables require at least five times as much code to maintain as a simple variable would.

： 这些通常形式为 keyword =value。这些值在加载时被加载到 Java 变量中。最明显的混淆技术是为关键词和 Java 变量使用略有不同的名称。即使是运行时从未改变的常量，也要使用配置文件。参数文件变量维护所需的代码量至少是简单变量的五倍。

### Bloated classes  臃肿的类别

: To ensure your classes are bounded in the most obtuse way possible, make sure you include peripheral, obscure methods and attributes in every class. For example, a class that defines astrophysical orbit geometry really should have a method that computes ocean tide schedules and attributes that comprise a Crane weather model. Not only does this over-define the class, it makes finding these methods in the general system code like looking for a guitar pick in a landfill.

： 为了确保你的类以最晦涩的方式被界定，确保每个类都包含外围、晦涩的方法和属性。例如，定义天体轨道几何的类实际上应该有一种方法来计算构成 Crane 天气模型的海洋潮汐时刻表和属性。这不仅过度定义了类，还让在通用系统代码中找到这些方法就像在垃圾填埋场找吉他拨片一样难。

### Subclass With Abandon  放纵子类

: Object oriented programming is a godsend for writing unmaintainable code. If you have a class with 10 properties (member/method) in it, consider a base class with only one property and subclassing it 9 levels deep so that each descendant adds one property. By the time you get to the last descendant class, you’ll have all 10 properties. If possible, put each class declaration in a separate file. This has the added effect of bloating your INCLUDE or USES statements, and forces the maintainer to open that many more files in his or her editor. Make sure you create at least one instance of each subclass.

面向对象编程对于编写无法维护的代码来说简直是福音。如果你有一个有 10 个属性（成员/方法）的类，考虑一个只有一个属性的基类，并将其子类到 9 层深，每个后代都添加一个属性。等你达到最后一个后代类别时，你会拥有全部 10 个属性。如果可能，将每个类声明单独放到一个文件里。这还会让你的 INCLUDE 或 USES 语句变得臃肿，并迫使维护者在编辑器中打开更多文件。确保你至少为每个子类创建一个实例。