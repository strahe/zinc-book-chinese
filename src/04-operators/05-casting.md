# 转换操作

`as` 是一个二元运算符.

*Accepts*
1. 任何类型的表达式
2. 相同或其他整数类型的表达式

*返回* 转换之后的值.

允许转换:

- 从整数到整数
- 从枚举到整数
- 相同的类型（没有效果，没有错误）

```rust,no_run,noplaypen
enum Order {
    First = 1,
}

let a = 1; // inferred as u8
let b = a as i8; // explicit casting to the opposite sign
let c: u8 = Order::First; // implicit casting to an integer
```
