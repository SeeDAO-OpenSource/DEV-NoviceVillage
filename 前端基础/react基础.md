## 前言

hello, 大家好。 这里是SeeDAO的新手村教学，很高兴又见面啦。在前面的教学中，相信小伙伴们了解并知道了web3的架构，git，node环境的配置,VScode, Javascript等基础知识。接下来，在本节中，我会从自己的理解来讲解**react**的使用，以及**ChakraUI**库的使用以及**Ethereum与页面的交互**。

---

## 学习react前需要掌握的知识


提到React, 就不得不提到前端的基础三剑客 - HTML（超文本标记语言）, CSS（层叠样式表）,和 Javascript （程序语言，以下简称JS）。React本身呢，其实是一个前端 UI 开发框架，方便 ~~偷懒（大雾）~~ 提升大家的编程的效率。


那么在开始之前的，需要小伙伴门自学一下 HTML, CSS。对于初出茅庐的开发者来说的话，如果来做对比的话，HTML可以理解为网站的骨架，CSS则是美丽的外表，JS是最后决定这个网站是否有趣的灵魂。关于新手向的HTML以及CSS教学，市面上有很多，在这里就不重复写教程了 ~~（毕竟这是一篇面向react开发的新手教学）~~。

这其中，强烈推荐小伙伴们阅读MDN（ps:MDN是mozilla旗下的网站，火狐浏览器就是他们家的，有兴趣的同学可以阅读它的历史，在此respect）关于[HTML](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML)，[CSS](https://developer.mozilla.org/zh-CN/docs/Learn/CSS)的教学。

---

## 从原生html, css, js 过渡到react的初体验

### 组件化

下面，让我们正式开始从原生写页面到使用react框架的教学。设想一下，当我们使用原生语言码完一个网站后。如果此时甲方爸爸希望我们再写一个页面的话，并且该页面使用到之前的页面UI的时候（例如相同的导航栏，尾栏等等），我们将有三个选择：

1. 从头到尾，将这个新的页面，重新码一次。
2. 将之前页面使用到的UI代码复制一份，然后粘贴到新的页面中。
3. ~~提桶跑路。~~

正常在使用原生写页面时，我们都会使用2的方式来 ~~偷懒~~  节约时间。这就是所谓前端**组件化**的目的，也是为了我们要使用react框架来搭建我们想要搭建的页面的原因。

下面让我们先创建一个**单页面React 应用 （single page app in react）**，实现一个基础的导航栏的复用。

1.  打开vscode，并打开命令行（terminal）。输入指令
 `npx create-react-app my-app` 后回车，开始创建react项目。（注：1. 电脑需要安装node环境，否则无法下载对应的依赖。 2.输入指令时请确保路径的位置。 3. [npx](http://nodejs.cn/learn/the-npx-nodejs-package-runner) 是Node.js包运行器 ， create-react-app 代表我们将使用名字为 create-react-app的模版， my-app 指我们这个项目的名字。）
 
 
![create-project](https://i.imgur.com/NKQl2Vq.png)

2. 第一次安装时，会被询问是否下载依赖包，输入 `y` 回车然后继续：

![enter-y](https://i.imgur.com/zcmYfrI.png)


3. 等待片刻后，待命令行安装停止，可以看到左侧有一个my-app的文件夹，里面就是我们创建的一个react项目啦。

![project-created](https://i.imgur.com/nPiqRpC.png)

4. 输入`cd my-app`指令并回车，来进入项目文件夹中，然后再在命令行中输入 `npm start` 回车来运行react程序。

![start-project](https://i.imgur.com/KsfmPeE.png)

5. 稍等片刻之后可以看到，提示 *Compiled successfully!* ，此时浏览器自动打开并进入react 项目中。（若浏览器没有启动，则在浏览器中输入[http://192.168.0.38:3000](http://192.168.0.38:3000) 来查看页面效果 ）

![started](https://i.imgur.com/Ey4WhUx.png)

![project-view](https://i.imgur.com/AbZGjcy.png)

6. 下面开始创建一个简单的导航栏，在开始之前先对react的项目内等下我们会接触的一些文件夹和文件做一下简单的介绍。

![file-list](https://i.imgur.com/xR5g7I3.png)

├── node_modules                    所有的依赖安装的目录
├── public                                静态公共目录
└── src                                        开发用的源代码目录
├── .gitignore            帮助我们在git add时将我们指定的一些文件自动排除在外，不提交到git当中。
├── package-lock.json            锁定安装时的包的版本号,保证团队的依赖一致。
├── package.json            记录了当前项目信息,例如项目名称、版本、作者、github地址、 当前项目依赖了哪些第三方模块等。
└── README.md                            使用方法的文档

由此可见我们需要在src文件夹中进行代码的开发和整理。

7. 让我们打开src文件加并点击App.js文件。

![app.js](https://i.imgur.com/A6kJzBI.png)

在这里，让我们先忽视第一行和第二行的import（熟悉ES6的小伙伴应该很容易了解这是一个JS文件中引入另一个JS）。在第六行到底21行，写过html和css的小伙伴是否会觉得很熟悉呢？这就是我们在首页看到的内容。

下面让我们改动上面的代码，做一个简单的导航栏：

8. 在src文件下右键点击`New File`.

![create-file](https://i.imgur.com/33Z4aun.png)

9. 给创建的文件命名为`Nav.js`

![reaname](https://i.imgur.com/8utZ2vy.png)


10. 在Nav.js中输入以下代码：

```javascript=
import React from "react";

const Nav = () => {
  return (
    <div style={{ display: "flex",justifyContent:"center" }}>
      <a href="/" style={{ textDecoration: "none", color: "#000" ,width:"3rem",textAlign:"center"}}>
        首页
      </a>
      <a href="/blog" style={{ textDecoration: "none", color: "#000",width:"3rem",textAlign:"center" }}>
        博客
      </a>
      <a href="/about" style={{ textDecoration: "none", color: "#000",width:"3rem",textAlign:"center" }}>
        关于
      </a>
    </div>
  );
};

export default Nav;

```

![Nav.js](https://i.imgur.com/pWBYHHy.png)

大家可以看到，这部分关于css的使用与我们使用原生css和html写代码是不一样的，具体解释我们会在下一部分JSX模板做详细的解释。


11. 返回App.js文件，删除部分代码：

![](https://i.imgur.com/K2u6wTi.png)

12. 在第一行输入 下面的代码引入我们刚才做好的导航栏文件

```javascript=
import Nav from './Nav'
```

在第五行，两个div之间中间输入下面的代码实现导航栏的使用和复用
```javascript=
<Nav></Nav>
<Nav></Nav>
```

![App.js](https://i.imgur.com/Dn22cz7.png)


13. 保存文件，同时查看浏览器。可以发现，下周页面中出现了两个导航栏。

![reuse-nav](https://i.imgur.com/qvdcb8J.png)

#### 总结： 恭喜你，现在你已经成功学会了如何依靠react实现 ~~偷懒~~ 组件的复用啦。 是不是同原生html，css的复制+粘贴的操作相比，react的组件（代码）复用更加方便呢？下面一章节，我会大概介绍为何react中关于css写法不同的原因。


---

### JSX

在上一章节中，我们简单介绍了组件的的复用，那么结合上一章节的疑问，本章节会跟大家简单的讨论以下为何React中关于CSS写法不同的原因。

众所周知，React是一个渲染UI的JS库。为了实现组件的复用，React使用JSX ~~（ps：我个人理解为一种HTML模板，模板是什么意思相信大家都懂）~~ 。它是一种JavaScript的语法扩展，全称JavaScript XML。**实际上，浏览器虽然能读懂JavaScript，但是它却无法理解JSX。** 为了让浏览器能够读懂JSX, React使用了JSX转换器（Babel）将JSX文件转换为 JavaScript对象，然后再将其传给浏览器。

那么为何React要选择JSX呢？ 这是因为React需要通过JSX来创建创建虚拟DOM。相信在HTML，JS的课程中小伙伴们都清楚了什么是DOM树形结构。实际上React并没有直接在DOM上进行任何的代码操作。（相信以前使用原生的你一定难以忘记重复DOM操作来更新页面的繁琐吧, 有兴趣的同学可以阅读这篇文章[[react] 什么是虚拟dom？](https://zhuanlan.zhihu.com/p/541599268)） 而是通过虚拟DOM转成真实DOM并插入页面中（render）来实现页面的渲染的。

因此，相比于原生的写法，JSX在写法上有一些细微的不同。

除了上面的例子，我们甚至可以讲html的模板注入到一个变量中，是不是很有意思呢？有了JSX，我们就能利用它的特性，来做很多很有意思的东西。欢迎大家阅读[JSX入门](https://zhuanlan.zhihu.com/p/26413299)。

![value](https://i.imgur.com/k3Tc58k.png)

#### 关于h function

---

### 单页面应用

在前面的教学之前，有细心的小伙伴有留意到，我提到了React的时候一定伴随着**单页面应用**,这五个字。抛开应用这两个字，这里的单页面又是什么意思呢？

让我们回到之前的React项目中,打开public文件夹，映入眼帘的就是我们熟悉的**index.html**文件啦。以前使用过HTML,CSS原生的小伙伴一定对index.html不会感到陌生。因为它就是一个网站的首页。在没接触前端框架之前，为了完成一个多个页面的网站，我们需要创建多了以.HTML为后缀的文件，来代表这是哪一个页面。这就是多页面应用啦。

![index.html](https://i.imgur.com/OVGrUmd.png)

但是在React中，当你翻遍所有的文件夹，你就只能看到项目中唯一的html文件（当然这不是绝对的，在某些react模板中会隐藏该文件），也就是说所有的页面都是在这一个HTML文件上展示的。这样也就大大的提升了页面加载的速度。假设目前我们在首页上，那么只有首页的内容会浏览器显示出来，其他的页面的内容会被“隐藏”起来。当你进入到关于页面时，那么首页上的内容就被“隐藏"起来，而关于页面上的内容就被显示出来。实现了组件之间的切换，实现了资源的局部刷新，对比多页面应用，速度当然会快上很多，那么体验也会更好。具体内对比和介绍，欢迎大家阅读[单页应用和多页应用的区别](https://juejin.cn/post/6877370557155934222)。

俗话说得好，天下武功，唯快不破。但是为了快，React也放弃了很多。一个最明显的不足就是，React对于SEO并不是十分的友好。那么什么时SEO呢？SEO是一个很宽泛的话题,怕是十天十夜都讲不完。 ~~（其实是我水平有限... = =）~~ 因此为了方便新人小伙伴的了解。这里举一个 ~~栗子~~ 例子。假如说有一天，你帮甲方爸爸官网的需求用React搭建好，好不容易上线了。过了很久很久，当甲方爸爸拿着~~百度~~谷歌问你：“我在这上头搜不到我的关于页面呀？” 千万不要提桶走人，一定要勇敢的回答他：“因为为了提升网站的速度，我们使用了React搭建了单页面应用。所以谷歌上的搜索引擎记录不到我们的关于页面。”
然后你就会收获~~离职~~表扬。

那么为什么搜索引擎记录不到关于页面呢？让我们回到之前的React项目的index.html

![meta](https://i.imgur.com/2yF8eR1.png)

让我们看到第8~第11行，让我们一起回顾一下html中的[meta](https://www.w3schools.com/tags/tag_meta.asp)标签。

> 该<meta>标签定义有关 HTML 文档的元数据。元数据是关于数据的数据（信息）。

> <meta>标签始终位于 <head> 元素内，通常用于指定字符集、页面描述、关键字、文档作者和视口设置。

> 元数据不会显示在页面上，但机器可解析。

> 元数据由浏览器（如何显示内容或重新加载页面）、搜索引擎（关键字）和其他 Web 服务使用。

> 有一种方法可以让网页设计师通过<meta>标签控制视口（网页的用户可见区域）（参见下面的“设置视口”示例）。
    
Bingo, 这就是为什么单页面应用对于SEO不友好的原因。对比多页面应用，单页面应用只有一个html文件！因此不方便搜索引擎找寻关键词。因此在制作SEO需求的网站时，一定要注意是否使用React的来来搭建单页面应用。
    
那么，如果你非要使用React ~~（React大法好）~~，但是又要SEO优化 ~~（我全都要）~~ ，怎么办呢？ 这里欢迎大家了解[Gatsby.js](https://www.gatsbyjs.com/)(这个坑有时间补一下),或是关注我的[gatsbyjs安利视频](https://www.bilibili.com/video/BV1Nz411B7Ed?share_source=copy_web&vd_source=1809c13c520c30c0d95513a2138bc275)，谢谢大家。

---
    
## 小结
    
在这一部分中，我们简单介绍了React组件的复用，和JSX以及什么是单页面应用。你以为这就完了么？ NO NO NO，这只是React庞大的生态中的一点点星光罢了。后面我们会跟随Web3项目开始介绍更有意思的东西，让我们拭目以待！ - Chao from [Obsidian Labs](https://www.obsidians.io/)