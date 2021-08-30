# `require` 函数

此函数在代码的任何位置创建自定义约束。使用 `require()` 你可以检查某些条件是否为真，如果不是，则使应用程序退出并显示错误。

完整的函数功能描述在 [这里](../appendix/D-intrinsic-functions.md).

## 例子

```rust,no_run,noplaypen
const BAD_VALUE: u8 = 42;

fn wrong(a: u8, b: u8) -> u8 {
    let c = a + b - BAD_VALUE;
    require(a + b == c, "always fails");
    c
}
```
