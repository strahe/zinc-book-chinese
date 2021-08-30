# 块

块表达式由零个或多个语句和可选的结果表达式组成。每个块都开始一个新的可见范围。

```rust,no_run,noplaypen
let c = {
    let a = 5;
    let b = 10;
    a + b
};
```
