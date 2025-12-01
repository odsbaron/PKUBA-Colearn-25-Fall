# 使用 Geth 读取链上数据

本周目标：

- 学会用 Geth 的 Go 客户端从 RPC 节点读取链上信息
- 理解区块链底层数据结构（block、transaction、receipt）

这些是做 DApp、链上分析、合约调试等的基础。

# Part I - Geth 简介

*介绍一下背景信息：什么是以太坊客户端、节点、RPC，以及它们分别是什么作用。*

*然后再详细介绍一下 Geth。*

Geth（Go Ethereum）是以太坊最常用的客户端实现。它包含：

- 运行节点的命令行程序
- 一套 Go 客户端库

我们不会运行本地节点，而是使用客户端库配合 RPC 来读取链上数据。

# Part II - Go 语言环境准备

*本周需要用 Go 编写与节点交互的简单程序。本节提供必要的最小知识。*

## 1. 安装 Go

官方下载：https://go.dev/dl/

检查安装：

```
go version
```

## 2. 新建项目

```
mkdir week3-geth && cd week3-geth
go mod init week3-geth
```

创建 `main.go`：

```
package main

import "fmt"

func main() {
    fmt.Println("hello go")
}
```

运行：

```
go run main.go
```

## 3. 安装 go-ethereum 库

```
go get github.com/ethereum/go-ethereum
```

# Part III - 使用 go-ethereum 读取链上数据

*验证一下程序及输出*

引入客户端：

```
import "github.com/ethereum/go-ethereum/ethclient"
```

连接到 Sepolia RPC

```
client, err := ethclient.Dial("https://ethereum-sepolia-rpc.publicnode.com")
if err != nil {
    panic(err)
}
```

获取当前区块高度

```
header, err := client.HeaderByNumber(context.Background(), nil)
fmt.Println("current block:", header.Number.String())
```

查询区块

```
blockNumber := big.NewInt(123456)
block, _ := client.BlockByNumber(context.Background(), blockNumber)

fmt.Println("hash:", block.Hash().Hex())
fmt.Println("parent:", block.ParentHash().Hex())
fmt.Println("tx count:", len(block.Transactions()))
```

查询交易与回执

```
txHash := common.HexToHash("0x你的交易哈希")
tx, _, _ := client.TransactionByHash(context.Background(), txHash)

fmt.Println("to:", tx.To())
fmt.Println("value:", tx.Value().String())
```

Receipt：

```
receipt, _ := client.TransactionReceipt(context.Background(), txHash)

fmt.Println("status:", receipt.Status)
fmt.Println("logs:", receipt.Logs)
```

# Follow Up - 理解 block, transaction, receipt 的结构

上一节中查询到的数据会包含大量字段。本部分任务要求理解其中关键字段的含义。

可以观看 B 站肖臻老师的公开课，并配合相关文档来理解。

关于 Block 建议理解的字段包括：

- number
- hash
- parentHash
- timestamp
- gasUsed / gasLimit
- transactions

Follow-Up：

- 为何 parentHash 能形成区块链
- gasLimit 如何影响合约执行

***

关于 Transcation 建议理解的字段包括：

- nonce
- from / to
- input (call data)
- gas / gasPrice
- value
- type (legacy, EIP-1559)

Follow-Up：

- 一笔交易最终执行逻辑是如何解析 input 的

***

关于 Receipt 建议理解的字段包括:

- status
- logs
- contractAddress

Follow-Up：

- 为什么 Receipt 里有 logs, 而 transaction 本身没有
- 为什么 block 中也包含 logs bloom

# Follow Up: 使用 eth_call 自己调用 Week 1 合约 

例如靶子合约的 hint(), 解题合约的方法等

.