对于需要打包压缩的目录、文件，如果该目录或者文件不可读，将会引发 Permission denied 的错误。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1582271966302-401a500a-9d5e-4840-80c2-c0787b8e5dd6.png#align=left&display=inline&height=495&originHeight=495&originWidth=814&size=54677&status=done&style=none&width=814)

如果需要检查一个目录打包是否出现因为权限导致的异常，可以检查 log 日志是否存在  failure/Permission denied/error 等关键字。

```bash
tar zvcf 2014060503104SSQ.tar.gz 2014060503104SSQ 1>2014060503104SSQ.log 2>&1
grep -i failure 2014060503104SSQ.log
```

![comment.gif](https://cdn.nlark.com/yuque/0/2020/gif/126032/1582274153199-1eb3c355-3155-4b42-bd66-919dfc7da1a7.gif#align=left&display=inline&height=157&originHeight=157&originWidth=164&size=29605&status=done&style=none&width=164)
