# 配置文件

`Wifipumpkin3` 支持`.ini`格式的配置文件。`~`是你的家目录，通常为`/home/username`。一个文件或文件夹名以字母为开头。`.`是linux隐藏文件/文件夹的标识。

因此`~/.config`是在你家目录中的一个隐藏文件夹。当`wp3`被安装后，`~/.config`中将包含`~/.config/wifipumpkin3`。文件结构类似于：

```text
$ ls 
 config  exceptions  helps  logs  scripts
```

> Ini 文件结构
>
> INI文件格式是一个计算平台和软件的配置文件的非正式标准。INI文件是一个具有节，属性和值的简单文本文件。

所有`.ini`格式文件都位于`config/app`文件夹中，让我们来看一下此文件夹中的文件结构

```text
.
├── captive-portal.ini
├── config.ini
├── dns_hosts.ini
├── pumpkinproxy.ini
└── sniffkin3.ini

0 directories, 5 files
```

主要的配置文件为`config.ini`，用于设置包括接入点、插件、模块、代理的所有信息与CLI接口中的全部变量

## DNS hosts（流氓DNS服务器）

host文件是用于进行DNS主机名解析的第一查找点的文件。wp3使用`Samuel Colvin @samuelcolvin`创建的python dns解析器实现这一功能

文件格式很简单。修改host dns文件需要添加两个条目，每个条目均包含你希望站点解析到的IP地址以及Internet地址的类型。~~简历中~~有一个恶意DNS服务器，用于将所需网站（搜索引擎，银行，broker等）的域名转换为具有意外内容的网站（甚至是恶意网站）的IP地址。

例如：

```text
example.com  A       10.0.0.1
example.com  CNAME   whatever.com
example.com  MX      ["whatever.com.", 5]
example.com  MX      ["mx2.whatever.com.", 10]
example.com  MX      ["mx3.whatever.com.", 20]
example.com  NS      ns1.whatever.com.
example.com  NS      ns2.whatever.com.
example.com  TXT     hello this is some text
example.com  SOA     ["ns1.example.com", "dns.example.com"]
```

如果你不更改dhcp范围ip地址，则所有请求客户端访问example.com的请求都将由localhost 10.0.0.1重定向。

## DHCP服务器配置

wp3像wp2一样拥有自己的DCHP工具，现在只需在`config.ini`文件中配置DHCP服务器。让我们看看它的配置文件

> DHCP 服务器
>
> DHCP服务器是一个网络服务，它自动的提供并分配IP地址，默认网关和其他网络参数给客户端设备

```text
[dhcpdefault]
broadcast=10.0.0.255
leasetimeDef=600
leasetimeMax=7200
netmask=255.0.0.0
range=10.0.0.20/10.0.0.50
router=10.0.0.1
subnet=10.0.0.0
```

到目前为止，这是网络IP类A的默认设置dhcp服务器，你可以根据需要设置类B或C。在config.ini中有两个IP地址类型的示例

