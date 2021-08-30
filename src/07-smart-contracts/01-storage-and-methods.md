# 储存和方法

一个典型的合约由几组实体组成：

- 隐式存储字段
- 显式存储字段
- 构造函数
- 公共方法
- 私有方法
- 内置方法
- 全局变量
- 常量

## 隐式存储字段

有几个隐式创建的字段，它们是在合约发布时设置的：

- 合约地址 (`u160` 类型的字段 `address`)
- 合约余额（`std::collections::MTreeMap<u160, u248>`类型的字段`balances`）,
其中key为zkSync token地址，value为token数量.

所以，当你看到一个空合约 `contract Empty {}` 时，它实际上是这样的：

```rust,no_run,noplaypen
// will not compile, because the fields are already there!
contract Empty {
    pub address: u160;

    pub balances: std::collections::MTreeMap<u160, u248>;
}
```

查询合约存储状态时，公共（`pub`）字段是可见的，而私有字段是内部的，无法看到。

## 显式存储字段

显式存储字段的声明方式与结构体中的声明方式相同，但以分号结尾。

```rust,no_run,noplaypen
contract Example {
    pub tokens: (u8, u64);

    data: [u8; 1000];

    //...
}
```

每个智能合约实例都有自己的存储，由 Zinc Zandbox 服务器写入持久数据库。

## 构造函数

每个合约都必须有一个构造函数，一个名为“new”的特殊函数，它返回一个“Self”合约实例。合约实例不是一个值，而是一个对它的引用，即它的类型为 `u160` 的 ETH 地址。

您不能初始化隐式存储字段，因为它们是自动填充的。

```rust,no_run,noplaypen
contract Example {
    pub value: u64;

    pub fn new(_value: u64) -> Self {
        Self {
            value: _value,
        }
    }
}
```

## 公共方法

合约声明包含几个公共函数，用作合约方法。合约必须至少具有一项公共功能。

```rust,no_run,noplaypen
contract Example {
    //...

    pub fn deposit(mut self, amount: u64) -> bool { ... }
}
```

## 私有方法

私有函数的声明没有使用 `pub` 关键字并且没有特殊含义。这些函数只是与合约相关联，可以从公共方法中调用。

```rust,no_run,noplaypen
contract Example {
    //...

    fn get_balance(address: u160) -> bool { ... }
}
```

## 内置方法

每个智能合约都包含两个内置方法。

方法签名在 [Appendix D](../appendix/D-intrinsic-functions.md).

### 转账

`transfer` 方法用于将代币发送到另一个帐户。该方法是可变的，因此只能从可变上下文中调用它。

```rust,no_run,noplaypen
contract Example {
    //...

    fn send(mut self, address: u160, amount: u248) -> {
        self.transfer(
            address, // recipient address
            0x0, // zkSync ETH token address
            amount, // amount in wei
        );
    }
}
```

### Fetch

`fetch` 方法用于从 Zandbox 服务器加载合约实例。

```rust,no_run,noplaypen
contract Example {
    //...

    pub fn get_balance(self, address: u160, token: u160) -> u248 {
        let instance = AnotherContract::fetch(address);
        let (balance, found) = instance.balances.get(token);
        balance
    }
}
```

## 全局变量

每个合约都包含全局变量 `zksync::msg`，其中包含调用合约时使用的传输数据。 
变量描述可以在 [Appendix F](../appendix/F-zksync-library.md) 中找到.

## 常量

合约可能包含一些与之相关的常量。常量没有任何特殊含义，可以在合约函数内部或外部使用。

```rust,no_run,noplaypen
contract Example {
    //...

    pub const VERSION: u8 = 1; // public constant 

    const LIMIT: u8 = 255; // private constant
}
```
