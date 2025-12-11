---
timezone: UTC+8
---

> 请在上边的 timezone 添加你的当地时区(UTC)，这会有助于你的打卡状态的自动化更新，如果没有添加，默认为北京时间 UTC+8 时区


# LuBryant

1. 自我介绍：大家好，我叫卢博文，是北大物院25级的博士，现在刚刚开始接触区块链，希望可以多多学习到相关的知识。
2. 你认为你会完成这次共学小组吗？我认为会的。
3. 你感兴趣的小组：Onchain-data
4. 你的微信号：18970447065
5. 质押的交易哈希：0xd9945efa0e52226f47de8b8937f34521fae5319271fdf004ab78a385f43847d9

## Notes

<!-- Content_START -->

### 2025.12.07
#### Github 上复制文件
方法一：使用 GitHub Web Editor（最推荐，快捷键 .）
这是最接近本地操作体验的方法。GitHub 内置了一个基于 VS Code 的网页编辑器。

1. 打开你的 GitHub 仓库主页。
2. 在键盘上直接按下 英文句号键 .（或者将浏览器地址栏中的 github.com 改为 github.dev）。
3. 这会打开一个网页版的 VS Code 编辑器。
4. 在左侧文件树中，右键点击你要复制的文件 -> Copy。
5. 右键点击目标文件夹 -> Paste。
6. 点击左侧的“源代码管理”图标（Git 图标），输入 Commit 信息并提交更改。

方法二：手动“复制内容 + 新建文件”（适合单个小文件）
如果你不想进入编辑器模式，可以使用传统的笨办法：

1. 打开你想复制的文件。
2. 点击右上角的 Copy raw contents 图标（两个重叠的小方块）或者点击 Raw 按钮然后全选复制。
3. 回到仓库首页或目标文件夹。
4. 点击 Add file -> Create new file。
5. 在文件名处输入路径（例如 new_folder/filename.py）并粘贴内容。
6. 提交更改（Commit changes）。

#### Github 使用小知识
如何知道自己有使用权限？
方法一：最直观的 UI 验证（看“Settings”标签）
这是最简单的方法，不需要写代码。

1. 在浏览器中打开 你朋友的那个仓库页面（注意：不是你 Fork 出来的那个，是原本的那个 URL）。
2. 看页面顶部的导航栏（Code, Issues, Pull requests 那一行）。
3. 关键点： 如果你能看到 Settings（设置） 这一项，说明你已经是协作者了，拥有写权限。普通路人是看不到别人仓库的 Settings 选项的。

方法三：硬核命令行验证（Dry Run）
如果你已经把代码 clone 到本地了，可以用 Git 命令来测试。

注意： 因为你之前是 Fork 的，所以你本地默认的 origin 应该是指向 你自己的仓库。你需要先确认你有没有把你朋友的仓库添加为远程仓库（通常命名为 upstream）。

请在终端执行以下步骤：

添加朋友的仓库作为远程地址（如果你还没加的话）：

```Bash
# 假设你朋友的仓库地址是 https://github.com/friend/project.git
git remote add upstream https://github.com/friend/project.git
测试推送权限（伪推送）： 使用 --dry-run 参数，这会模拟推送过程但不会真的把代码传上去，专门用来测试连接和权限。
```
```Bash
git push upstream main --dry-run
```
如果成功： 会显示 Everything up-to-date 或者模拟写入的信息。说明你有权限！

如果失败： 会显示 403 Forbidden 或者 Permission denied。

---
Fork 了项目只是自己有了一份，并不会修改原有仓库。

既然你已经在自己的 Fork 里提交了代码，现在要把这个改动“发送”给你的朋友。因为你是在网页端操作，最简单的方法就是 提 Pull Request (PR)。

虽然你有写权限，但**跨仓库（从你的 Fork 到他的 Upstream）**传输代码，在网页上只能通过 PR。

操作步骤：

1. 打开 你自己 的那个仓库页面（你能看到 Commit 的那个页面）。
2. 点击页面上方的 Pull requests 标签。
3. 点击绿色的 New pull request 按钮。
4. 关键检查：
    - 左边（Base repository）：应该是 你朋友的仓库 (main 分支)。
    - 右边（Head repository）：应该是 你的仓库 (main 分支)。
5. 你会看到两个仓库的差异（也就是你刚才修改的内容）。
6. 点击 Create pull request。

重点来了： 因为你有 写权限，当你创建完 PR 后，你会发现 Merge 按钮是绿色的（或者你可以直接去他的仓库里点 Merge）。你自己就可以批准并合并这个 PR，不需要等他操作。合并后，他的仓库就有了你的更新。

下次如何直接利用“权限”修改？（不走弯路）
如果你想利用他对你开放的权限，跳过 Fork 和 PR 的步骤，直接修改他的项目，你需要改变操作习惯：

场景 A：如果你坚持在网页上改
不要去你自己的仓库！

1. 直接在浏览器打开 你朋友的仓库 URL。
2. 找到那个文件。
3. 点击右上角的笔形图标（Edit）。
4. 修改完直接 Commit。
    - 注意：如果你的权限没问题，它会允许你直接 Commit to main branch。如果只能 Create a new branch，说明主分支被保护了，还是得走 PR。

场景 B：如果你在本地电脑（推荐，更有高手风范）
你需要告诉你的本地 Git，要把代码推送到哪里。

1. 在本地终端，把 你朋友的仓库 添加为一个新的远程地址（通常叫 upstream）
```Bash
git remote add upstream https://github.com/朋友ID/项目名.git
```
下次修改完代码，直接推送到他的仓库：

```Bash
git push upstream main
```

#### 动手部署一个智能合约
PKUBlockchain 25 Fall Get Ready Quest: https://github.com/aliceyzhsu/crypto-techguy/blob/main/quests/get-ready.md

🎯 目标：在 Ethereum Sepolia 测试网上部署一个属于你自己的智能合约，并与该合约交互。

📌 准备工作：
1️⃣ 安装并配置好 MetaMask 钱包

2️⃣ 领取 Sepolia 测试网 ETH 测试币：https://cloud.google.com/application/web3/faucet/ethereum/sepolia

3️⃣ 准备好你要部署的合约代码, 可以直接使用下面的例子:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HelloWeb3 {
    event Greeting(address indexed sender, uint256 timestamp);
    
    constructor() {}

    function hello() external {
        emit Greeting(msg.sender, block.timestamp);
    }
}
```
这个代码定义了一个 event，可以在区块链浏览器看到。


🔄 操作流程详解：

1. 打开 Remix IDE：[https://remix.ethereum.org](https://remix.ethereum.org/)
2. 新建 HelloWeb3.sol 文件，粘贴上述代码
3. 进入 Solidity Compiler 标签页，点击“Compile”
4. 进入 Deploy & Run Transactions 标签页
5. 环境选择 “Injected Provider - MetaMask” （Injected Provider 表示 Metamask 是哪条链，就是哪个，可以直接选用 Sepolia Testnet - MetaMask）
6. 确认 MetaMask 已切换至 Sepolia 网络
7. 点击 Deploy，在 MetaMask 中确认交易
8. 等待部署成功，在 Remix 控制台复制合约地址和交易哈希
9. 在已经部署的合约中调用 hello 方法
10. 打开[区块链浏览器](https://sepolia.etherscan.io/)，搜索部署的合约地址，查看 Transactions 和 Events 结果




编译完成：

<img width="1629" height="1289" alt="image" src="https://github.com/user-attachments/assets/39b984f7-8165-41cb-9da9-beec42c12f2a" />

编译完成后部署。合约需要一个 Owner。

<img width="389" height="615" alt="image" src="https://github.com/user-attachments/assets/f0876244-e0b9-4b93-bc8f-a5d062b58664" />

<img width="971" height="647" alt="image" src="https://github.com/user-attachments/assets/d788eacb-de73-494b-9e57-af5cfe6c22f0" />

成功部署之后，就可以调用 hello 函数，

找到合约地址（Contract address），打开区块链浏览器：https://sepolia.etherscan.io/

部署成功。
<img width="1456" height="877" alt="image" src="https://github.com/user-attachments/assets/d9d361ec-11f1-46e7-b5e6-f8937a5cc975" />

#### 智能合约编写
使用代码
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// 1. 定义靶子合约的接口 (Interface)
// 我们不需要知道靶子的全部代码，只需要知道我们要调用的函数签名
interface ITarget {
    function hint() external view returns (string memory);
    function query(bytes32 _hash) external;
}

contract Solver {
    // 靶子合约地址
    address public targetAddress = 0x4a6C0c0dc8bD8276b65956c9978ef941C3550A1B;

    // 步骤 A: 获取提示
    // 这只是为了方便我们在 Remix 里直接看到提示内容
    function getHint() public view returns (string memory) {
        return ITarget(targetAddress).hint();
    }

    // 步骤 B: 提交答案
    // 假设逻辑是：将答案字符串进行 keccak256 哈希后提交
    // 如果题目逻辑不同，你可以修改这里的计算方式
    function solve(string memory solution) public {
        // 计算哈希 (通常 CTF 的 query(bytes32) 都是要求提交某个字符串的哈希)
        bytes32 answerHash = keccak256(abi.encodePacked(solution));
        
        // 调用靶子合约
        ITarget(targetAddress).query(answerHash);
    }

    // 备用：如果你在链下算好了 bytes32，直接用这个函数提交
    function solveRaw(bytes32 _hash) public {
        ITarget(targetAddress).query(_hash);
    }
}
```

在部署的时候，CONTRACT 要选择 Solver - Solver.sol

第二步：部署攻击合约
在 Remix 左侧点击 Solidity Compiler (以及图标)，点击 Compile Solver.sol。

点击左侧 Deploy & Run Transactions (以太坊图标)。

Environment 选择 Injected Provider - MetaMask (这会连接你的钱包)。

确保 Contract 选中的是 Solver。

点击 Deploy 按钮，在 MetaMask 中确认交易。

等待交易成功，左下角 "Deployed Contracts" 会出现你的合约。

第三步：获取提示 (Get Hint)
展开你刚部署的合约。

点击 getHint 按钮。

查看返回的字符串内容（Result）。

假设提示是 "Hello"，或者是某个具体的单词/短语。

注意：有时候提示本身就是一个谜题，你需要根据提示算出真正的答案字符串。

这是一个非常经典的 CTF（Capture The Flag）智能合约挑战任务。你需要充当一名“攻击者”，编写一个中间合约去调用靶子合约。

由于题目明确指出 query() 方法只能通过合约调用（通常是通过 msg.sender != tx.origin 来限制的），所以你不能直接在 Etherscan 上操作，必须写代码。

请按照以下步骤在 Remix IDE 中完成操作。

准备工作
工具：打开 Remix IDE (remix.ethereum.org)。

钱包：确保你的 MetaMask 切换到了 Sepolia 测试网，并且账户里有少量的测试币（ETH）用于支付 Gas 费。

第一步：编写攻击脚本 (Solver.sol)
在 Remix 的 contracts 文件夹下新建一个文件，命名为 Solver.sol，并将以下代码复制进去：

Solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// 1. 定义靶子合约的接口 (Interface)
// 我们不需要知道靶子的全部代码，只需要知道我们要调用的函数签名
interface ITarget {
    function hint() external view returns (string memory);
    function query(bytes32 _hash) external;
}

contract Solver {
    // 靶子合约地址
    address public targetAddress = 0x4a6C0c0dc8bD8276b65956c9978ef941C3550A1B;

    // 步骤 A: 获取提示
    // 这只是为了方便我们在 Remix 里直接看到提示内容
    function getHint() public view returns (string memory) {
        return ITarget(targetAddress).hint();
    }

    // 步骤 B: 提交答案
    // 假设逻辑是：将答案字符串进行 keccak256 哈希后提交
    // 如果题目逻辑不同，你可以修改这里的计算方式
    function solve(string memory solution) public {
        // 计算哈希 (通常 CTF 的 query(bytes32) 都是要求提交某个字符串的哈希)
        bytes32 answerHash = keccak256(abi.encodePacked(solution));
        
        // 调用靶子合约
        ITarget(targetAddress).query(answerHash);
    }

    // 备用：如果你在链下算好了 bytes32，直接用这个函数提交
    function solveRaw(bytes32 _hash) public {
        ITarget(targetAddress).query(_hash);
    }
}
第二步：部署攻击合约
在 Remix 左侧点击 Solidity Compiler (以及图标)，点击 Compile Solver.sol。

点击左侧 Deploy & Run Transactions (以太坊图标)。

Environment 选择 Injected Provider - MetaMask (这会连接你的钱包)。

确保 Contract 选中的是 Solver。

点击 Deploy 按钮，在 MetaMask 中确认交易。

等待交易成功，左下角 "Deployed Contracts" 会出现你的合约。

第三步：获取提示 (Get Hint)
展开你刚部署的合约。

点击 getHint 按钮。

查看返回的字符串内容（Result）。

假设提示是 "Hello"，或者是某个具体的单词/短语。

注意：有时候提示本身就是一个谜题，你需要根据提示算出真正的答案字符串。

第四步：提交答案 (Submit Flag)
根据你拿到的提示，计算出答案。通常有两种情况：

情况 A：提示就是答案本身 假设 hint() 返回的是 "Alice"，而题目要求你提交这个名字的哈希。

在你的合约中找到 solve 函数。

输入框填入提示的内容："Alice" (注意包含双引号，如果是字符串)。

点击 transact (也就是黄色按钮)。

MetaMask 确认交易。

情况 B：提示是一个谜题 假设 hint() 返回 "1+1"。

你需要先算出答案是 "2"。

然后在 solve 函数中输入 "2" 并提交。

结果：
```
0: string: keccak PKUBlockchain
```

这句话的意思是：“请对字符串 PKUBlockchain 进行 keccak256 哈希运算，并将结果提交上来。”

keccak: 指的是哈希算法 keccak256 (以太坊的标准哈希算法)。

PKUBlockchain: 指的是你需要进行哈希的原始内容（原文）。

进入在线网站：https://emn178.github.io/online-tools/keccak_256.html

得到 `PKUBlockchain` 的哈希值为 `e2a73c8e3af6379fa58e477b0e2129f21e0230100f0462b9832b00cd22414215`

成功完成任务。

测试一下推送到远端，这是从本地的推送。现在测试一下命令行。


















<!-- Content_END -->
