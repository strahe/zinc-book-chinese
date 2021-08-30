# 最小的例子

在这个例子中，我们将实现最简单的交易智能合约，可以在一对代币之间以恒定的价格进行交易。

您将需要 `zargo`，它是 Zinc 包管理器，它使用位于项目 `data` 目录中的输入数据 JSON 模板捆绑智能合约项目并简化合约方法的使用。

## 项目初始化

要创建新的智能合约项目，请使用以下命令：

```bash,no_run,noplaypen
zargo new --type contract constant_price
```

Zargo 将使用一些默认模板代码创建一个项目：

```rust,no_run,noplaypen
//!
//! The 'constant_price' contract entry.
//!

contract ConstantPrice {
    pub value: u64;

    pub fn new(value: u64) -> Self {
        Self {
            value: value,
        }
    }
}
```

让我们通过以下方式更改代码：

- 删除多余自动生成字段的 `value`
- 添加`fee`参数
- 添加两个可变的方法来实现 `exchange`s 和 `deposit`s
- 添加一个不可变的方法来获取合约手续费的值
- 声明 `Address` 和 `Balance` 类型别名
- 使用 zkSync 令牌地址声明 `TokenAddress` 枚举类型

`TokenAddress` 枚举列出了来自 `Rinkeby` zkSync 网络的类似令牌地址的标识符。 `Rinkeby` 网络上的代币地址不应更改，可以从此处获取以供进一步使用。

```rust,no_run,noplaypen
type Address = u160;
type Balance = u248;

enum TokenAddress {
    ETH = 0x0000000000000000000000000000000000000000,
    USDT = 0x3b00ef435fa4fcff5c209a37d1f3dcff37c705ad,
    USDC = 0xeb8f08a975ab53e34d8a0330e0d34de942c95926,
    LINK = 0x4da8d0795830f75be471f072a034d42c369b5d0a,
    TUSD = 0xd2255612f9b045e9c81244bb874abb413ca139a3,
    HT = 0x14700cae8b2943bad34c70bb76ae27ecf5bc5013,
    OMG = 0x2b203de02ad6109521e09985b3af9b8c62541cd6,
    TRB = 0x2655f3a9eeb7f960be83098457144813ffad07a4,
    ZRX = 0xdb7f2b9f6a0cb35fe5d236e5ed871d3ad4184290,
    BAT = 0xd2084ea2ae4bbe1424e4fe3cde25b713632fb988,
    REP = 0x9cac8508b9ff26501439590a24893d80e7e84d21,
    STORJ = 0x8098165d982765097e4aa17138816e5b95f9fdb5,
    NEXO = 0x02d01f0835b7fdfa5d801a8f5f74c37f2bb1ae6a,
    MCO = 0xd93addb2921b8061b697c2ab055979bbefe2b7ac,
    KNC = 0x290eba6ec56ecc9ff81c72e8eccc77d2c2bf63eb,
    LAMB = 0x9ecec4d48efdd96ae377af3ab868f99de865cff8,
    GNT = 0xd94e3dc39d4cad1dad634e7eb585a57a19dc7efe,
    MLTT = 0x690f4886c6911d81beb8130db30c825c27281f22,
    XEM = 0xc3904a7c3a95bc265066bb5bfc4d6664b2174774,
    DAI = 0x2e055eee18284513b993db7568a592679ab13188,
}

impl TokenAddress {
    pub fn is_known(address: Address) -> bool {
        match address {
            0x0000000000000000000000000000000000000000 => true,
            0x3b00ef435fa4fcff5c209a37d1f3dcff37c705ad => true,
            0xeb8f08a975ab53e34d8a0330e0d34de942c95926 => true,
            0x4da8d0795830f75be471f072a034d42c369b5d0a => true,
            0xd2255612f9b045e9c81244bb874abb413ca139a3 => true,
            0x14700cae8b2943bad34c70bb76ae27ecf5bc5013 => true,
            0x2b203de02ad6109521e09985b3af9b8c62541cd6 => true,
            0x2655f3a9eeb7f960be83098457144813ffad07a4 => true,
            0xdb7f2b9f6a0cb35fe5d236e5ed871d3ad4184290 => true,
            0xd2084ea2ae4bbe1424e4fe3cde25b713632fb988 => true,
            0x9cac8508b9ff26501439590a24893d80e7e84d21 => true,
            0x8098165d982765097e4aa17138816e5b95f9fdb5 => true,
            0x02d01f0835b7fdfa5d801a8f5f74c37f2bb1ae6a => true,
            0xd93addb2921b8061b697c2ab055979bbefe2b7ac => true,
            0x290eba6ec56ecc9ff81c72e8eccc77d2c2bf63eb => true,
            0x9ecec4d48efdd96ae377af3ab868f99de865cff8 => true,
            0xd94e3dc39d4cad1dad634e7eb585a57a19dc7efe => true,
            0x690f4886c6911d81beb8130db30c825c27281f22 => true,
            0xc3904a7c3a95bc265066bb5bfc4d6664b2174774 => true,
            0x2e055eee18284513b993db7568a592679ab13188 => true,
            _ => false,
        }
    }
}

contract ConstantPrice {
    const MAX_FEE: u16 = 10000;
    const PRECISION_MUL: Balance = 1E3;

    pub fee: u16;

    pub fn new(_fee: u16) -> Self {
        require(_fee <= Self::MAX_FEE, "The fee value must be between 0 and 10000");

        Self {
            fee: _fee,
        }
    }

    pub fn deposit(mut self) {
        // check if the transaction recipient is the contract address
        require(zksync::msg.recipient == self.address, "The transfer recipient is not the contract");

        // check if the deposited token is known to the contract
        require(TokenAddress::is_known(zksync::msg.token_address), "The deposited token is unknown");

        // check if the deposited amount is not zero
        require(zksync::msg.amount > 0, "Cannot deposit zero tokens");
    }

    pub fn exchange(
        mut self,
        withdraw_token: Address,
    ) {
        // check if the transaction recipient is the contract address
        require(zksync::msg.recipient == self.address, "The transfer recipient is not the contract");

        // check if the deposited token is known to the contract
        require(TokenAddress::is_known(zksync::msg.token_address), "The deposited token is unknown");

        // check if the withdrawn token is known to the contract
        require(TokenAddress::is_known(withdraw_token), "The withdrawn token is unknown");

        // check if the deposited amount is not zero
        require(zksync::msg.amount > 0, "Cannot deposit zero tokens");

        // check if the deposited and withdrawn token identifiers are different
        require(zksync::msg.token_address != withdraw_token, "Cannot withdraw the same token");

        let withdraw_token_amount = zksync::msg.amount *
            ((Self::MAX_FEE - self.fee) as Balance * Self::PRECISION_MUL / Self::MAX_FEE as Balance) /
            Self::PRECISION_MUL;
        // check if there is enough balance to withdraw
        require(self.balances.get(withdraw_token).0 >= withdraw_token_amount, "Not enough tokens to withdraw");

        self.transfer(zksync::msg.sender, withdraw_token, withdraw_token_amount);
    }

    pub fn get_fee(self) -> u16 {
        self.fee
    }
}
```

> 在我们的例子中，`fee`是一个介于`0`和`10000`之间的整数值，后者代表`100%`。 
> 以这种方式使用整数值是一种常见的做法，因为在安全的智能合约语言中通常对浮点数的支持有限。

> 我们还使用了一些额外的小数位来避免整数除法后得到零。 
> 也就是说，我们不做`amount * 9900 / 10000`，而是做`amount * 9900 * 1E3 / 10000 / 1E3`。

## 发布合约

在发布之前，在项目目录中运行 `zargo build`。然后，打开 `.datainput.json` 构造函数输入模板文件并填写您要传递的构造函数参数：

```json
{
  "arguments": {
    "new": {
      "_fee": "100"
    }
    // ...
  }
}
```

>另外，将您的帐户私钥放在项目根目录下的“private_key”文件中。
> 所有存款和转移到新创建的合约都将从该帐户完成。
> 确保您的帐户已解锁并有足够的余额来支付费用。
> 要了解如何解锁新的 zkSync 帐户，请访问 [故障排除](./04-troubleshooting.md) 章节.

要发布合约，请使用此简单命令与网络标识符和实例名称:

```bash,no_run,noplaypen
zargo publish --network rinkeby --instance default
```

> 由于本教程的每个follower都创建了一个名称为`constant_price`的合约，该合约可能无法上传，因为名称和版本必须是唯一的。
> 为了解决这个问题，您可以在`Zargo.toml` manifest 中更改您的合约名称或版本. 要查看所有上传的项目，请使用 `zargo download --list` 命令。

合约发布成功后，会返回其ETH地址和zkSync账户ID。您将需要该地址才能发起更多的调用。让我们假设它是`0x1234...1234`。

实例名称用于唯一标识您已发布的合约，而无需记住其 ETH 地址。

合约已经公布！

## 查询合约存储

“constant_price”合约现已发布，并已创建其专用存储实例。您可以查询 Zandbox 服务器以查看其零余额：

```bash,no_run,noplaypen
zargo query --network rinkeby --address 0x1234...1234
```

## 调用不可变合约方法

可以使用与上述相同的查询调用不可变合同方法，但使用 `method` 参数：

```bash,no_run,noplaypen
zargo query --network rinkeby --address 0x1234...1234 --method get_fee
```

输出:
```json
{
  "output": "100"
}
```

## 调用可变合约方法

现在让我们调用我们的合约 `deposit` 方法！

打开方法输入模板文件`.datainput.json`并指定要交换的令牌标识符和数量：

```json
{
  "msg": {
    "sender": "<your_address>",
    "recipient": "0x1234...1234",
    "token_address": "0x0000000000000000000000000000000000000000", // ETH
    "amount": "0.1_E18"
  }
}
```

> 指定代币数量的指数值时要小心，因为为每个代币指定正确的十进制数字数至关重要。

要调用合约方法，请使用以下命令以及方法名称和合约账户 ID：

```bash,no_run,noplaypen
zargo call --network rinkeby --address 0x1234...1234 --method deposit
```

调用成功后，再次查询合约存储，可以看到预期的结果：

```json
{
  "address": "0x1234...1234",
  "balances": [
    {
      "key": "0x0",
      "value": "100000000000000000" // 0.1_E18
    }
  ]
}
```

现在您可以重复调用其他代币，当交易所上有多个代币时，调用“exchange”方法，指定您要提取的代币。

## 下一步是什么

当你有一个新的智能合约版本时，只需用另一个实例名发布它，它就会得到一个单独的存储实例，独立存在！

此外，还有一个 [Curve smart contract](./03-curve-implementation.md) 在 Zinc实现. 一探究竟！
