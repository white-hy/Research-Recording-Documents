#         PEP8 命名风格学习

## 整体概念：

对于一个别人能看到的文件，重要的应该是让别人知道怎么用而不是怎么实现。

## 命名方式

前面为命名样例，后面为命名类型。

1. b (单独的小写)
2. B (单独一个大写)
3. lowercase（整体小写）
4. lower_case_with_underscores(带下划线小写)
5. UPPERCASE（整体大写）
6. UPPER_CASE_WITH_UNDERSCORES(带下划线大写)
7. CapitalizedWords（单词首字母大写）
8. mixedCase（一个大写一个小写）
9. Capitalized_Words_With_Underscores（超丑）

属性(e.g. property)与方法名称以一个对象为前缀，函数以模块为前缀。

此外，以下特殊的形式是可以识别的，它们一般可以与任意的例子结合，作为惯例

1. _single_leading_underscore

   这表示开头有一个小的下划线，表示一个私有函数，一般from ... import * 不会import进来，ipython也不会提示，但是可以采用from ... import 名称 直接调用出来

2. single_trailing_underscore_

   这表示尾部有一个小下划线，为了避免与keyword冲突，比如```class_='ClassName'```

3. __double_leading_underscore:

   首部有2个下划线一般多用于class中，表示一个只能在class内部调用的变量，不能被直接.__调用，会报错，但是class内可以用

4. \_\_double_leading_and_trailing_underscore\__:比如\_\_init\_\_,\_\_import\_\_等

   这些名字只是依据文档调用，不要发明

## 有用的命名约定

不要用容易混肴的单个字母'l','O','I'等命名，如果要使用'l'，用"L"代替

必须用ASCII能表示的字符。

### 包和模块名

（package就是你要从什么里import什么，模块可以在工程中理解为很多函数组成的一个文件）

module的名字应该是简短的，所有都是lowercase的名字。如果下划线能够增加可读性，那么用下划线。同时，package的名字也应该是短的全部都是小写的，**不鼓励下划线** 。

用C与C++写的扩展模块开头可以有一个下划线

### Class Names

类名应该使用CapWords，也就是大小写结合的。

如果有一些函数满足1.接口被记录了下来，2.只是被私密调用的话可以用CapWords。如果是公开调用的，用下面函数的命名规则(lowercase)

注意有一些内建函数，比如abs(),all(),any(),min(),sorted(),list()等等，这些内建函数都是一个单词，或者是两个一起的单词，此时我们可以用CapWords来区别于这些内建函数构造例外的函数。比如我们想自己写一个list函数，我们应该用**List()**， 而一些内置常量也可以用CapWords命名。            

### 异常命名

异常应该是类，因此类命名约定适用。但是如果异常名称上使用错误的话，应该加后缀Error

### 全局变量

全局变量应该也是CapWords型的，与Class Names相似

### 函数与变量名

函数名应该是小写字母，如果可以增加可读性，用下划线。

变量名与函数名采用相同的约定。

而mixedCase这种大小写都有的命名只有在“你要对老代码维护，而老代码有很多这种奇怪的命名的时候”使用，以增加向后兼容性，其它情形下不鼓励。

### 函数与方法的参数

方法就是类内函数。类内的方法分为类方法，静态方法和实例方法。

类方法当我们需要直接调用整个类的时候可以用，比如说

```python
class Date(object):

    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        date1 = cls(day, month, year)
        return date1

    @staticmethod
    def is_date_valid(date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        return day <= 31 and month <= 12 and year <= 3999

date2 = Date.from_string('11-09-2012')
is_date = Date.is_date_valid('11-09-2012')
```

这里类方法就可以直接新创一个类，而静态方法没有必要加入self

实例方法第一个参数必须是self，而类方法第一个参数必须是cls

如果说我们需要用一个参数变量class，那么我们一般采用class_代替class，也就是用后下划线代替缺一个的clss

### 方法命名与实例变量

方法命名与函数命名一致，使用小写字符+能保证可读性的下划线

对于非公开（不可直接调用）的方法与实例参数，命名前加一个下划线

为了不与子类名字冲突，有一些只是本类内使用的实例变量与方法用2个短下划线开头命名（这样不可被调用，只能在类内使用）

### 常量命名

常量一般在Module的Level上被def（比如一个``.py`文件的开头，一般我们用CAPITAL_WORDS来定义，全大写+下划线分割单词）

### 设计继承

我们的代码常常会出现一些继承的东西，因此我们在写类的时候一定要想明白哪些类方法与实例变量是public或者是no-public的。如果我们对一个方法的公开性很疑惑，用non-public

我们设定一个方法是public的，如果我们认为这个方法会被不同的，互不相关的客户所调用，同时他们调用所产生的结果不会对兼容性有影响。（也就是A不影响B使用，B不影响C使用）

Non-public指代于那些**不希望**被第三方使用的方法。但是用Non-public不等于不可更改或者不可移除。

注意，Python里没有private这个概念，因为所有的attribute都并不是真的private的。

另一类属性是子类API的一部分，这就是继承与扩展。在这个时候对于public的推断要更加严谨。

因此，在这个思想上有以下pythonic的指导

1. 公开属性没有下划线

2. 如果公开属性与关键词冲突，在后面加下划线

3. Python的优势在于可扩展性，因此对于那些简单的公共数据属性，我们应该仅公开这些属性的名称，而不设计复杂的访问方法。如果我们要扩展一个属性，使用property来设计获得它的接口。

   注意，property仅在新式class中使用。在需要大量计算的操作的时候避免使用property。property给人的感觉应该是访问是简单的。

4. 如果类要被继承，考虑对不用扩展的部分用双下划线，不要使用尾下划线。

### 设计接口

当我们设计公共接口的时候应该考虑向后兼容性，因此，我们需要让公共接口与内部接口良好区分。

记录于文档中的接口应该是公共的，除非文档特殊声明。不声明的应该假设为内部的。

为了更好内部检查，一个module应该使用_\_all__属性在API中明显地声明名称。将all设为空表示该模块没有公共的API（就是不是外界调用的，而是内部作用）

(所谓的\_\_all\_\_就是一个list，这个list含有from A import *中import的所有东西的list，如

```python
import numpy as np
print(np.__all__)
```

出来一个np里所有可调用的函数

)



### 对于import包函数

不要用from ... import a as 语句，除非a与另外一个包有重复，此时as后加 as a_

如果只是用1个函数，那么直接import到函数名，如果要用到一个部分下的多个函数，那么import上一级然后使用.

