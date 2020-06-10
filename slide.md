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
大家好今天给大家分享我对babel一个初步的理解，有什么疑问的地方大家可以一起探讨
:::

<slide class="aligncenter">

# What is babel {.text-landing}

:::

<slide class="fullscreen">

:::card

![](http://image.gxx365.com/babel.jpeg)

---

Babel的原意指得是人类为了见到上帝，准备建造的一座通天塔(babel)，上帝为了阻止人类，让他们语言变的不同。于是，人类开始有了文化差异，从而导致了冲突，塔也就建不起来了……
==旧约圣经==

:::

:::note
babel \`beibl\ 的相关历史：
我第一次打开搜索引擎 查询关于Babel的资料时，出现的了很多关于 Babel 的传说还有电影
Babel的原意 指得是人类为了见到上帝，准备建造一座通天塔，上帝为了阻止人类，让他们语言变的不同。于是，人类开始有了文化差异，从而导致了冲突，塔也就建不起来了……

当我们明白了babel的作用 再回来看这个项目命名的的时候 会发现这个名字起的是非常贴切

了解完这个神话之后，下面来说在我们前端的babel
:::

<slide class="aligncenter">

{.text-subtitle}

## 什么是 Babel

### Babel is a JavaScript compiler {.tobuild.fadeInLeft}

### Babe居然‘认识’代码、更改代码。就是操作代码的代码 {.tobuild.fadeInLeft}

[:fa-github: Babel](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&spec=false&loose=false&code_lz=BQSgBAvAfGDeBQBIAxgewHYGdUBsCmAdDqgObADkADgIboAm15I8AvgNzzxpYAuY1kMAEY2QA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Ces2015%2Cenv&prettier=false&targets=&version=7.10.2&externalPlugins=){.button.ghost}

:::note

没错就一句话，Babel是一个现代JS编译器, 有很多**优秀的es6,es7新语法**都不能直接在**支持低版本语法的浏览器**中运行, 各浏览器对JS版本的支持各不相同。为了解决这个“沟通不畅”的问题，所以就有了 Babel，Babel 的出现使得我们可以无须顾忌的去使用新的的语法

我们可以通过这个网站来看一下babel的转换

我们会发现 babel 可以认识并且更改代码
 
我们传递一段源代码给 Babel，然后它返回一串新的代码给我们，它不会运行我们的代码，也不会去打包我们的代码

我们看一下babel在这中间做了什么处理？

:::

<slide :class="size-60">

## Babel的处理步骤

---

:::shadowbox

## 解析

通过词法分析和语法分析，将JS源代码转换成抽象语法树AST

---

## 转换

对语法树AST进行转换操作，把ES2015+的部分转换为ES5

---

## 生成

将转换之后的语法树重新生成代码

:::

:::note

babel的处理步骤主要有下面这三个：

1. 解析（parse）-- 将源代码变成抽象语法树AST
2. 转换（transform）-- 操作AST，把es6/es7等新语法的部分转换为ES5, 这部分我们可以通过配置bebel插件去操作
3. 生成（generate）-- 将更改后的AST，再变回JS代码

注意babel本身不具有任何转化功能，它把转化的功能都分解到插件里面。因此当我们不配置任何插件时，经过babel的代码和输入是相同的

我们可以来看一下babel的基本配置
:::

<slide :class="size-80">

:::column {.vertical-align}
### **Babel的基本配置**

- #### plugins：transform 的载体
- #### presets：预设，一系列plugins的组合
  - @babel/preset-env
  - @babel/preset-flow
  - @babel/preset-react
  - @babel/preset-typescript

----
```js
// babel.config.js
module.exports = function (api) {
    api.cache(true);
    const presets = ["@babel/preset-env"];
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

如果希望以编程的方式创建配置文件, 那么**babel.config.js**可以满足我们的需求，其他几种这里就不过多介绍

babel配置里面主要分成两大块：
plugins 和 presets

这里配置了**箭头函数**和**class**的转换插件，这样就可以在代码里面使用它们。
但是如果在实际开发过程中，每一个插件都需要我们手动去添加，会影响效率，
为此 babel 为我们提供了presets**预设**就是plugins 的组合，你也可以理解为是套餐

官方提供了如下几个
@babel/preset-env 推荐使用的
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript

介绍完基本的配置后我们来看下babel是如何进行解析和转换的这样一套相关的操作

:::

<slide class="fullscreen">

:::card

![](https://user-gold-cdn.xitu.io/2019/10/2/16d8d0cd559c7e1e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

---

### Babel的处理流程

- AST

[:fa-github: AST](https://astexplorer.net/#/Z1exs6BWMq){.button.ghost}
:::

:::note
babel \`beibl\ 
如图为babel的一个处理流程：
分别与前面的处理步骤对应起来

1. 解析：对源代码词法分析和语法分析
2. 转换：遍历AST并应用转换器增删改查AST节点
3. 生成：将更改后的AST转换为源代码

我们发现Babel插件就是作用于AST上进行的，

其实在我们开发中例如：代码压缩、代码高亮，css预处理器、eslint等其实都是基于AST来实现的，所以还是很有必要的了解一下AST

AST是什么，我们可以先通过这个网站来感受一下

<!-- 我们可以通过这个网站来感受下AST
它就是一棵'对象树'，用来表示代码的语法结构
AST 是 Babel 转译的核心数据结构，后续的操作都依赖于 AST -->
我从左边输入一个常见的赋值语句，右边就展示出了这个语句的AST，有Tree和JSON两种形式，里面分成来很多个节点来表示我们这样一个赋值语句，这些节点就构成一颗语法树

在某种层面上这两边是对等的

我们只要知道通过这样一个AST语法树就可以描述我们的JS代码，具体的对应规则这里不做过多解释，感兴趣的可以下来了解

下面我对应每一个阶段 说一下babel都是怎么操作的：
:::

<slide>
:::{.content-center}
## 一：解析 (Parsing)

将代码解析成抽象语法树（AST），每个js引擎（比如V8引擎）都有自己的AST解析器

- ### 词法分析
- ### 语法解析
  
### Babylon

:::

:::note

一：代码解析 (Parsing)
将代码解析成抽象语法树（AST），每个js引擎（比如V8引擎）都有自己的AST解析器。

在解析过程中有两个阶段：
词法分析和语法分析

词法分析: 可以看成是对代码进行“分词”，它接收一段源代码，执行一个函数把代码分割成被称为Tokens（令牌） ，令牌类似于AST中节点；
语法分析: 通过语法分析把 Tokens 转化为上面提到过的 AST

Babel中用的解析器是babylon/`baibilen/ 我们来体验一下：

和我们预期的一样，得到AST了。这里我注意到还有start、end、loc这样位置信息的字段，应该可以对生成Source Map有用的字段。

:::

<slide>
:::{.content-center}
## 二：转换（Transform）

在这个阶段，Babel接收得到AST并对其进行深度优先遍历，在此过程中对节点进行添加、更新及移除操作。这部分也是Babel插件介入工作的部分

### babel-traverse

:::

:::note
代码转换（Transform）

在这个阶段，Babel接收得到AST并对其进行深度优先遍历，在此过程中对节点进行添加、更新及移除操作。这部分也是Babel插件介入工作的部分

<!-- 我们转换代码的关键就是根据当前的AST,以我们定义的规则生成新的AST,转换的过程就是生成新AST的过程. -->

Babel中的babel-traverse就是用来做转换的,继续看下demo

<!-- traverse方法是一个遍历方法，path封装了每一个节点，并且还提供容器container，作用域scope这样的字段。提供个更多关于节点的相关的信息，让我们更好的操作节点。 -->
我们可以看到ast被更改了

:::

<slide>
:::{.content-center}
## 三：生成(Generator)

经过上面两个阶段，需要转译的代码已经经过转换，生成了新的AST，最后一个阶段就是根据这个AST来生成代码

### babel-generator

:::

:::note

经过上面两个阶段，需要转译的代码已经经过转换，生成了新的AST，最后一个步就是根据这个AST 来生成代码. Babel中的babel-generator对应这一步操作

在code字段里，我们可以看到里面新生成的代码

上面讲的这几个库，还有更多细节，有兴趣的同学可以再细入研究

最后 这些库的集合实现了的这一整套流程，就是我们的Babel

<slide class="bg-black aligncenter" image="https://source.unsplash.com/UJbHNoVPZW0/ .dark">

# Thanks{.text-landing.text-shadow}
