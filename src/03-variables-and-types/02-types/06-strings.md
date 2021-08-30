# 字符串

目前，字符串的实现和可用性非常有限。

字符串值只能以字面量形式存在，并且只能出现在 `dbg` 和 `require` 内部函数中：

```rust,no_run,noplaypen
dbg!("{}", 42); // format string

require(true != false, "a very obvious fact"); // optional error message
```
