# 智能合约

Zinc 智能合约由入口文件 `main.zn` 组成，其中声明了合约本身，以及零个或多个模块，其内容可以导入到主文件中。

## 例子

### 入口文件

```rust,no_run,noplaypen
/// 
/// 'src/cube_deposit.zn'
///
/// Triples the deposited amount.
///

mod simple_math;

use simple_math::cube;

contract CubeDeposit {
    pub balance: u64;

    pub fn deposit(mut self, amount: u64) {
        self.balance += cube(amount);
    }
}
```

### 模块 `simple_math` 文件

```rust,no_run,noplaypen
/// 
/// 'src/simple_math.zn'
/// 

/// Returns x^3.
fn cube(x: u64) -> u64 {
    x * x * x
}
```
