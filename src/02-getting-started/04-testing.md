# 测试

Zinc 框架提供了一些基本的单元测试功能。

单元测试只是用 `[test]` 属性标记的简单函数。此类函数可以在任何模块的根范围内的任何位置声明。

测试函数还可以被其他特殊属性标记：

- `#[should_panic]` 此类测试必须失败才能成功，例如通过将错误值传递给 `require` 函数或导致溢出。

- `#[ignore]` 此类测试会被忽略.

## 例子

```rust,no_run,noplaypen
#[test]
fn ordinar() {
    require(2 + 2 == 4, "The laws of the Universe have been broken");
}

#[test]
#[should_panic]
fn panicking() {
    require(2 + 2 == 5, "And it's okay");
}

#[test]
#[ignore]
fn ignored() {
    require(2 + 2 > 4, "So we'll just ignore it");
}
```
