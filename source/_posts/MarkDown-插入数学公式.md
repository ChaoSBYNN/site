---
title: MarkDown 插入数学公式
date: 2017-02-28 20:08:20
tags: [ MarkDown , LaTeX ] 
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/markdown.jpeg"
---

### 使用Google Chart的服务器

> 提示：若无法正确打开
> 在hosts 文件中添加

```bash
 61.91.161.217	chart.googleapis.com
```

```html
<img src="http://chart.googleapis.com/chart?cht=tx&chl= 在此插入Latex公式" style="border:none;">
```

一个例子，

```html
<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" style="border:none;">
```

公式显示结果为：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" style="border:none;">

### 符号对照表

> 注意：不知什么原因 “+” 无法编译 ，后面 “+” 使用 %2B 代替即可

##### 如何输入上下标

> ^表示上标, _表示下标。如果上下标的内容多于一个字符，要用{}把这些内容括起来当成一个整体。上下标是可以嵌套的，也可以同时使用。

例子：x^{y^z}=(1+{\rm e}^x)^{-2xy^w}

显示： 
<img src="http://chart.googleapis.com/chart?cht=tx&chl=x^{y^z}=(1%2B{\rm e}^x)^{-2xy^w}" style="border:none;">

##### 如何输入括号和分隔符

> ()、[]和|表示自己，{}表示{}。当要显示大号的括号或分隔符时，要用\left和\right命令。

例子：f(x,y,z) = 3y^2z \left( 3+\frac{7x+5}{1+y^2} \right)

显示： 
<img src="http://chart.googleapis.com/chart?cht=tx&chl=f(x,y,z) = 3y^2z \left( 3%2B\frac{7x%2B5}{1%2By^2} \right)" style="border:none;">


##### 如何输入分数

例子：\frac{1}{3}　或　1 \over 3

显示： <img src="http://chart.googleapis.com/chart?cht=tx&chl=\frac{1}{3} " style="border:none;"> 　或  <img src="http://chart.googleapis.com/chart?cht=tx&chl=1 \over 3" style="border:none;">

##### 如何输入开方

例子：\sqrt{2}　和　\sqrt[n]{3}

显示： <img src="http://chart.googleapis.com/chart?cht=tx&chl=\sqrt{2}  " style="border:none;">　和　 <img src="http://chart.googleapis.com/chart?cht=tx&chl=\sqrt[n]{3} " style="border:none;">

##### 如何输入省略号

> 数学公式中常见的省略号有两种，\ldots表示与文本底线对齐的省略号，\cdots表示与文本中线对齐的省略号。

例子：f(x1,x2,\ldots,xn) = x1^2 + x2^2 + \cdots + xn^2

显示： 
<img src="http://chart.googleapis.com/chart?cht=tx&chl=f(x<em>1,x</em>2,\ldots,x<em>n) = x</em>1^2 %2B x<em>2^2 %2B \cdots %2B x</em>n^2" style="border:none;">

##### 如何输入矢量

例子：\vec{a} \cdot \vec{b}=0

显示： 
<img src="http://chart.googleapis.com/chart?cht=tx&chl=\vec{a} \cdot \vec{b}=0" style="border:none;">

##### 如何输入积分

例子：\int_0^1 x^2 {\rm d}x

显示： <img src="http://chart.googleapis.com/chart?cht=tx&chl=\int_0^1 x^2 {\rm d}x" style="border:none;">

##### 如何输入极限运算

例子：\lim_{n \rightarrow +\infty} \frac{1}{n(n+1)}

显示： <img src="http://chart.googleapis.com/chart?cht=tx&chl=\lim_{n \rightarrow %2B\infty} \frac{1}{n(n%2B1)}" style="border:none;">

##### 如何输入累加、累乘运算

例子：\sum{i=0}^n \frac{1}{i^2}　和　\prod{i=0}^n \frac{1}{i^2}

显示： <img src="http://chart.googleapis.com/chart?cht=tx&chl=\sum{i=0}^n \frac{1}{i^2}" style="border:none;"> 　和　 <img src="http://chart.googleapis.com/chart?cht=tx&chl=\prod{i=0}^n \frac{1}{i^2}" style="border:none;">

##### 如何输入希腊字母

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\alpha|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\alpha" style="border:none;">|\beta|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\beta" style="border:none;">|
|\gamma|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\gamma" style="border:none;">|\delta|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\delta" style="border:none;">|
|\epsilon|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\epsilon" style="border:none;">|\varepsilon|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\varepsilon" style="border:none;">|
|\zeta|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\zeta" style="border:none;">|\eta|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\eta" style="border:none;">|
|\theta|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\theta" style="border:none;">|\vartheta|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\vartheta" style="border:none;">|
|\iota|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\iota" style="border:none;">|\kappa|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\kappa" style="border:none;">|
|\lambda|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\lambda" style="border:none;">|\mu|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\mu" style="border:none;">|
|\nu|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\nu" style="border:none;">|\xi|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\xi" style="border:none;">|
|\pi|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\pi" style="border:none;">|\varpi|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\varpi" style="border:none;">|
|\rho|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\rho" style="border:none;">|\varrho|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\varrho" style="border:none;">|
|\sigma|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\sigma" style="border:none;">|\varsigma|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\varsigma" style="border:none;">|
|\tau|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\tau" style="border:none;">|\upsilon|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\upsilon" style="border:none;">|
|\phi|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\phi" style="border:none;">|\varphi|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\varphi" style="border:none;">|
|\chi|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\chi" style="border:none;">|\psi|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\psi" style="border:none;">|
|\omega|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\omega" style="border:none;">|

##### 如何输入其它特殊字符

关系运算符

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\pm|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\pm" style="border:none;">|\times|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\times" style="border:none;">|
|\div|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\div" style="border:none;">|\mid|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\mid" style="border:none;">|
|\nmid|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\nmid" style="border:none;">|\cdot|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\cdot" style="border:none;">|
|\circ|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\circ" style="border:none;">|\ast|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\ast" style="border:none;">|
|\bigodot|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigodot" style="border:none;">|\bigotimes|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigotimes" style="border:none;">|
|\bigoplus|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigoplus" style="border:none;">|\leq|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\leq" style="border:none;">|
|\geq|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\geq" style="border:none;">|\neq|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\neq" style="border:none;">|
|\approx|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\approx" style="border:none;">|\equiv|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\equiv" style="border:none;">|
|\sum|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\sum" style="border:none;">|\prod|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\prod" style="border:none;">|
|\coprod|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\coprod" style="border:none;">|

集合运算符

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\emptyset|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\emptyset" style="border:none;">|\in|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\in" style="border:none;">|
|\notin|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\notin" style="border:none;">|\subset|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\subset" style="border:none;">|
|\supset|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\supset" style="border:none;">|\subseteq|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\subseteq" style="border:none;">|
|\supseteq|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\supseteq" style="border:none;">|\bigcap|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigcap" style="border:none;">|
|\bigcup|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigcup" style="border:none;">|\bigvee|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigvee" style="border:none;">|
|\bigwedge|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigwedge" style="border:none;">|\biguplus|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\biguplus" style="border:none;">|
|\bigsqcup|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bigsqcup" style="border:none;">|

对数运算符

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\log|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\log" style="border:none;">|\lg|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\lg" style="border:none;">|
|\ln|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\ln" style="border:none;">|

三角运算符

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\bot|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\bot" style="border:none;">|\angle|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\angle" style="border:none;">|
|30^\circ|<img src="http://chart.googleapis.com/chart?cht=tx&chl=30^\circ" style="border:none;">|\sin|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\sin" style="border:none;">|
|\cos|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\cos" style="border:none;">|\tan|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\tan" style="border:none;">|
|\cot|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\cot" style="border:none;">|\sec|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\sec" style="border:none;">|
|\csc|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\csc" style="border:none;">|

微积分运算符

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\prime|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\prime" style="border:none;">|\int|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\int" style="border:none;">|
|\iint|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\iint" style="border:none;">|\iiint|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\iiint" style="border:none;">|
|\iiiint|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\iiiint" style="border:none;">|\oint|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\oint" style="border:none;">|
|\lim|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\lim" style="border:none;">|\infty|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\infty" style="border:none;">|
|\nabla|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\nabla" style="border:none;">|

逻辑运算符

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\because|**未找到**|\therefore|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\therefore" style="border:none;">|
|\forall|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\forall" style="border:none;">|\exists|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\exists" style="border:none;">|
|\not=|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\not=" style="border:none;">|\not>|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\not>" style="border:none;">|
|\not\subset|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\not\subset" style="border:none;">|

戴帽符号

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\hat{y}|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\hat{y}" style="border:none;">|\check{y}|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\check{y}" style="border:none;">|
|\breve{y}|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\breve{y}" style="border:none;">|

连线符号

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\overline{a+b+c+d}|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\overline{a+b+c+d}" style="border:none;">|\underline{a+b+c+d}|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\underline{a+b+c+d}" style="border:none;">|
|\overbrace{a+\underbrace{b+c}<em>{1.0}+d}^{2.0}|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\overbrace{a+\underbrace{b+c}{1.0}+d}^{2.0}" style="border:none;">|

箭头符号

|符号|值|符号|值|
|:---:|:---:|:---:|:---:|
|\uparrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\uparrow" style="border:none;">|\downarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\downarrow" style="border:none;">|
|\Uparrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Uparrow" style="border:none;">|\Downarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Downarrow" style="border:none;">|
|\rightarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\rightarrow" style="border:none;">|\leftarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\leftarrow" style="border:none;">|
|\Rightarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Rightarrow" style="border:none;">|\Leftarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Leftarrow" style="border:none;">|
|\longrightarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\longrightarrow" style="border:none;">|\longleftarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\longleftarrow" style="border:none;">|
|\Longrightarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Longrightarrow" style="border:none;">|\Longleftarrow|<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Longleftarrow" style="border:none;">|
