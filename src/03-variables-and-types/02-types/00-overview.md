# 类型

Zinc 是一种静态类型语言，因此所有变量都必须在编译时具有已知的类型。
严格类型系统允许捕获大多数在动态类型语言中很常见的运行时错误。

> 如果您熟悉 Rust，您会发现 Zinc 类型系统非常相似，但有一些修改、限制和控制。

类型分为几组:

- [Scalar](./01-scalar.md)
- [Array](./02-arrays.md)
- [Tuple](./03-tuples.md)
- [Structure](./04-structures.md)
- [Enumeration](./05-enumerations.md)
- [String](./06-strings.md)

要了解关于 显示类型转换, 隐式类型转换, 和 类型策略, 访问 [这个章节](./07-casting-and-conversions.md).

您可以在 Zinc 中声明类型别名，这允许您通过给它们一个名称来缩短复杂类型的类型签名：

```rust,no_run,noplaypen
type ComplexType = [(u8, [bool; 8], field); 16];

fn example(data: ComplexType) {}
```
