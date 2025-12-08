---
timezone: UTC+8
---

# 你的名字

1. 自我介绍
   henry, 软件与微电子学院三年级网安方向，目前小白，想深入学习合约安全
2. 你认为你会完成这次共学小组吗？
   可以
3. 你感兴趣的小组
   合约安全小组
4. 你的联系方式（Wechat or Telegram）
   Wechat：bsd_crow

## Notes

<!-- Content_START -->

### 2025.11.22

#### Part I - 动手部署一个智能合约

注册metamask钱包，然后通过以下两个水龙头网站获取测试币（没有主网eth要求）

> https://cloud.google.com/application/web3/faucet/ethereum/sepolia
>
> https://faucet.metana.io/#

https://remix.ethereum.org/ 部署合约，

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HelloWeb3 {
    event Greeting(address indexed sender, uint256 timestamp);
  
    constructor() {}

    function hello() external {
        emit Greeting(msg.sender, block.timestamp);
    }
```

然后按步骤进行操作，最后查看交易信息

![image](https://github.com/henrymartin262/PKUBA-Colearn-25-Fall/blob/main/img/201dbb50-6a41-43e2-a967-5ea556a69bb2.png)

![image](https://github.com/henrymartin262/PKUBA-Colearn-25-Fall/blob/main/img/Snipaste_2025-11-22_18-23-45.png)

### 2025.11.22

#### Part II - 智能合约编写

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// 靶子合约接口（根据题目给的方法名来写）
interface ITarget {
    function hint() external view returns (string memory);
    function query(bytes32 _hash) external returns (string memory);
    function getSolvers() external view returns (address[] memory);
}

contract Week1Solver {
    ITarget public target;

    // 保存最近一次 query 的返回结果（例如 Flag）
    string public lastResult;

    // 方便在日志里看到返回值
    event QueryResult(string result);

    constructor(address _target) {
        target = ITarget(_target);
    }

    // 调靶子合约的 hint，拿到提示
    function getHint() external view returns (string memory) {
        return target.hint();
    }

    // 帮助你在链上算 hash
    function calcHash(string memory s) external pure returns (bytes32) {
        return keccak256(abi.encodePacked(s));
    }

    // 真正提交答案的函数，接收并返回 query 的字符串结果
    function solve(bytes32 answer) external returns (string memory) {
        // 调用靶子合约，拿到返回的字符串（例如 Flag）
        string memory res = target.query(answer);

        // 存一份到状态变量，方便用 lastResult() 查看
        lastResult = res;

        // 打事件，方便在区块浏览器 / Remix Logs 里看
        emit QueryResult(res);

        // 同时作为返回值返回（在 Remix 的 decoded output 里看）
        return res;
    }

    // 直接从靶子合约读取完成者列表
    function getSolversFromTarget() external view returns (address[] memory) {
        return target.getSolvers();
    }
}
```

代码如上

![image](https://github.com/henrymartin262/PKUBA-Colearn-25-Fall/blob/main/img/Snipaste_2025-11-22_18-55-35.png)

返回结果如上，提示对 `PKUBlockchain` 进行keccak哈希，然后在链上算出 keccak256("PKUBlockchain")，然后把这个 bytes32 传给 query()

![image](https://github.com/henrymartin262/PKUBA-Colearn-25-Fall/blob/main/img/Snipaste_2025-11-22_19-20-43.png)

成功获取到flag，前往 https://sepolia.etherscan.io 查看事件信息，是否成功提交

![image](https://github.com/henrymartin262/PKUBA-Colearn-25-Fall/blob/main/img/Snipaste_2025-11-22_19-03-18.png)

确认

![image](https://github.com/henrymartin262/PKUBA-Colearn-25-Fall/blob/main/img/Snipaste_2025-11-22_19-03-42.png)

### 2025.12.09

通过 Week3.md 中的内容学习了使用Geth读取链上数据，对基本概念有了一些了解，同时完成本地环境搭建，并使用go-ethereum打印出完整链上数据，以下是相关代码：

```go
package main

import (
	"context"
	"fmt"
	"log"
	"math/big"
	"time"

	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/core/types"
	"github.com/ethereum/go-ethereum/ethclient"
)

func main() {
	ctx := context.Background()

	// 1. 连接节点
	client, err := ethclient.Dial("https://ethereum-sepolia-rpc.publicnode.com")
	if err != nil {
		log.Fatal(err)
	}
	defer client.Close()

	// 2. 获取当前区块头
	header, err := client.HeaderByNumber(ctx, nil)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("========== Current Chain Status ==========")
	fmt.Printf("Current block number: %s\n", header.Number.String())
	fmt.Println()

	// 3. 获取指定区块详情
	targetBlockNumber := big.NewInt(123456)
	block, err := client.BlockByNumber(ctx, targetBlockNumber)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("========== Block Details ==========")
	// Block Number: 区块高度，即这是链上的第几个区块
	fmt.Printf("Block Number:     %s\n", block.Number().String())
	// Block Hash: 区块的唯一标识符 (Hash)
	fmt.Printf("Block Hash:       %s\n", block.Hash().Hex())
	// Parent Hash: 上一个区块的 Hash，用于连接成链
	fmt.Printf("Parent Hash:      %s\n", block.ParentHash().Hex())
	// Block Time: 区块产生的时间戳
	fmt.Printf("Block Time:       %s\n", time.Unix(int64(block.Time()), 0).Format(time.RFC3339))
	// Difficulty: 挖矿难度 (PoS 之后通常为 0)
	fmt.Printf("Difficulty:       %s\n", block.Difficulty().String())
	// Gas Limit: 该区块允许消耗的最大 Gas 总量
	fmt.Printf("Gas Limit:        %d\n", block.GasLimit())
	// Gas Used: 该区块内所有交易实际消耗的 Gas 总量
	fmt.Printf("Gas Used:         %d\n", block.GasUsed())
	// Miner: 挖出该区块的矿工/验证者地址 (Coinbase)
	fmt.Printf("Miner:            %s\n", block.Coinbase().Hex())
	if block.BaseFee() != nil {
		// Base Fee: EIP-1559 引入的基础 Gas 费率，会被销毁
		fmt.Printf("Base Fee:         %s\n", block.BaseFee().String())
	}
	// Transactions: 该区块中包含的交易数量
	fmt.Printf("Transactions:     %d\n", len(block.Transactions()))
	fmt.Println()

	// 4. 获取交易详情
	txHash := common.HexToHash("0x903bd6b44ce5cfa9269d456d2e7a10e3d8a485281c1c46631ec8f79e48f7accb")
	tx, isPending, err := client.TransactionByHash(ctx, txHash)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("========== Transaction Details ==========")
	// Tx Hash: 交易的唯一标识符
	fmt.Printf("Tx Hash:    %s\n", tx.Hash().Hex())
	// Is Pending: 是否还在内存池中等待打包 (false 表示已上链)
	fmt.Printf("Is Pending: %t\n", isPending)
	// Type: 交易类型 (0=Legacy, 1=AccessList, 2=EIP-1559)
	fmt.Printf("Type:       %d (0=Legacy, 2=EIP-1559)\n", tx.Type())
	// Nonce: 发送者发送的交易计数，用于防止重放攻击
	fmt.Printf("Nonce:      %d\n", tx.Nonce())
	// Gas Limit: 发送者愿意为这笔交易支付的最大 Gas 量
	fmt.Printf("Gas Limit:  %d\n", tx.Gas())
	// Gas Price: 发送者愿意支付的 Gas 单价 (wei)
	fmt.Printf("Gas Price:  %s wei\n", tx.GasPrice().String())
	// Value: 随交易发送的 ETH 金额 (wei)
	fmt.Printf("Value:      %s wei\n", tx.Value().String())

	// 解析 From 地址 (需要 ChainID)
	chainID, err := client.NetworkID(ctx)
	if err != nil {
		log.Printf("Failed to get chainID: %v\n", err)
	} else {
		if sender, err := types.Sender(types.LatestSignerForChainID(chainID), tx); err == nil {
			// From: 交易发送方地址
			fmt.Printf("From:       %s\n", sender.Hex())
		}
	}

	if to := tx.To(); to != nil {
		// To: 交易接收方地址 (如果是合约调用，则是合约地址)
		fmt.Printf("To:         %s\n", to.Hex())
	} else {
		// 如果 To 为 nil，说明这是一笔创建新合约的交易
		fmt.Println("To:         [Contract Creation]")
	}

	// 打印 Input Data (前50字节)
	data := tx.Data()
	// Input Data: 交易附带的数据 (调用合约函数时的参数编码)
	fmt.Printf("Input Data: %x", data)
	if len(data) > 0 {
		fmt.Println(" ...")
	} else {
		fmt.Println(" (empty)")
	}
	fmt.Println()

	// 5. 获取交易回执
	receipt, err := client.TransactionReceipt(ctx, txHash)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("========== Transaction Receipt ==========")
	// Status: 交易执行结果 (1=成功, 0=失败/Revert)
	fmt.Printf("Status:             %d (1=Success, 0=Fail)\n", receipt.Status)
	// Block Number: 交易被打包进的区块高度
	fmt.Printf("Block Number:       %s\n", receipt.BlockNumber.String())
	// Transaction Index: 交易在区块中的索引位置
	fmt.Printf("Transaction Index:  %d\n", receipt.TransactionIndex)
	// Gas Used: 这笔交易实际消耗的 Gas 量
	fmt.Printf("Gas Used:           %d\n", receipt.GasUsed)
	// Cumulative GasUsed: 区块中这笔交易及之前所有交易消耗的 Gas 总和
	fmt.Printf("Cumulative GasUsed: %d\n", receipt.CumulativeGasUsed)

	if receipt.ContractAddress != (	) {
		// Contract Address: 如果是部署合约交易，这里显示新合约地址
		fmt.Printf("Contract Address:   %s\n", receipt.ContractAddress.Hex())
	}

	// Logs: 交易执行过程中触发的事件日志
	fmt.Printf("Logs Count:         %d\n", len(receipt.Logs))
	for i, l := range receipt.Logs {
		fmt.Printf("  [Log %d] Address: %s | Topics: %d\n", i, l.Address.Hex(), len(l.Topics))
	}
}

```

运行结果如下图所示：
![image](https://github.com/henrymartin262/PKUBA-Colearn-25-Fall/blob/main/img/Snipaste_2025-12-09_00-50-36.png)

关键字段解释理解如下：

#### **1. Block (区块)**

**核心字段解释**

- **`number` (区块高度)**: 该区块在整条区块链中的序号（例如第 100 个区块）。
- **`hash` (区块哈希)**: 该区块的唯一标识符（身份证），由区块头的所有数据计算得出。
- **`parentHash` (父区块哈希)**: 前一个区块的哈希值。它将当前区块与上一个区块“锁”在一起。
- **`timestamp` (时间戳)**: 区块被矿工/验证者打包出的时间（Unix 时间戳）。
- **`gasUsed` (Gas 用量)**: 该区块内**所有交易**实际消耗的 Gas 总和。
- **`gasLimit` (Gas 上限)**: 该区块**允许**消耗的最大 Gas 总量。它决定了一个区块能容纳多少笔交易。
- **`transactions` (交易列表)**: 该区块中包含的所有交易数据。

**Follow-Up 问答**

- **Q: 为何 `parentHash` 能形成区块链？**
  - **A**: 每个区块都包含前一个区块的哈希 (`parentHash`)。如果你篡改了第 N 个区块的任何数据，它的哈希就会改变。那么第 N+1 个区块记录的 `parentHash` 就对不上了，导致第 N+1 个区块也无效。这种环环相扣的结构（链式结构）保证了历史数据一旦写入就无法被悄悄修改，从而形成了不可篡改的“区块链”。
- **Q: `gasLimit` 如何影响合约执行？**
  - **A**:
    1. **单笔交易限制**: 一笔交易消耗的 Gas 不能超过区块的 `gasLimit`，否则永远无法被打包。
    2. **区块容量限制**: `gasLimit` 决定了区块的“容量”。如果网络拥堵，交易很多，矿工会优先打包 Gas Price 高的交易，直到填满 `gasLimit`。
    3. **防止攻击**: 它防止了恶意用户发送死循环代码或超大计算量的交易来阻塞整个网络节点。



#### 2. Transaction (交易)

**核心字段解释**

- **`nonce` (计数器)**: 发送者账户发出的交易序号（从 0 开始递增）。用于防止**重放攻击**（即防止同一笔交易被重复执行）。
- **`from` / `to`**:
  - `from`: 发起交易的账户地址（必须由私钥签名）。
  - `to`: 接收方地址。如果是普通转账，就是收款人；如果是调用合约，就是合约地址；如果是**创建合约**，该字段为空 (`nil`)。
- **`input` (或 `data`)**: 交易附带的数据。
  - 普通转账：通常为空。
  - 合约调用：包含要调用的函数签名和参数编码。
  - 合约部署：包含合约的字节码。
- **`gas` (Gas Limit)**: 发送者愿意为这笔交易支付的最大 Gas 数量（防止合约死循环耗光余额）。
- **`gasPrice`**: 发送者愿意支付的 Gas 单价（单位通常是 wei）。在 EIP-1559 后通常由 `maxFeePerGas` 和 `maxPriorityFeePerGas` 替代。
- **`value`**: 随交易发送的 ETH 金额（单位是 wei）。
- **`type`**: 交易类型。`0` 是老式交易，`2` 是 EIP-1559 标准交易（引入了 Base Fee 销毁机制）。

**Follow-Up 问答**

- **Q: 什么是 ABI？一笔交易最终执行逻辑是如何解析 `input` 的？**
  - **A**:
    - **ABI (Application Binary Interface)**: 应用二进制接口。它定义了如何将高级语言（如 Solidity）的函数和参数编码成机器能读懂的二进制数据。
    - **解析逻辑**: EVM（以太坊虚拟机）读取 `input` 数据的前 **4 个字节**（函数选择器），匹配合约中对应的函数代码。剩下的数据按照 ABI 规则被解码为函数的**参数**，然后开始执行该函数的逻辑。



#### 3. Receipt (交易回执)

**核心字段解释**

- **`status` (状态)**:
  - `1`: **成功** (Success)。
  - `0`: **失败** (Failure/Revert)。交易虽然上链了，Gas 也扣了，但状态回滚，业务逻辑未生效。
- **`logs` (日志)**: 智能合约执行过程中通过 `emit` 关键字产生的事件（Event）。这是链下应用（如前端、后端索引器）监听链上状态变化的主要方式（例如监听 `Transfer` 事件来更新余额）。
- **`contractAddress`**:
  - 只有当交易是**部署新合约**时，这个字段才会有值（即新生成的合约地址）。
  - 对于普通转账或调用，该字段为空。



<!-- Content_END -->

### 
