title: babel
speaker: xiongyang
prismTheme: dark
plugins:

    - echarts

<slide class="aligncenter">

# babel {.text-landing.text-shadow}

By xiongyang {.text-intro}

[:fa-github: Github](https://github.com/ksky521/nodeppt){.button.ghost}

:::note
babel\`beibl\
大家好今天给大家分享一下我对babel一个初步的理解，有什么疑问和不对的地方希望大家指出来一起讨论
:::

<slide class="aligncenter">

# What is babel {.text-landing}

:::

<slide class="fullscreen">

:::card

![](http://image.gxx365.com/babel.jpeg)

---

Babel的原意就是指人类为了见到上帝，准备建造的一座通天塔，上帝为了阻止人类，让他们语言变的不同。于是，人类开始有了文化差异，从而导致了冲突，塔也就建不起来了……
==旧约圣经==

:::

:::note
babel \`beibl\ 的相关历史：
我第一次打开搜索引擎查询关于 Babel 的资料时，出现的竟然是关于 Babel 的传说还有电影
是巴比伦文化里的通天塔，Babel的原意就是指人类为了见到上帝，准备建造一座通天塔，上帝为了阻止人类，让他们语言变的不同。于是，人类开始有了文化差异，从而导致了冲突，当我们明白了babel的作用再回来看这个项目命名的的时候回发现这个名字起的是是非常的贴切

了解完这个神话之后，下面就是正题

在我们前端，什么是babel
:::

<slide class="aligncenter">

{.text-subtitle}
## 什么是 Babel

### Babel is a JavaScript compiler {.tobuild.fadeInLeft}

### Babe居然‘认识’代码、更改代码。就是操作代码的代码 {.tobuild.fadeInLeft}

[:fa-github: Github](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&spec=false&loose=false&code_lz=BQSgBAvAfGDeBQBIAxgewHYGdUBsCmAdDqgObADkmm5I8AvkA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Ces2015%2Creact%2Cenv&prettier=false&targets=&version=7.10.2&externalPlugins=){.button.ghost}
:::note
没错就一句话，Babel 是一个 JS 的编译器,有很多优秀的比如es6,es7新语法都不能直接在浏览器中运行, 各个浏览器对 JS 版本的支持各不相同。为了解决这个“沟通不畅”的问题，所以就有了 Babel，

Babel通过转换，让我们写新版本的语法，转换到低版本，这样就可以在只支持低版本语法的浏览器里运行了。比如说 常见的箭头符号，class, promise，async,await 等等

它居然‘认识’代码、更改代码。就是操作代码的代码

我们今天就来看一下babel的这一系列处理过程：
:::

<slide :class="size-60">

## Babel的处理步骤

---

:::shadowbox

## 解析

通过词法分析和语法分析，将JS源代码转换成抽象语法树

---

## 转换

对语法树（AST）进行转换操作，把ES2015+的部分转换为ES5

---

## 生成

将转换之后的语法树重新生成代码

:::

:::note
babel是如何将es6+的新语法转化成低版本浏览器能够识别的语法呢？
主要有下面这三个阶段：

1. 解析（parse）-- 将源代码变成抽象语法树AST
2. 转换（transform）-- 操作AST，这也是我们可以通过配置插件去操作的部分
3. 生成（generate）-- 将更改后的AST，再变回代码。

注意 babel 本身不具有任何转化功能，它把转化的功能都分解到一个个 plugin 里面。因此当我们不配置任何插件时，经过 babel 的代码和输入是相同的

我们来看一下babel的基本配置
:::

<slide :class="size-80">

:::column {.vertical-align}
### **Babel的基本配置**

- #### plugins：transform 的载体
- #### presets：提前定义好了一系列的转换插件
  - @babel/preset-env
  - @babel/preset-flow
  - @babel/preset-react
  - @babel/preset-typescript

----
```js
module.exports = function (api) {
    api.cache(true);
    const presets = ["env"];
    const plugins = [
        "@babel/plugin-transform-arrow-functions",
        "@babel/plugin-transform-classes"
    ];

    // 单测需要转一下es modules至commonjs
    if (process.env.NODE_ENV === 'test:unit') {
        plugins.push('@babel/plugin-transform-modules-commonjs');
    }

    return {
        presets,
        plugins
    };
};
```
:::

---

:::note
Babel 支持多种的配置方式,
- babel.config.js
- .babelrc
- .babelrc.js
- package.json

如果希望以编程的方式创建配置文件,那么 babel.config.js 可以满足你的需求，还有几种这里就不过多介绍

babel里面主要分成两大块：
plugins 和 presets

这里配置了箭头函数和 class 的 转换插件，这样就可以在代码里面使用它们。但是实际开发过程中，如果需要一个一个插件的手动去添加，会影响效率，并且我们可能也不清楚我们的浏览器环境到底需要哪些 转换插件，
为此 babel 为我们提供了 presets （预设） 就是 plugins 的组合，你也可以理解为是套餐
官方提供了如下几个
@babel/preset-env
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript

介绍完基本的配置后我们来看下babel是如何进行解析和转换的这样一套相关的处理流程

:::

<slide class="fullscreen">

:::card

![](https://user-gold-cdn.xitu.io/2019/10/2/16d8d0cd559c7e1e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

---

### Babel的处理流程

[:fa-github: Github](https://astexplorer.net/#/Z1exs6BWMq){.button.ghost}
[:fa-github: Babel](https://astexplorer.net/#/Z1exs6BWMq){.button.ghost}
:::

:::note
babel \`beibl\ 的处理流程
如图为babel的一个处理流程：
分别为：
下面我们对应每一个步骤说一下babel都是怎么操作的：
:::

<slide>
:::{.content-center}
## 解析 (Parsing)

将代码解析成抽象语法树（AST），每个js引擎（比如Chrome浏览器中的V8引擎）都有自己的AST解析器，而Babel是通过Babylon实现的

- ### 词法分析
- ### 语法解析

:::

:::note

1. 代码解析 (Parsing)
将代码解析成抽象语法树（AST），每个js引擎（比如Chrome浏览器中的V8引擎）都有自己的AST解析器，而Babel是通过Babylon实现的。

在解析过程中有两个阶段：词法分析和语法分析，词法分析阶段把字符串形式的代码转换为令牌（tokens）流，令牌类似于AST中节点；而语法分析阶段则会把一个令牌流转换成 AST的形式，同时这个阶段会把令牌中的信息转换成AST的表述结构。

Babel中的解析器是babylon。我们来体验一下：

和我们预期的一样，得到AST了。这里我注意到还有start、end、loc这样位置信息的字段，应该可以对生成Source Map有用的字段。

<!-- 我们可以通过这个网站来感受下AST
它就是一棵'对象树'，用来表示代码的语法结构
AST 是 Babel 转译的核心数据结构，后续的操作都依赖于 AST -->
:::

<slide>
:::{.content-center}
## 转换（Transform）

在这个阶段，Babel接受得到AST并通过babel-traverse对其进行深度优先遍历，在此过程中对节点进行添加、更新及移除操作。这部分也是Babel插件介入工作的部分

:::

:::note
代码转换（Transform）

在这个阶段，Babel接受得到AST并通过babel-traverse对其进行深度优先遍历，在此过程中对节点进行添加、更新及移除操作。这部分也是Babel插件介入工作的部分

就是操作 AST
在 Babel 中我们使用者最常使用的地方就是代码转换, 大家常用的 Babel 插件就是定义代码转换规则而生的,而代码解析和生成这一头一尾都主要是 Babel 负责
我们转换代码的关键就是根据当前的抽象语法树,以我们定义的规则生成新的抽象语法树,转换的过程就是生成新抽象语法树的过程.
2.1 遍历抽象语法树
抽象语法树是一个树状数据结构,我们要生成新语法树,那么一定需要访问 AST 上的节点,因此我们需要一个工具来遍历抽象语法树的节点.
2.2 转换代码(实现转换器transformer)

得到ast了，该操作它了，Babel中的babel-traverse用来干这个事

<!-- traverse方法是一个遍历方法，path封装了每一个节点，并且还提供容器container，作用域scope这样的字段。提供个更多关于节点的相关的信息，让我们更好的操作节点。 -->
确实我们的ast被更改了，用这个ast生成的code就会是const alteredText = 'Hello World';

:::

<slide>
:::{.content-center}
## 生成(Generator)

经过上面两个阶段，需要转译的代码已经经过转换，生成了新的AST，最后一个阶段就是根据这个AST来输出代码

:::

:::note

得到操作后的ast，下一步该生成新代码了。Babel中的babel-generator用来干这个事

在code字段里，我们看到里新生成的代码
上面提到的这四个库，还有更多细节，有兴趣的可以再研究研究
这些库的集合，就是我们的Babel

:::
<slide image="https://webslides.tv/static/images/satya.png .left-bottom">

:::div {.content-right}

> "There is something only a CEO uniquely can do, which is set that tone, which can then capture the soul of the collective."
> ==Satya Nadella, CEO of Microsoft.==

:::

<slide class="bg-black" video="https://webslides.tv/static/videos/working.mp4 poster='https://webslides.tv/static/images/working.jpg'" >

`.background-video` 

## **WebSlides is the easiest way to make HTML presentations. Inspire and engage.**

<slide class="bg-black aligncenter" image="https://source.unsplash.com/UJbHNoVPZW0/ .dark">

# Iceland{.text-landing.text-shadow}

`slide[class*="bg-"] > .background.dark` 
