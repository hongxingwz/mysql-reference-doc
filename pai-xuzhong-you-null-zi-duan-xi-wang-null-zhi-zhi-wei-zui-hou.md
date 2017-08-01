# 排序中有null字段希望null值置为最后面

## 如有如下表

![](/assets/import-2017-08-01-02.png)

## 方式一：利用ISNULL函数

ISNULL\(NULL\)       返回 1

ISNULL\('非null值'\) 返回 0



**select \* from nums order by isnull\(num\) asc, num asc**

这样子null值就永远在最后面了



## 方式二：利用asc null值在最上面， desc null值在最下面的特性

desc null值在最下面的特性

**select \* from nums order by -num desc; **



## 方式三:得用coalesce\(..\)函数

coalesce\(\)函数  如果所有参数都为null ，则返回null。 否则返回第一个遇到的非null值

**select \* from nums order by coalesce\(num, 10000\) asc;**







