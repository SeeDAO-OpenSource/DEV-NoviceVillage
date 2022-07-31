# 基础环境搭建

整理者：0xRory

## Git

### 1.安装

- 安装地址

[Git - 下载 (git-scm.com)](https://git-scm.com/downloads)

- 详细安装流程

[(3条消息) Git 详细安装教程（详解 Git 安装过程的每一个步骤）_mukes的博客-CSDN博客_git安装](https://blog.csdn.net/mukes/article/details/115693833)

### 2.git的命令

- 常用命令

![git命令](https://github.com/boombb12138/DEV-NoviceVillage/blob/patch-1/git%E5%91%BD%E4%BB%A4.png)

- git所有命令列表

[Git - 参考 (git-scm.com)](https://git-scm.com/docs)

## Visual Studio Code

下载地址

[Visual Studio Code - Code Editing。重新](https://code.visualstudio.com/)

安装教程(0:00-2:00)

[『教程』VsCode五分钟上手教程 无一句废话_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1bK411P767?spm_id_from=333.337.search-card.all.click&vd_source=e4e8b43a42050142086778d5311ad07f)



## Node

#### window版

http://t.csdn.cn/1k1nN

http://t.csdn.cn/mtT2b

最后一步用管理员运行cmd还报错的看往这
Node.js Express安装报错总结
express 4.x版本之前 全局安装express 命令是 npm install express -g
express 4.x版本之后 全局安装express 命令是 npm install -g express-generator



#### Linux

##### Ubuntu

将以下命令复制并粘贴到终端中：

```
sudo apt update
sudo apt install curl git
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt install nodejs

```

#### 苹果操作系统

确保你已安装。否则，请遵循[这些说明](https://www.atlassian.com/git/tutorials/install-git)安装。`git`

在MacOS上有多种安装Node.js的方法。我们将使用 [Node 版本管理器（nvm）](http://github.com/creationix/nvm)。将以下命令复制并粘贴到终端中：

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.35.2/install.sh | bash
nvm install 16
nvm use 16
nvm alias default 16
npm install npm --global ## 将npm升级到最新版本

```




## cnpm/npm/yarn

- 通过npm安装模块时都是去国外的镜像下载的，有的时候由于网络原因会导致安装模块失败，好在阿里有团队维护国内镜像:<http://npm.taobao.org/> ，这个即为cnpm，上面有使用说明
- 其实更推荐大家用yarn，因为更快，安装地址：<https://yarnpkg.com/zh-Hans/docs/install>


安装了node之后就可以安装HardHat





## Hardhat

Hardhat是一个方便在以太坊上进行构建的任务运行器。使用它可以帮助开发人员管理和自动化构建智能合约和dApp的过程中固有的重复任务，以及轻松地围绕此工作流程引入更多功能。Hardhat还内置了Hardhat EVM，后者是为开发而设计的本地以太坊网络。它允许你部署合约，运行测试和调试代码。

#### 1. 概述

欢迎来到**Hardhat**的初学者指南，看看如何基于**哈德哈特**进行以太坊合约和dApp开发。

**Hardhat**是一个方便在以太坊上进行构建的任务运行器。使用它可以帮助开发人员管理和自动化构建智能合约和dApp的过程中固有的重复任务，以及轻松地围绕此工作流程引入更多功能。

**Hardhat**还内置了**Hardhat 网络**，**Hardhat 网络**是为开发而设计的本地以太坊网络。用来部署合约，运行测试和**调试代码**。

在本教程中，我们将指导你完成以下操作：

- 为以太坊开发设置Node.js环境
- 创建和配置 Hardhat 项目

要完成本教程，你应该能够：

- 编写 [JavaScript](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics)
- 打开 [终端](https://en.wikipedia.org/wiki/Terminal_emulator)
- 使用 [git](https://git-scm.com/doc)
- 了解 [智能合约](https://ethereum.org/learn/#smart-contracts) 基础知识
- 设置 [Metamask](https://metamask.io/)钱包

如果你不具备上述知识，请先了解一下

#### 2.安装hardhat

我们将使用npm 命令行安装**hardhat**。 NPM是一个Node.js软件包管理器和一个JavaScript代码库。
打开一个新终端并运行以下命令：

```
mkdir hardhat-tutorial 
cd hardhat-tutorial 
npm init --yes 
npm install --save-dev hardhat

```

> 提示：安装**Hardhat**将安装一些以太坊JavaScript依赖项，因此请耐心等待。

  至此Hardhat已经安装完成

  另外，[Visual Studio Code 的 Hardhat](https://hardhat.org/hardhat-vscode)是官方的 Hardhat 扩展，它将 Solidity 的高级支持添加到 VSCode 中。在VScode中也可以安装Hardhat插件

创建新的 Hardhat 工程

在安装**Hardhat**的目录下运行：

```
npx hardhat
```

使用键盘选择 “创建一个新的hardhat.config.js()”，然后回车。`Create an empty hardhat.config.js`

```
$ npx hardhat
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

Welcome to Hardhat v2.0.0

? What do you want to do? …
  Create a sample project
❯ Create an empty hardhat.config.js
  Quit

```

在运行**Hardhat**时，它将从当前工作目录开始搜索最接近的文件。 这个文件通常位于项目的根目录下，一个空的足以使**Hardhat**正常工作。`hardhat.config.js``hardhat.config.js`

#### 3.Hardhat 架构

**Hardhat**是围绕**task(任务)**和**plugins(插件)**的概念设计的。 **Hardhat **的大部分功能来自插件，作为开发人员，你[可以自由选择](https://hardhat.org/plugins/)你要使用的插件。

##### Tasks(任务)

每次你从CLI运行**Hardhat**时，你都在运行任务。 例如 正在运行任务。 要查看项目中当前可用的任务，运行。 通过运行，可以探索任何任务。`npx hardhat compile``compile``npx hardhat``npx hardhat help [task]`

> 提示：你可以创建自己的任务。 请查看[创建任务](https://hardhat.org/guides/create-task.html)指南。

##### Plugins(插件)

**Hardhat** 不限制选择哪种工具，但是它确实内置了一些插件，所有这些也都可以覆盖。 大多数时候，使用给定工具的方法是将其集成到**Hardhat**中作为插件。

在本教程中，我们将使用Ethers.js和Waffle插件。 通过他们与以太坊进行交互并测试合约。 稍后将解释它们的用法。 要安装它们，请在项目目录中运行：

```
npm install --save-dev @nomiclabs/hardhat-ethers ethers @nomiclabs/hardhat-waffle ethereum-waffle chai

```

将高亮行`require("@nomiclabs/hardhat-waffle")` 添加到你的`hardhat.config.js`中，如下所示：

```
require("@nomiclabs/hardhat-waffle");

/**
 * @type import('hardhat/config').HardhatUserConfig
 */
module.exports = {
  solidity: "0.7.3",
};

```

这里引入`hardhat-waffle`，因为它依赖于`hardhat-ethers`，因此不需要同时添加两个。



参考：

[Visual Studio Code - Code Editing。重新](https://code.visualstudio.com/)

[安全帽|入门Nomic基金会为专业人士提供的以太坊开发环境 (hardhat.org)](https://hardhat.org/hardhat-runner/docs/getting-started#quick-start)

[[译\] Hardhat 入门教程 | 登链社区 | 区块链技术社区 (learnblockchain.cn)](https://learnblockchain.cn/article/1356#%E5%AE%89%E8%A3%85%20Node.js)

