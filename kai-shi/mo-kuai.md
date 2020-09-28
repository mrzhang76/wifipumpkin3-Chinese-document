# 模块

模块提供了接入点并不必须的特性，模块应设计为攻击添加新的功能，就像 `devices discovery`, `services enumeration`, `perform deauthentication attacks`等。通过引入模块来增添攻击功能。

> 注意
>
> 模块的语法基本上遵循`metasploit`模块的结构

基础核心命令准则：

| 命令 | 描述 |
| :--- | :--- |
| set | 设置模块配置 |
| back | 回到上一级 |
| help | 显示可用命令 |
| options | 显示模块当前的配置 |
| run | 运行模块 |

  欢迎模块开发者和用户将模块加入到这个项目中，点击查看开发参考  [how to create a module](https://wifipumpkin3.github.io/docs/getting-started#development)

