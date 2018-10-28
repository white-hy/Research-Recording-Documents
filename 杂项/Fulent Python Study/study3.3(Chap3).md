# Study 3.3

[TOC]

## Chap3

1. 常见字典方法
2. 如何处理查不到的键
3. 标准库字典类变种
4. set与Frozenset类
5. 散列表

**只有可散列的数据类型可以作为映射里的键**

所谓可散列就是有实现hash方法与qe方法。如果两个可散列对象相等，那么散列值相等。原子不可变数据类型(str,bytes,数值类型)都是可散列的。Frozenset也是，元组的话只有当元组包含的所有元素都是可散列的，元组才是可散列的。

一般用户自定义的类型对象都是可以散列的，散列值就是id()返回值

### 字典推导

```python
>>> DIAL_CODES = [ ➊
... (86, 'China'),
... (91, 'India'),
... (1, 'United States'),
... (62, 'Indonesia'),
... (55, 'Brazil'),
... (92, 'Pakistan'),
... (880, 'Bangladesh'),
... (234, 'Nigeria'),
... (7, 'Russia'),
... (81, 'Japan'),
... ]
>>> country_code = {country: code for code, country in DIAL_CODES} ➋
>>> country_code
{'China': 86, 'India': 91, 'Bangladesh': 880, 'United States': 1,
'Pakistan': 92, 'Japan': 81, 'Russia': 7, 'Brazil': 55, 'Nigeria':
234, 'Indonesia': 62}
>>> {code: country.upper() for country, code in country_code.items() ➌
... if code < 66}
{1: 'UNITED STATES', 55: 'BRAZIL', 62: 'INDONESIA', 7: 'RUSSIA'}
```

也就是说可以和list一样，但是需要{A:B}来做。

一些有用的方法：

```python
d.clear()
d.copy()
del d[k]
d[k]
d.items()# 返回所有的键值对
d.keys()
len(d)
d.setdefault(k)# 如果有键k，将它的值设为default，返回它。如果没有，那么设default，然后返回default
d.values()
```

#### setdefault的妙用：

```python
my_dict.setdefault(key,[]).append(new_value)
if key not in my_dict:
	my_dict[key]=[]
my_dict[key].append(new_value)
```

上面一行与下面是一样的，这样就可以一次查找展示defalut了。

setdefault本质就是查找一个key值对应的元素，如果没有key的话就把元素变为key后的默认值，然后返回这个元素，然后我们可以对元素进行操作，比如append一个元素等等，这样可以减少键查询的次数。



### 处理查不到的键

使用defaultdict，这个字典结构可以在查不到的键时调用`default_factory`，然后实际上这个特性所有映射类型都可以支持。

我们可以自己定义一个`StrKeyDict`将输入的键全部转换为str，其实本质就是继承了dict后又写了一个miss方法

注意，调用misssing方法后有可能产生递归循环调用错误，因此我们用contains来找键位的时候应该加上 a in .keys() or str(a) in .keys()

### 字典变种

`collections.OrderedDict`

有序字典，添加键的时候保持次序。

`collections.ChainMap`

`collections.Counter`

这个映射会给键准备一个整数计数器，每次更新一个键的时候都会增加这个计数器，这个计数器可以给可散列表对象计数，或者用在多重集中。比如计算单词中字母出现字数：

```python
>>> ct = collections.Counter('abracadabra')
>>> ct
Counter({'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
>>> ct.update('aaaaazzz')
>>> ct
Counter({'a': 10, 'z': 3, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
>>> ct.most_common(2)
[('a', 10), ('z', 3)]
```

### UserDict

与Dataloade一样，给出了几个简单的东西，在继承UserDict的基础上进行增添。

## 不可变映射类型

采用`MappingProxyType`来进行设置

## 集合

set与不可变集合frozenset



**集合中的元素必须是可以散列的** ，set本身是不可散列的，但是frozenset可以，因此如果要集合套集合，只能用Frozenset

如果是空集，那么只能写成set()形式，同时集合一般是用`{item1,item2,item3}`

来初始化的，这样要比set函数快。因为如果是`set([])`那么必须先新建列表，再传入集合。但是用`{}`构造的话，就会利用一个`BUILD_SET`的字节码创建集合。

集合推导式：

集合有一些运算符：

`&,&=`注意乐园是可以迭代的it

并，`|,|=`

差集

对称差集

## 散列表的本质

效率有多高

为什么是无序的

为什么不是所有python对象都可以被当成键与元素

为什么dict键与set元素的顺序是根据被添加的次序而定的

为什么不能迭代的同时增加元素

散列是一个总有空白元素的数组（稀疏数组），单元叫表元。

对键的引用与对值的引用。

散列值是散列表的索引。



获取字典的key后的值，python调用hash函数来计算散列值，把值最低的几位数字作为偏移量查找表元。如果表元是空的，那么key Error，如果不是空的，返回一堆found_key:found_value，如果search与found相同，那么返回value

字典使用了散列表，散列表是稀疏的，因此空间效率低下。因此存放数量巨大的记录，那么存在元组，或者具名元素更好。

次序不同问题：

键次序取决于添加次序，添加新键可能会遇到散列冲突，因此会被安排到另外一个位置。

* 代码里有一个`dict(sorted(DIAL_CODES,key=lambda x:x[1]))`

往字典里加新键会导致字典扩容，新建更大的散列表并添加，在这个过程中会遇到散列冲突。因此不能对字典同时进行迭代与修改，先迭代，建立一个新字典，再修改

### Set的实现

set与frozenset的实现也依赖于散列表，但是它们存放的只有元素的引用。因此，这些数据结构都需要能散列的。因此，这些数据结构都是以内存换查询时间与高效判断。元素次序取决于添加次序，改变集合会改变次序

特殊映射类型，UserDict模块。setdefault能更新字典存放的可变值，避免重复搜索，而update是批量更新。

## 总结：

setdefault可以快速更新字典存放的可变值，update是批量更新，或者更新已有键值对。__missing__方法可以自定义找不到某个键会发生什么。

不可变映射对象

散列表效率



