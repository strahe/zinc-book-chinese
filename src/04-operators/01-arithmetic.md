# 算术运算符

算术运算符在编译时不执行任何类型的溢出检查。如果发生溢出，Zinc VM 将在运行时失败。

> 在负数的除法方面，Zinc 遵循欧几里得除法概念。
> 这意味着`-45 % 7 == 4`。 获取详细说明和一些示例, 看 [这篇文章](https://en.wikipedia.org/wiki/Euclidean_division).

`+=`、`-=`、`=`、`=`、`%=` 快捷操作符执行操作并将结果分配给第一个操作数。第一个操作数必须是可变内存位置，如变量、数组元素或结构字段。

### 加法

`+` 和 `+=` 是二元运算符。

*Accepts*
1. 整数表达式
2. Expression of the operand 1 type

*返回* 相同类型的整数.

### 减法

`-` 和 `-=` 是二元运算符。

*Accepts*
1. 整数表达式
2. Expression of the operand 1 type

*返回* 相同类型的整数.

### 乘法

`*` 和 `*=` 是二元运算符。

*Accepts*
1. 整数表达式
2. Expression of the operand 1 type

*返回* 相同类型的整数.

### 除法

`/` 和 `/=` 是二元运算符。

*Accepts*
1. 整数表达式 (除`field`外的任何类型)
2. Expression of the operand 1 type

*返回* 相同类型的整数。

### 求余

`%` 和 `%=` 是二元运算符.

*Accepts*
1. 整数表达式 (除`field`外的任何类型)
2. Expression of the operand 1 type

*返回* 相同类型的整数。

### 负数

`-` 是一元运算符.

*Accepts*
1. 无符号整数表达式

*返回* 相同类型的整数。