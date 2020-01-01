# shell实用的命令

## 复制到粘贴板

> 注意`pbcopy`不是bash自带命令，需要检查是否有该命令，可能需要单独安装。

```bash
echo "hello word" | pbcopy
```

输出的`hello world`会自带换行。

如果不想要换行，还可以：

```bash
echo "hello word" |tr -d "\n"| pbcopy
```

***注意：这个会将所有换行删除的***
