# 元组

元组是不同类型值的匿名集合，顺序存储在内存中，并由于某些逻辑关系聚集在一起。

元组字段可以通过点运算符访问，详细解释在[这里](../../04-operators/06-access.md).

```rust,no_run,noplaypen
let mut tuple: (u8, field) = (0xff, 0 as field);
tuple.0 = 42;
dbg!("{}", tuple.1);
```

> 如果您熟悉 Rust，您可能还记得单元值、括号表达式和一个元素的元组之间的特殊联系:
> - `()` 是一个单元值
> - `(value)` 是一个带括号的表达式
> - `(value,)` 是一个元素的元组
