# 使用

## 交互窗口

一旦使用`sudo wifipumpkin3`启动工具，你将看到像`metasploit framework` 的一个交互式会话，这里你可以启用或禁用模块，插件，代理配置AP等等。

CLI界面非常简单，你将使用一些基本命令来执行操作，如设置一个接入点的会话信息（bssid, channel, interface），启动/终止接入点和监视用户的活动。

## Pulps

`Pulps`指的是从南瓜中提取的果肉，可以被用于各种混合物。可以使用pulps文件编写你的交互式会话脚本。`Pulps`（带有`.pulp`后辍的脚本文件）是一个很有力的方式去进行你的自动化攻击，就像`metasploit`的`.rc`文件，其中的每一行都是一条将逐步执行的命令

让我们看看如何创建一个脚本来进行框架配置，设置接口，进行无代理启动，设置网络ssid，配置dns的无日志工作模式并启动接入点。

```text
# configure the interface
set interface wlan1 
# set name of access point will be created
set ssid demo
# set noproxy plguin
set proxy noproxy
# ignore all log from pydns_server 
ignore pydns_server
# start the Access Point
start
```

将脚本另存为demo.pulp文件后，你可以通过以下方式执行： 

```text
sudo wifipumpkin3 --pulp /path/to/demo.pulp
```

如果你并不想使用pulp文件，可以选择使用参数`-xpulp`或`-x`，每个命令可以单独执行或以`;`分隔。例如：

```text
sudo wifipumpkin3 --xpulp "set interface wlan1; set ssid demo; set proxy noproxy; start"
```

## 参数命令

最基础的命令行参数（`wifipumpkin3 -h`）为

```text
-i INTERFACE 
```

要绑定到的网络接口，如果为空，则使用上一次会话的接口

```text
-s SESSION
```

攻击会话的ID，如果你输入了旧的会话ID，所有的日志将会添加进这一会话中

```text
--pulp PULP
```

交互式会话可以使用.pulp文件编写的脚本，这是一种自动执行攻击的强大方法

```text
--xpulp XPULP
```

每个命令可以单独执行，或者使用`;`分隔

```text
--wireless-mode WIRELESS_MODE
```

使用此选项来设置无线模式（`static,docker`），默认为`static`模式，你可以更改此选项以运行在docker平台上

```text
--no-colors
```

禁用终端颜色和效果

```text
-v, --version
```

查看程序版本并退出

## 核心命令

> help

列出所有可用的命令

> clients

使用高级UI显示所有接入点上所有用户连接

> ap

显示设置AP的所有变量和状态。包括（ `bssid`, `ssid`, `channel`, `security`, 或 `status ap`）

> set

设置可变代理，插件和接入点，此命令类似于`metasploit`的`set`命令

> start

启动接入点，如果一切设置恰当，`proxy`，`plugin`初始化后，你将看到带有`hostapd`程序的新AP

> stop

终止在后台运行的接入点，进程，线程，插件和代理

> ignore

消息日志将被忽略，命令参数可为（ `captiveflask`, `pumpkinproxy`, `pydns_server` `sniffkin3`）。如果你使用了此命令将不会在WP窗口中看到任何日志。

> restore

还原消息日志，此命令与`ignore`相反，参数相同，将使控制台日志还原

> info

获得代理和插件信息，此命令将展示一些代理/插件信息像（日志路径，配置路径，描述\)

> jobs

查看所有后台运行的进程/线程。有时需要查找`WP`创建的进程的ID或线程名

> mode

显示所有可用的无线模式

> plugins

显示所有可用的插件和它们的状态

> proxies

显示所有可用的代理和它们的状态

> show

显示可用的模块

> search

按名称搜索模块，这将在有百万级模块的时候使用（滑稽）

> use

从模块集中选择模块，灵感完全来自于`metasploit`模块

> dump

转储连接在接入点上用户的信息

> update

从远程git库中更新





