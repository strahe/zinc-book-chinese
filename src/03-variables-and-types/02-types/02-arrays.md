# 数组

数组是顺序存储在内存中的相同类型值的集合。

数组支持索引和切片运算符，详细解释在[这里](../../04-operators/06-access.md).

```rust,no_run,noplaypen
let mut fibbonaci = [0, 1, 1, 2, 3, 5, 8, 13];
let element = fibbonaci[3];
fibbonaci[2] = 1;
```

> 当前语言状态下的数组有一个小的限制。 
> 数组不能使用见证值进行索引，而只能使用常量或与见证无关的变量进行索引。
