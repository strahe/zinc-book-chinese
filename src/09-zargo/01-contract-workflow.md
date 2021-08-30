# 合约工作流程

此代码片段描述了创建、构建、发布智能合约和调用其方法的工作流程。

```bash,no_run,noplaypen
# create a new contract called 'swap'
zargo new --type contract swap
cd swap/

# write some code

# rebuild, publish the contract, and get its address
zargo publish --instance default --network rinkeby

# query the newly created contract storage
zargo query --address <address>

# call some contract method
zargo call --method exchange --address <address>
```

## Manifest 文件

Zinc 智能合约在清单文件`Zargo.toml`中描述，其结构如下：

```toml,no_run,noplaypen
[project]
name = 'test'
type = 'contract'
version = '0.1.0'
```
