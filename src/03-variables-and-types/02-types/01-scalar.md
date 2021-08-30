# 标量类型(Scalar types)

标量类型也被称为原生类型， 并且只包含一个值。

## 单元类型(Unit)

单元类型和值用空圆括号`()`表示。
该类型的值是从函数、块和其他不显式返回值的表达式中隐式返回的。此外，此类型可用作`main`函数的输入、见证和输出类型的占位符。

`()` 是单元类型和值的表示。 单元类型值不能被任何运算符使用或来回转换。

单元类型可以作为独立值存在：

```rust,no_run,noplaypen
let x = (); // ()
```

它由块或函数隐式返回:

```rust,no_run,noplaypen
fn check(value: bool) {
    // several statements
};

let y = check(true); // y is ()
```

## 布尔类型(Boolean)

`bool` 是布尔类型关键字。

布尔值用  `0` 或者 `1` 来表示`field`。

不允许在布尔类型和整数类型之间进行类型安全转换。

### 字面量(Literals)

`true` 和 `false`.

### 例子

```rust,no_run,noplaypen
let a = true;
let b: bool = false;

if a && !b {
    debug(a ^^ b);
};
```

## 整数类型 (Integer)

整数类型可以是 1 到 32 字节之间的任意大小。
这个特性是从 Solidity 借来的，它有助于减少约束的数量和智能合约的大小。
内部整数表示使用不同位长的 BN256 字段。

### 类型

- `u8` .. `u248`: 无符号整数
- `i8` .. `i248`: 有符号整数
- `field`: 原生字段整数

整数类型位长步长等于 8，也就是说，只有以下位长是可能的：`8`、`16`、...、`240`、`248`。

`field` 值是约束系统中使用的椭圆曲线的原生字段元素。 它表示位长的无符号整数等于字段模型长度（例如，对于 BN256，场模长度是“254”位）.

所有类型都使用`field`作为其基本组成部分。分配整数变量时，必须在约束系统中强制执行其位长。

### 字面量(Literals)

- decimal: `0`, `1`, `122`, `574839572494237242`
- hexadecimal: `0x0`, `0xfa`, `0x0001`, `0x1fffDEADffffffffffBEEFffff`

只能表示无符号整数，因为减号不是文字字面量的一部分而是一个独立的运算符.因此，可以使用减号将无符号值隐式转换为有符号值。

### Casting

只能在整数和`field`类型之间进行转换。如果该值不适合目标类型，则将其截断。

### 推理

如果未指定类型，则推断出可能的最小位长。

### 例子

```rust,no_run,noplaypen
let a = 0; // u8
let a: i24 = 0; // i24
let b = 256; // u16
let c = -1;  // i8
let c = -129; // i16
let d = 0xff as field; // field
let e: field = 0; // field
```
