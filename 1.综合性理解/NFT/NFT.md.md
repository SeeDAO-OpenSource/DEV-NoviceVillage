# ERC721 - NFT 非同质化代币
本篇主要讲解ERC721库和NFT的点点滴滴

#历史

首先我们要从历史开始了解，为什么会有这个协议，以及这个协议的基础

关键词：**2018**， **EIP-721**

### 协议网址

https://eips.ethereum.org/EIPS/eip-721

### 协议宗旨

ERC-721是基于**EIP-721**提案完善的代码标准。EIP-721提案是William Entriken, Dieter Shirley, Jacob Evans, Nastassia Sachs
在2018年提出的标准，宗哲是在已有的ERC20的基础上创造一个独立的代币体系。

在本提案中，他们提及“NFT可以是作为虚拟或是实体资产的所有权证明”，也就是我们现在所说的万物皆可NFT。
同时他们也给出了一些用例：
- 实体资产: 房屋，艺术品
- 虚拟收集品: 独特的图片，收集交换式卡片（TCG）
- 负资产: 贷款，负担和其他责任

# 具体实现

我们会以OpenZeppelin的ERC721库为源码进行解析和之后的操作

[协议具体地址](https://docs.openzeppelin.com/contracts/4.x/api/token/erc721)

### 结构

#### EIP接口

- IERC721
- IERC721Metadata
- IERC721Enumerable
- IERCReceiver

#### OpenZeppelin接口

- ERC721
- ERC721Enumerable
- ERC721Holder

#### 其他接口

- ERC721URIStorage
- ERC721Votes
- ERC721Royalty
- ERC721Pausable
- ERC721Burnable

从结构来看我们可以看到，所有的接口都是基于EIP提案提出的4个接口为基础
扩建出来的，接下来我们会去拆解主要的架构。

## IERC721

### 导入语句

```import "@openzeppelin/contracts/token/ERC721/IERC721.sol";```

### 具体方法解析

```balanceOf(owner)```:

balanceOf是一个很方便的内置功能主要帮助我们查询用户的tokens数量
也就是说我们需要传入一个```address```值，然后这个函数会返还一个
```uint256```类别的余额值。

```ownerOf(tokenId)```:

ownerOf就如同字面意思，帮助我们透过```tokenId```去查询持有者的地址
也就是说我们传入一个```uint256```的```tokenId```，这个函数会返还
一个```address```给我们。

#### 常见用例:

一个比较常见的用法是结合require语句去进行条件判断。下面我们通过```ownerOf```
去判定交互用户是否为token持有者。

```require(ownerOf(tokenId) == msg.sender, "You are not the owner of the token");```



## NFT实战