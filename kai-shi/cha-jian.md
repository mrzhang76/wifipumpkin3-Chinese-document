# 插件

插件被设计用于向`WP3`核心添加特性并与接入点\(`AP`\)一起运行，`WP3`提供了开发插件的工具。一般来说，为了让插件正常工作，你还是要付出一些努力的。

> 注意
>
> 因为这些插件被设计成只监视和分析接入点上的用户产生的流量，你可以同时运行多个插件。

获取插件的基本命令准则是：

如果要启用或禁用插件，请遵循以下命令

```text
wp3 > set plugin plugin_name true/false
```

如果插件有子插件，当使用插件时你将看到一些可设置的子选项。你可以使用命令启用/禁用子插件，使用`TAB`来自动填充

```text
wp3 > set plugin_name.subplugins_name true/false
```

欢迎插件开发者和用户将插件加入到这个项目中，点击查看开发参考  [how to create a plugin](https://wifipumpkin3.github.io/docs/getting-started#development)

