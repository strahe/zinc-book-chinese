# 标准库

标准库当前位于名为 `std` 的内置模块中.
该库包含以下模块:
- `crypto` - 密码和散列函数
    - `ecc` - 椭圆曲线算法
    - `schnorr` - EDDSA 签名验证
- `convert` - 位数组转换函数
- `array` - 数组处理函数
- `ff` - 有限域函数
- `collections` - 数据集合类型

所有标准库内容都列在 [Appendix E](../appendix/E-standard-library.md).

标准库模块可以直接使用或通过 `use` 导入:

```rust,no_run,noplaypen
use std::crypto::sha256; // 导入

fn main(preimage: [bool; 256]) -> ([bool; 256], (field, field)) {
    let input_sha256 = sha256(preimage); // imported
    let input_pedersen = std::crypto::pedersen(preimage); // directly

    (input_sha256, input_pedersen)
}
```

# zkSync 库

zkSync 库是一个新兴的库，目前只包含全局事务 `msg` 变量：

```rust,no_run,noplaypen
let amount = zksync::msg.amount;
```

zkSync 库内容列在 [Appendix F](../appendix/F-zksync-library.md).
