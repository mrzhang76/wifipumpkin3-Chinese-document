# 安装

## FAQ 安装

1、当我启动wp3时出现错误：

`ModuleNotFoundError: No module named 'PyQt5.sip'`

在进行一些分析后，我发现了这个问题的修复方法，你可以运行以下命令：

```text
sudo python3.7 -m pip install --upgrade PyQt5
```

现在，在`wifipumpkin3`文件夹你可以运行s`udo make test`命令来检查运行情况。如果日志中没有显示任何错误，此bug就被解决了。

