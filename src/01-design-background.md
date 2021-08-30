# 设计背景

Zinc 的目标是可以轻松的编写安全的零知识电路和基于 ZKP 的智能合约。它的设计遵循以下原则：

- **Security**. 编写确定性和安全的应用程序应该很容易。相反，编写一些其他编程语言中发现的可能的漏洞应该很难。
- **Safety**. 该语言必须强制执行最严格的可用语义，例如强大的静态显式类型系统。
- **高效**. 代码应该编译为最有效的电路。
- **成本透明**. 无法有效优化的性能成本必须明确告知开发人员。一个例子是要求使用常量显式指定循环范围。
- **简单**. 任何熟悉类 C 语言（Javascript、Java、Golang、C++、Rust、Solidity、Move）的人都应该能够简单快速的学习 Zinc。
- **可读性**. Zinc 中的代码对于熟悉 C++ 语言系列的任何人来说都应该很容易阅读。不应该有违反直觉的概念。
- **极简主义**. 代码越少越好。理想情况下，应该只有一种方法可以有效地做某事。应该降低复杂性。
- **表现力**. 该语言应该足够强大，可以轻松构建复杂的应用程序。
- **图灵不完备**. Zinc 中不允许无限循环和递归。这不仅可以实现更高效的 R1CS 电路构建，还可以更轻松地对调用和堆栈安全进行形式验证，并消除图灵完备智能合约平台（例如 EVM）固有的 gas 计算问题。

# 主要特点

- 类型安全
- 类型推断
- 不变性
- 作为一等公民的可移动资源
- 模块定义和导入
- 表达性语法
- 工业级编译器优化
- 图灵不完备性：没有递归或无限循环
- Rust, JS, Solidity, C++ 开发人员学习曲线平坦

# 与 Rust 的比较

Zinc 专为 ZK 电路和基于 ZKP 的智能合约开发而设计，因此与 Rust 的一些差异是不可避免的。

## 类型系统

我们需要调整类型系统以在有限域中有效表示，这是 R1CS 的基本组成部分。目前的类型系统大多遵循 Rust，但有些方面是从智能合约语言中借用的。例如，Zinc 提供 1 字节步长的整数类型，就像 Solidity 中的那些一样。

## 所有权和引用

与 冯.诺依曼 架构相比，R1CS 电路中的内存管理非常不同。此外，由于 R1CS 并不意味着并行编程模式，因此 Rust 设计的许多元素将是不必要和多余的。 Zinc 在 Rust 中没有所有权机制，因为所有变量都将按值传递。借用机制仍在设计中，但将来可能只允许不可变引用。

## 循环和递归

Zinc 是一种图灵不完备语言，因为它不允许递归和可变循环索引。每个循环范围都必须以常量文字或表达式为界。