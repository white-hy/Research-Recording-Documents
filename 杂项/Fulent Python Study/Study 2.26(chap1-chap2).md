# Study Report ：Chap 1- Chap2

[TOC]

## 字体约定：

楷体表示新的术语，加粗等宽字体(**constant width bold**)表示由用户输入的命令或其它文本，等宽斜体(*Constant width italic*)表示由用户输入的值或者根据上下文确定的值替换的文本。

在学习过程中，大标题只应该与书中大标题出现的地方一样，重点可以用*+空格以重点的形式标出

## 第一章 Python 数据模型

### magic 与 dunder

dunder就是双下划线的意思。一般我们在python中使用带双下划线的方法，如\_\_getitem\_\_方法的时候，这个方法被称为特殊方法，叫做magic method，同时又称作双下划线方法

我们获取一个元素的本质方法是调用了`__getitem__`方法，在关于card的这个例子中，它告诉了我们如何自己构建一个能用[]来调用的类，以及这个类本质是怎么来的。三个magic method： `__init__,__len__,__getitem__`，这三个方法可以构建一个可以调用，可以迭代的类。正如我们的Dataloader中一样，我们需要做的3个方法和这里一模一样，返回了一个可以迭代的数据结构。

* 代码中的几个小tips:

  ```python
  list("ABCD")
  =>[A,B,C,D]
  a=[A,B,C,D]
  a.index("A")
  =>0 #return the index of the item in the list
  ```

* 如果我们有了一个有3个隐式方法的Class，也就是可以获取长度，可以获取值，且被初始化过，那么我们可以用sorted函数+一个特定的key进行排序。比如函数：

  ```python
  suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0) 
  def spades_high(card): 
  	rank_value = FrenchDeck.ranks.index(card.rank) 		return rank_value * len(suit_values) + suit_values[card.suit]
  ```

  这里我们相当于对这个类的每一个`__geiitem__`所返回的值有了一个函数，这个函数计算每一个值的价值，然后用sorted函数加key进行排序

#### 如何洗牌

先有一个概念，可以用`__setitem__`进行洗牌,也就是改变一个类里面元素的位置，比如打乱顺序等等。

### 使用特殊方法

刚刚在例子里面deck[]就是使用了特殊方法`__getitem(self,position)__`，而相对应的是函数`len(deck)`来得到特殊方法`__len__`的值。很多时候，特殊方法的调用是隐式的，比如`for i in x`调用了`x.__iter__()`方法一个一个输出。一般你不用直接调用特殊方法，python提供了很多隐式编程。大概只有`__init__`方法常常使用。

内置函数有`len,iter,str`等等，也就是str()这种。一般特殊函数速度更快。

在写程序的时候不要随意添加特殊方法，比如`__foo__`之类的，因为可能这个名字没有被python内部使用，但是后来可能被使用

#### 一个向量模拟（采用特殊方法，加法减法等）

```python
from math import hypot

class Vector:

    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def __repr__(self):
        return 'Vector(%r, %r)' % (self.x, self.y)

    def __abs__(self):
        return hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

    def __add__(self, other):
        x = self.x + other.x
        y = self.y + other.y
        return Vector(x, y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
```

这个方法可以实现：

```python
In [7]: v1=Vector(2,4)

In [8]: v1
Out[8]: Vector(2, 4)

In [9]: v2=Vector(2,1)

In [10]: v2+v1
Out[10]: Vector(4, 5)

In [11]: v2*3
Out[11]: Vector(6, 3)

In [12]: abs(v1)
Out[12]: 4.47213595499958
```

因此,`__abs__`可以认为是加法的特殊方法，`__mul__`可以认为是乘法的特殊方法，`__repr__`是print的一个返回方法.

`repr`是python的一个内置函数，可以把一个对象用字符串来表达出来。如果没有这个函数，那么我们`print(v1)`的时候就会输出一个`<Vector object at 0x10e100070>`。

在采用算数运算符的时候，我们写的时候要注意不改变操作对象的值，这是一个基本原则。

这里还提出了一个自定义的bool值。

### `len`是一个特殊方法，不是普通方法

如果x是一个内置类型实例，那么len（x）的速度很快，因为Cpython会直接从一个C结构体里读取对象的长度，完全不会调用任何方法。而获取元素数列是一个常见操作，在很多数据类型上，这个操作很高校

因此，len不是一个普通办法，但是为了让python自带的数据结构可以走后门，abs也是。但是它们是特殊方法，因此可以把len用于自定义数据类型，这种处理方式在保持效率与保证一致性平衡，这也就是python之禅的“不能让特例特殊到开始破坏既定规则”。

### 小节

通过这些特殊方法，自定义数据类型可以表现得和**内置类型**一样(比如可以自定义运算符)，从而写出更有python风格的代码。

python对象的一个基本要求是它需要具有合理的字符串表示形式，我们可以通过`__repr__,__str__`来满足，前者很方便调试

特殊方法可以模拟序列数据，尤其是自定义的序列数据，这个很重要。

## 序列构成的数组

对于序列数据，无论是哪种数据结构，比如字符串，列表，字典序列，数组，xml元素等都可以用一套迭代，切片，排序，拼接操作。

深入理解python不同序列类型，可以让我们避免重新发明轮子，也可以让我们把API设计得和原生序列一样。

### 内置序列类型概览

Python标准库有丰富序列类型：

容器序列：

list，tuple，collections.deque

这些序列可以存放不同类型的数据

扁平序列：

str，bytes，bytearray，memoryview，array。array都是只能容纳一种类型的序列

#### Collections介绍

collections在内置数据类型的基础上提供了几个额外的数据类型：

* namedtuple() 生成可以用名字访问的tuple子类
* deque()双端队列
* Counter:计数器
* OrderedDict：有序字典
* defaultdict:带有默认值的字典

可变序列与不可变序列

**列表推导与生成器表达式**

列表推导很好用，但有一个原则。一般只用它来创建新的列表，而且要尽量保持简短。如果列表推导式代码写超过了2行，那么就要考虑用for重写

* Python会忽略代码里[],{},()中的换行，因此如果你的代码里有多行列表等反正需要换行，可以省略`\`

列表推导不会有变量泄露的问题，也就是命名空间更加明确了

列表推导可以计算笛卡儿积，也可以生成列表。

列表推导只能生成列表，如果想生成其它类型的序列，那么可以用生成器表达式，比如

```python
tuple(ord(symbol) for symbol in symbols)
array.array('I', (ord(symbol) for symbol in symbols))
```

生成器表达式一般用一个()+列表表达式内部类似，这个生成的东西可以被for所迭代，它主要可以初始化列表外的序列，并避免占用额外内存。

#### 元组

**元组不仅仅是不可变列表**

元组存储模式是存储字段数据与字段的位置。它是一些字段的集合。

* 元组拆包

  比如`city, year, pop, chg, area = ('Tokyo', 2003, 32450, 0.66, 8014)`

* 用`*`来把一个可迭代对象拆开，比如*()表示把一个元组的元素分开，分别送入函数什么的。同时在元组拆包的时候\*可以用来代替一些字段，比如`city, year, *x, area = ('Tokyo', 2003, 32450, 0.66, 8014)`

* 嵌套元组拆包，被嵌套的元素用()来弄。

  ```python
  metro_areas = [ ('Tokyo','JP',36.933,(35.689722,139.691667)), # ➊ ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889)), ('Mexico City', 'MX', 20.142, (19.433333, -99.133333)), ('New York-Newark', 'US', 20.104, (40.808611, -74.020386)), ('Sao Paulo', 'BR', 19.649, (-23.547778, -46.635833)),
  ]
  print('{:15} | {:^9} | {:^9}'.format('', 'lat.', 'long.')) fmt = '{:15} | {:9.4f} | {:9.4f}' 
  for name, cc, pop, (latitude, longitude) in metro_areas: 
      if longitude <= 0: 
          print(fmt.format(name, latitude, longitude))

  ```

总之，记住元组是用来记录的，这个记录可以被拆包而送入几个变量中，这是很重要的。我们如何给记录命名呢？这里就可以用namedtuple来做了。

#### 具名元组

使用namedtuple来创建，2个参数，前面一个是名字，后面一个是字段，我们可以给字段赋值，用_fields来获取字段，用.make来接受一个可迭代对象，用\_asdict作为字典。

### 切片

切片本质是一个函数slice(a,b)，可以用a:b调用

### +与*

对列表使用*代表重复多次，如果重复的是列表，比如`[[]]\*3`这就代表了变成3个空列表，但是3个空列表指的是同一个空列表对象。

### += 与*=

在有可变元素的元组中这种操作可能会吃亏，因为会先改变可变元素再改变元组

### sorted与.sort

都可以用key，但是前者返回新对象，后者返回None表示是对自己的更改

### bisect

为了保持一个排好序的序列顺序不变的操作方式，可以用来获取保序下的插入位置，或者是插入并保序

### 替代列表的其它序列

array.array是一个

#### Numpy  and Scipy

Numpy自带存储功能，即可以把数组存入一个叫.npy的文件中。numpy处理千万级浮点数速度非常快。

#### 双向队列

利用append与pop方法，可以把列表当作栈或者队列来使用，但是删除列表第一个元素，或者添加元素很耗时间，因此collections.deque类是一个线程安全，可以从两端添加或删除元素的数据类型。



### 第二章总结

第二章主要讲python中的序列，包括可变，不可变序列，也可以分为扁平序列和容器序列。扁平序列体积小，速度快，用起来简单，但是只能保存数字，字符或者字节。容器序列比较灵活，但是容器序列遇到可变对象的时候需要小心。

元组的两个角色，无名称的字段记录与不可变列表。具名元组很有用，可以通过名字获取各个字段信息，也可以用._asdict()来变成有序字典类型。

序列切片是通过slice函数做的，非常有用。

