# 表达式语句

## 表达式

表达式语句是一个以`;`结尾的表达式，以忽略其结果。最常见的用途是给可变变量赋值：

```rust,no_run,noplaypen
let mut a = 0;
a = 42; // an expression statement ignoring the '()' result of the assignment
```

有关表达式的更多信息，请查看 [这一章节](../05-expressions/00-overview.md).

## 分号

Zinc 中的表达式语句必须始终以`;`结尾，以消除有关块和条件表达式的一些歧义。
