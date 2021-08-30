# 控制语句

控制语句既不忽略结果也不声明新项。唯一的这样的语句是`for-while` 循环。

## `for-while` 循环

```rust,no_run,noplaypen
for {identifier} in {range} [while {expression}] {
    ...
}
```

`for` 循环语句可以使用 `while` 条件进行修改，该条件将在每次循环迭代之前进行检查。 `while` 条件表达式可以访问循环迭代器变量。

```rust,no_run,noplaypen
let x = 7;

for i in 0..10 while i % x != 2 {
    // do something
};
```

只有常量表达式可以用作迭代器范围的边界。 `while` 条件不会导致提前返回，但会抑制循环体的副作用。

Zinc 是一种图灵不完备语言，因为它受 R1CS 限制规定，因此循环始终具有固定的迭代次数。
一方面，可以将循环计数器优化为常数，降低电路成本，但另一方面，不能强制循环提前返回，增加电路成本。

## `if` 和 `match`

[条件匹配](../05-expressions/03-conditionals.md) 表达式可以作为控制语句，忽略返回值。
要在这样的角色中使用它们，只需用分号终止表达式：

```rust,no_run,noplaypen
fn unknown(value: u8) -> u8 {
    match value {
        1 => dbg!("One!"),
        2 => dbg!("Two!"),
        _ => dbg!("Perhaps, three!"),
    };
    42
}
```
