# 字典

`std::collections::MTreeMap` 是一种特殊类型，只能用作智能合约存储字段：

```rust,no_run,noplaypen
use std::collections::MTreeMap;

struct Data {
    a: u8,
    b: u8,
}

contract Test {
    owner: u160;
    data: MTreeMap<u8, Data>;
    
    pub fn new(owner: u160) -> Self {
        Self {
            owner: owner,
            data: MTreeMap,
        }
    }

    pub fn example(mut self) {
        let (old1, existed1) = self.data.insert(42, Data { a: 16, b: 9 });
        let (value, exists1) = self.data.get(42);
        let exists2 = self.data.contains(42);
        let (old2, existed2) = self.data.remove(42);
    }
}
```

> 字典引入了泛型类型的新概念，但此功能只能用于指定 `MTreeMap` 实例的键和值类型。

`MTreeMap` 方法的完整描述在 [这里](../../appendix/E-standard-library.md#stdcollectionsmtreemapk-v).

## 初始化

要初始化字典，只需使用类型名称作为值。将来，语法可能会更改为更直观的内容。

```rust,no_run,noplaypen
use std::collections::MTreeMap;

contract Test {
    data: MTreeMap<u8, field>;
    
    pub fn new(owner: u160) -> Self {
        Self {
            data: MTreeMap,
        }
    }
}
```
