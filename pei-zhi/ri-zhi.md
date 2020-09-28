# 日志

## 概述

`wp3`有一个系统日志管理器将日志保存在`.json`格式文件中，或者你可以在`config.ini`中自定义像`wp2`一样保存简单日志，详情查看 [config](https://wifipumpkin3.github.io/docs/configuration/configuration-file)

{% page-ref page="pei-zhi-wen-jian.md" %}

输出的`.json`文件内包含很多会话信息包含日期、时间戳、会话ID，包的额外信息等。如果选择不保存`.json`文件，则只保存`wp3`接口上的进程生成日志

## 会话ID

如果你想在继续进行某次攻击，可以恢复会话ID。这种功能非常强大，可以恢复会话以获取出色的日志报告，此功能将很快添加

## 日志本地化

`wp3`生成的所有日志唯一的保存在`logs`文件夹中，`~`是你的家目录，通常为`/home/username`。一个文件或文件夹名以字母为开头。`.`是linux隐藏文件/文件夹的标识。

因此`~/.config`是在你家目录中的一个隐藏文件夹。当`wp3`被安装后，`~/.config`中将包含`~/.config/wifipumpkin3`。文件结构类似于：

```text
$ cd ~/.config/wifipumpkin3/
$ ls 
 config  exceptions  helps  logs  scripts
```

```text
$ cd ~/.config/wifipumpkin3/logs/ap
$ ls
captiveportal.log  pumpkin_proxy.log  pydns_server.log  responder3.log  sniffkin3.log
```

日志的扩展名是`.log`，在默认情况下，文件中的所有数据都保存为`json`对象。让我们看看包括所有捕获的http数据包的文件示例

```text

  "text": "2020-04-09 at 19:23:29 INFO - [ 10.0.0.21 > 172.217.30.99 ] b'GET' b'connectivitycheck.gstatic.com'b'/generate_204'\n",
  "record": {
    "elapsed": {
      "repr": "0:00:13.495115",
      "seconds": 13.495115
    },
    "exception": null,
    "extra": {
      "dns": 144,
      "session": "b52e2300-7ab0-11ea-95ed-94e979fa917b",
      "packet": {
        "urlsCap": {
          "IP": {
            "options": [
            ],
            "version": 4,
            "ihl": 5,
            "tos": 0,
            "len": 284,
            "id": 44754,
            "flags": "",
            "frag": 0,
            "ttl": 64,
            "proto": 6,
            "chksum": 62904,
            "src": "10.0.0.21",
            "dst": "172.217.30.99"
          },
          "Headers": {
            "Headers": "b'User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.32 Safari/537.36\\r\\nHost: connectivitycheck.gstatic.com\\r\\nConnection: Keep-Alive\\r\\nAccept-Encoding: gzip'",
            "Host": "b'connectivitycheck.gstatic.com'",
            "User-Agent": "b'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.32 Safari/537.36'",
            "Accept-Encoding": "b'gzip'",
            "Connection": "b'Keep-Alive'",
            "Method": "b'GET'",
            "Path": "b'/generate_204'",
            "Http-Version": "b'HTTP/1.1'"
          }
        }
      },
      "name": "sniffkin3",
      "specific": true
    },
    "file": {
      "name": "logger_manager.py",
      "path": "/usr/local/lib/python3.7/dist-packages/wifipumpkin3-1.0.0-py3.7.egg/wifipumpkin3/core/widgets/default/logger_manager.py"
    },
    "function": "info",
    "level": {
      "icon": "ℹ️",
      "name": "INFO",
      "no": 20
    },
    "line": 149,
    "message": "[ 10.0.0.21 > 172.217.30.99 ] b'GET' b'connectivitycheck.gstatic.com'b'/generate_204'",
    "module": "logger_manager",
    "name": "wifipumpkin3.core.widgets.default.logger_manager",
    "process": {
      "id": 10809,
      "name": "MainProcess"
    },
    "thread": {
      "id": 140676164286272,
      "name": "MainThread"
    },
    "time": {
      "repr": "2020-04-09 19:23:29.459485-03:00",
      "timestamp": 1586471009.459485
    }
  }
}
```

如上文所示，格式化文件的每一行都包含一个`json`结构数据

## JSON 格式

wp3使用python模块`loguru`，这是一个非常牛逼的用于系统日志记录的模块。在配置文件config.ini中可以通过修改一个键值使用传统的日志模式。这种`json`格式的很适合API读取，此功能将在之后的版本中添加

```text
# get file on ~/.config/wifipumpkin3/config/app/config.ini
[settings]
log_colorize=true # for get colors when print output on CLI interface
log_serialize=true # for diasble json format only change for false
```

