# 依赖系统

Zargo 提供了使用其他 Zinc 项目作为依赖项的可能性。

要使用依赖项，请在`Zargo.toml`项目清单的`dependencies`部分指定其名称和版本：

```toml,no_run,noplaypen
[project]
name = 'caller'
type = 'contract'
version = '0.1.0'

[dependencies]
callee = '0.1.0'
```

然后，依赖项目将作为普通模块在主项目中可用。非常简单！

```rust,no_run,noplaypen
use callee::Callee;

contract Caller {
    pub value: u64;

    pub fn new(value: u64) -> Self {
        Self {
            value: value,
        }
    }

    pub fn create_and_transfer(mut self) {
        // creates an instance of contract `Callee`
        let mut instance = Callee::new(self.value / 2);
        
        // sends some tokens to the newly created instance
        self.transfer(instance.address, 0x0 as u160, 0.1_E18 as u248);
        
        // sends half of the tokens back to the creator
        instance.transfer(self.address, 0x0 as u160, 0.05_E18 as u248);
    }
}
```

## 库项目类型

`library` 项目只是类型和函数的集合，不能作为单独的项目运行。
相反，它可以上传到 Zandbox 并用作依赖项。要创建库，请使用 `library` 类型初始化一个项目：

```bash,no_run,noplaypen
zargo new --type library math
```

## 上传项目

要将您的项目上传到 Zandbox 数据库，只需使用 `zargo upload` 命令。
项目名称和版本必须是唯一的。

要检查哪些已被占用，请使用以下命令：

```bash,no_run,noplaypen
zargo download --list
```

## 下载项目

通常，默认情况下会下载所有项目依赖项并存储在相对于主项目根目录的`target/deps`目录中。

但是，有时您需要下载您或其他人的一些项目以进行有用的更改和调整。为此，请使用以下命令：

```bash,no_run,noplaypen
zargo download --name callee --version 0.1.0
```
