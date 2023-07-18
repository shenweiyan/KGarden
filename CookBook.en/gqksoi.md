一些类型与编码解码使用示例。

```bash
# xlrd 中 sheet0.row_values(n) 的每个元素都是 unicode;
# Python 中如果执行 str+unicode 拼接会出现 UnicodeDecodeError 错误.
geneid = str(line[2]) if isinstance(line[2], float) else line[2].encode("utf-8")
```
