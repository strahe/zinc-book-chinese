# 函数

函数是 Zinc 中唯一可调用的类型。
但是，R1CS 规范要求函数必须完全执行，因此没有 `return` 语句。
返回值的唯一方法是将其指定为函数体的最后一个未终止语句。

函数由几个部分组成：名称、参数、返回类型和代码块。
函数名称在其命名空间内唯一地定义了该函数。
参数只能按值传递，函数结果只能按值返回。
如果省略返回类型，则认为该函数返回一个单元值`()`。
代码块可以访问全局作用域，但它没有关于从何处调用函数的信息。

```rust,no_run,noplaypen
const GLOBAL: u8 = 31;

fn wierd_sum(a: u8, b: u8) -> u8 {
    dbg!("{} + {}", a, b);
    a + b + GLOBAL // return value
}

fn main() {
    let result = wierd_sum(42, 27);
    require(result == 100, "the weird sum is incorrect");
}
```

## 方法

方法是在结构或枚举实现或智能合约定义中声明的函数。
这些函数接受对象实例作为第一个参数，并且可以通过点运算符调用。

```rust,no_run,noplaypen
struct Data {
    a: u8,
    b: u8,
    c: u8,
    d: u8,
}

impl Data {
    pub fn sum(self) -> u8 {
        self.a + self.b + self.c + self.d
    }
}

fn main() {
    let data = Data { a: 1, b: 2, c: 3, d: 4 };
    
    dbg!("Data sum is: {}", data.sum());
}
```

方法可以像普通函数一样使用声明它们的类型命名空间来调用。在某些语言中，它被称为静态方法：

```rust,no_run,noplaypen
dbg!("Data sum is: {}", Data::sum(data));
```

如果方法的第一个参数是可变的，则该方法被认为是可变的，它可以更改实例字段值。
此外，一个可变方法只能从另一个可变方法中调用，从而提供了一些额外的数据安全性。

```rust,no_run,noplaypen
struct Data {
    a: u8,
    b: u8,
}

impl Data {
    pub fn double(mut self) -> Self {
        self.a *= 2;
        self.b *= 2;
        self
    }
}

fn main() {
    let mut data = Data { a: 2, b: 1 };
    dbg!("Data x1 is: {}", data);

    let data_x2 = data.double();
    dbg!("Data x2 is: {}", data_x2);
}
```

## 常数函数

常量函数在编译时被调用，因此它们只能接受和返回常量表达式。
当您需要使用大量相似的参数化值，并且您不愿意每次都重复计算代码时，此类函数很有用。

```rust,no_run,noplaypen
const fn cube(x: u64) -> u64 { x * x * x }

fn main() {
    let cubed_ten = cube(10 as u64); // 1000
    let cubed_twenty = cube(20 as u64); // 8000
}
```

此类函数仅存在于编译时，因此它们根本不会影响应用程序性能。
