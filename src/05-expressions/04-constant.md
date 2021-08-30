# 常量表达式

在编译时计算常量表达式。声明一些在整个项目中使用的全局数据很有用。

```rust,no_run,noplaypen
const UNIX_EPOCH_TIMESTAMP: (u64, u8, u8) = (1970, 1, 1);
```
