# String Literals

字符串是二进制或字符的序列，在单引号\('\)或多引号\("\)。例如：

```
’a string‘
"another string"
```

紧挨着的多个字符会被替换成一个单一的字符串。下面的两行是等价的：

```
’a string‘
'a' ' ' 'string'
```

如果ANSI\_QUOTES SQL模式被激活，字符串可以仅被单一的字符串包围。因为一个由双引号包围的字符串会被解释为标识符。

二进制字符串是字符串的二进制形式，每个二进制字符串具有一个字符集和较验集命名为binary.A nonbinary string is a string of characters.It has a character set other than binary and a collation that is compatible with the character set.



