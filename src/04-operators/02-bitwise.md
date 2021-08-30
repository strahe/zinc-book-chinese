# 按位操作符

`|=`、`^=`、`&=`、`<<=`、`>>=` 快捷操作符执行操作并将结果分配给第一个操作数。 第一个操作数必须是可变内存位置，如变量、数组元素或结构字段。

### 按位或

`|` 和 `|=` 是二元运算符.

*Accepts*
1. 无符号整数表达式 (排除 `field`)
2. Expression of the operand 1 type

*返回* 相同类型的整数.

### 按位异或

`^` 和 `^=` 是二元运算符。

*Accepts*
1. 无符号整数表达式 (排除 `field`)
2. Expression of the operand 1 type

*返回* 相同类型的整数.

### 按位与

`&` 和 `&=` 是二元运算符。

*Accepts*
1. 无符号整数表达式 (排除 `field`)
2. Expression of the operand 1 type

*返回* 相同类型的整数.

### 按位左移

`<<` 和 `<<=` 是二元运算符。

*Accepts*
1. 无符号整数表达式 (排除 `field`)
2. 常量无符号整数表达式

*返回* 相同类型的整数.

### 按位右移

`>>` 和 `>>=` 是二元运算符。

*Accepts*
1. 无符号整数表达式 (排除 `field`)
2. 常量无符号整数表达式

*Returns* an integer result of the operand 1 type.

### 按位非

`~` 是一元运算符。

*Accepts*
1. 无符号整数表达式 (排除 `field`)

*返回* 一个整数结果.
