# python 内置函数整理  

参数解读(在大多数情况下)：

- x 表示数值类型。
- iterable 表示可迭代对象。
- object 表示对象。

## 数值处理

### abs()

用法：abs(x)

返回 x 的绝对值

### divmod()

用法：`divmod`(*a*, *b*)

它将两个（非复数）数字作为实参，并在执行整数除法时返回一对商和余数。

```
>>> divmod(19,3)
(6, 1)
```

### pow()

用法：`pow`(*x*, *y*[, *z*])

返回 *x* 的 *y* 次幂；如果 *z* 存在，则对 *z* 取余（比直接 `pow(x, y) % z` 计算更高效）。两个参数形式的 `pow(x, y)` 等价于幂运算符： `x**y`。

### round()

用法：`round`(*number*[, *ndigits*])

返回 *number* 舍入到小数点后 *ndigits* 位精度的值。 如果 *ndigits* 被省略或为 `None`，则返回最接近输入值的整数。

## 序列操作

### all()

用法：all(*iterable*)

如果 *iterable* 的所有元素均为真值（或可迭代对象为空）则返回 `True` 。

### any()

用法：`any`(*iterable*)

如果 *iterable* 的任一元素为真值则返回 `True`。 如果可迭代对象为空，返回 `False`。

`any` 与 `all` 可以用来判断序列元素是否为真。

### enumerate()

用法：`enumerate`(*iterable*, *start=0*)

返回一个枚举对象。*iterable* 必须是一个序列，或 [iterator](https://docs.python.org/zh-cn/3.6/glossary.html#term-iterator)，或其他支持迭代的对象。

```
>>> e = enumerate([1,2,3,4])
>>> type(e)
<class 'enumerate'>
>>> e
<enumerate object at 0x000002901E94D5A0>
>>> list(e)
[(0, 1), (1, 2), (2, 3), (3, 4)]
```

	### filter()

用法：`filter`(*function*, *iterable*)

用 *iterable* 中函数 *function* 返回真的那些元素，构建一个新的迭代器。

```
>>> f = filter(lambda x:x>5,[1,3,5,7,9])
>>> f
<filter object at 0x000002901E8F5A20>
>>> list(f)
[7, 9]
```

### len()

用法：`len`(*s*)

返回对象的长度（元素个数）。实参可以是序列（如 string、bytes、tuple、list 或 range 等）或集合（如 dictionary、set 或 frozen set 等）。

### map()

用法：`map`(*function*, *iterable*, *...*)

返回一个将 *function* 应用于 *iterable* 中每一项并输出其结果的迭代器。 如果传入了额外的 *iterable* 参数，*function* 必须接受相同个数的实参并被应用于从所有可迭代对象中并行获取的项。

```
>>> list(map(lambda x:x**2,[1,2,3]))
[1, 4, 9]
>>> list(map(lambda x,y:x+y,[1,2,3],[4,5,6,7]))
[5, 7, 9]
```

### max()

用法：`max`(*iterable*, *[, *key*, *default*])  

`max`(*arg1*, *arg2*, **args*[, *key*])

返回可迭代对象中最大的元素，或者返回两个及以上实参中最大的。

```
>>> max(['ab','ba'])
'ba'
>>> max(['ab','ba'],key=lambda a : a[1])
'ab'

>>> max('ab','ba')
'ba'
>>> max('ab','ba',key=lambda a:a[1])
'ab'
```

### min()

用法：与 max 相同。

### reversed()

用法：`reversed`(*seq*)

返回一个反向的 [iterator](https://docs.python.org/zh-cn/3.6/glossary.html#term-iterator)。 

### sorted()

用法：`sorted`(*iterable*, ***, *key=None*, *reverse=False*)

根据 *iterable* 中的项返回一个新的已排序列表。*key* 指定带有单个参数的函数，用于从 *iterable* 的每个元素中提取用于比较的键。*reverse* 为一个布尔值。 如果设为 `True`，则每个列表元素将按反向顺序比较进行排序。

### sum()

用法：`sum`(*iterable*[, *start*])

从 *start* 开始自左向右对 *iterable* 中的项求和并返回总计值。一般应用于数字序列。

### zip()

用法：`zip`(**iterables*)

创建一个聚合了来自每个可迭代对象中的元素的迭代器。直到有一个可迭代对象没有元素时停止。

```
>>> z = zip([1,2,3,4],('a','b','c'))
>>> list(z)
[(1, 'a'), (2, 'b'), (3, 'c')]
```



## 对象操作

### ascii()

用法：ascii(object)

将对象转换为一个可打印的字符串，如 `ascii([1,2,3])`，返回为 `'[1, 2, 3]'`。

### callable()

用法：`callable`(*object*)

如果参数 *object* 是可调用的就返回 [`True`](https://docs.python.org/zh-cn/3/library/constants.html#True)，否则返回 [`False`](https://docs.python.org/zh-cn/3/library/constants.html#False)。如果返回 `True`，调用仍可能失败，但如果返回 `False`，则调用 *object* 将肯定不会成功。 

### iter()

用法：`iter`(*object*[, *sentinel*])

返回一个 [iterator](https://docs.python.org/zh-cn/3.6/glossary.html#term-iterator) 对象。

```
>>> i = iter([1,2,3,4,5])
>>> i
<list_iterator object at 0x000002901DE06A58>
>>> next(i)
1
```

## 进制转换

### bin() 

用法：bin(x)

将一个整数转变为一个前缀为“0b”的二进制字符串。

### hex()

用法：hex(x)

将整数转换为以“0x”为前缀的小写十六进制字符串。

### oct()

用法：`oct`(*x*)

将一个整数转变为一个前缀为“0o”的八进制字符串。

## 类型转换

### bool()

用法：bool(object)

返回一个布尔值，`True` 或者 `False`。一个对象在默认情况下均被视为真值，会被视为假值的内置对象:

- 被定义为假值的常量: `None` 和 `False`。
- 任何数值类型的零: `0`, `0.0`, `0j`, `Decimal(0)`, `Fraction(0, 1)`
- 空的序列和多项集: `''`, `()`, `[]`, `{}`, `set()`, `range(0)`

### bytes()

用法：`bytes`([*source*[, *encoding*[, *errors*]]])

返回一个新的“bytes”对象， 是一个不可变序列，包含范围为 `0 <= x < 256` 的整数。

可选形参 *source* 可以用不同的方式来初始化数组：

- 如果是一个 *string*，则必须提供 *encoding* 参数（*errors* 参数仍是可选的）；bytes 会使用 [`str.encode()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#str.encode) 方法来将 string 转变成 bytes。如 ` bytes('aaaaa',encoding='utf8')`，结果为 `b'aaaaa'`。
- 如果是一个 *integer*，会初始化大小为该数字的数组，并使用 null 字节填充。如 ` bytes(5)`结果为`b'\x00\x00\x00\x00\x00'`。
- 如果是一个符合 *buffer* 接口的对象，该对象的只读 buffer 会用来初始化字节数组。
- 如果是一个 *iterable* 可迭代对象，它的元素的范围必须是 `0 <= x < 256` 的整数，它会被用作数组的初始内容。如 `bytes([1,2,3])` 结果为 `b'\x01\x02\x03'`。

### bytearray()

用法：`bytearray`([*source*[, *encoding*[, *errors*]]])

返回一个新的 bytes 数组。 bytearray 类是一个可变序列。参数和 bytes 函数相同。

### **complex()**

用法：*class* `complex`([*real*[, *imag*]])

返回值为 *real* + *imag**1j 的复数，或将字符串或数字转换为复数。

```
>>> pl = complex(1,2)
>>> pl
(1+2j)
```

### dict()

用法：`dict`(***kwarg*)

*class* `dict`(*mapping*, ***kwarg*)

*class* `dict`(*iterable*, ***kwarg*)

创建一个字典类型。

```
>>> dict({'three': 3, 'one': 1, 'two': 2})
{'three': 3, 'one': 1, 'two': 2}

>>> dict(a=1,b=2,c=3)
{'a': 1, 'b': 2, 'c': 3}

>>> dict([('a',1),('b',2),('c',3)])
{'a': 1, 'b': 2, 'c': 3}
```

### float()

用法：`float`([*x*])

返回从数字或字符串 *x* 生成的浮点数。

```
>>> float(1)
1.0
>>> float(1.1)
1.1
>>> float('-1.1')
-1.1
>>> float('      -1.1')
-1.1
```

### frozenset()

用法：

返回一个新的 [`frozenset`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#frozenset) 对象，它包含可选参数 *iterable* 中的元素。是不可变集合对象。

```
>>> fs = frozenset([1,2,3,4,1])
>>> fs
frozenset({1, 2, 3, 4})
>>> type(fs)
<class 'frozenset'>
>>> hash(fs)
5575258175646371796
```

### int()

用法：`int`(*x*, *base=10*)

返回一个使用数字或字符串 *x* 生成的整数对象，或者没有实参的时候返回 `0` 。如果 *x* 不是数字，或者有 *base* 参数，*x* 必须是字符串、[`bytes`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#bytes)、表示进制为 *base* 的 [整数字面值](https://docs.python.org/zh-cn/3.6/reference/lexical_analysis.html#integers) 的 [`bytearray`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#bytearray) 实例。

```
>>> int()
0
>>> int(2.6)
2
>>> int('111',2)
7
>>> int('111',8)  
73                
>>> int('111',16) 
273               
```

### list()

用法：`list`([*iterable*])

将 iterable 转换为 list 形式。

### range()

用法：`range`(*stop*)

`或 range`(*start*, *stop*[, *step*])

返回一个 range 对象

```
>>> r = range(1,10,2)
>>> r
range(1, 10, 2)
>>> list(r)
[1, 3, 5, 7, 9]
```

### set()

用法：`set`([*iterable*])

返回一个新的 [`set`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#set) 对象，可以选择带有从 *iterable* 获取的元素。

### slice()

用法：`slice`(*stop*)

或 `slice`(*start*, *stop*[, *step*])

返回一个表示由 `range(start, stop, step)` 所指定索引集的 [slice](https://docs.python.org/zh-cn/3.6/glossary.html#term-slice) 对象。

```
>>> s = slice(1,5,2)
>>> s
slice(1, 5, 2)
>>> type(s)
<class 'slice'>
>>> [1,2,3,4,5][s]
[2, 4]
```

### str()

用法：`str`(*object=''*)

或 `str`(*object=b''*, *encoding='utf-8'*, *errors='strict'*)

返回一个 [`str`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#str) 版本的 *object* 。

### tuple()

用法：`tuple`([*iterable*])

返回一个元祖。

### type()

用法：`type`(*object*)

或 `type`(*name*, *bases*, *dict*)

传入一个参数时，返回 *object* 的类型。 返回值是一个 type 对象。

传入三个参数时，返回一个新的 type 对象。*name* 字符串即类名并且会成为 [`__name__`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#definition.__name__) 属性；*bases* 元组列出基类并且会成为 [`__bases__`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#class.__bases__) 属性；而 *dict* 字典为包含类主体定义的命名空间并且会被复制到一个标准字典成为 [`__dict__`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#object.__dict__) 属性。

```
class A:
...     a = 1
>>> type(A)
<class 'type'>
>>> a = A()
>>> type(a)
<class '__main__.A'>

>>> X = type('X', (object,), dict(a=1))
>>> x = X()
>>> type(x)
<class '__main__.X'>
>>> type(X)
<class 'type'>
```

 会创建相同的 [`type`](https://docs.python.org/zh-cn/3.6/library/functions.html#type) 对象



## Unicode 编码解码

### chr()

用法：chr(i)

返回 Unicode 码位为整数 *i* 的字符的字符串格式。实参的合法范围是 0 到 1,114,111（16 进制表示是 0x10FFFF）。如 ` chr(25105)` 返回 '我' 。它是 ord() 的逆函数。

### ord()

用法：`ord`(*c*)

对表示单个 Unicode 字符的字符串，返回代表它 Unicode 码点的整数。如 `ord('我')` 返回 25105。

## 类操作

### property

用法：`property`(*fget=None*, *fset=None*, *fdel=None*, *doc=None*)

*fget* 是获取属性值的函数。 *fset* 是用于设置属性值的函数。 *fdel* 是用于删除属性值的函数。并且 *doc* 为属性对象创建文档字符串。

主要用于托管属性，使用装饰器来实现。

```
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

    @x.deleter
    def x(self):
        del self._x
```



### **classmethod**

用法：

```python
class C:
    @classmethod
    def f(cls, arg1, arg2, ...): ...
C.f()  # 调用合法
c = C()
c.f    # 也可以这样调用

```

把一个方法封装成类方法。一个类方法把类自己作为第一个实参，就像一个实例方法把实例自己作为第一个实参。可以直接使用类来调用方法，也可以在实例上进行。

###  staticmethod

将方法转换为静态方法。是一个函数装饰器。它不会接收隐式的第一个参数。

用法：

```
class C:
    @staticmethod
    def f(arg1, arg2, ...): ...
```

可以通过类调用 C.f() 也可以通过实例调用 C().f()

### super()

用法：`super`([*type*[, *object-or-type*]])

返回一个代理对象，它会将方法调用委托给 *type* 指定的父类或兄弟类。

*super* 有两个典型用例。

第一个是在具有单继承的类层级结构中，*super* 可用来引用父类而不必显式地指定它们的名称，从而令代码更易维护。

第二个用例是在动态执行环境中支持协作多重继承。

### isinstance()

用法：`isinstance`(*object*, *classinfo*)

返回 object 是否是 classinfo 的实例化对象。

### issubclass()

用法：`issubclass`(*class*, *classinfo*)

返回 class 是否是 classinfo 的子类。

```
>>> isinstance(1,int)
True
>>> isinstance('111',str)
True

>>> issubclass(bool,int)
True
```



## 编译,代码对象相关

### compile()

用法：`compile`(*source*, *filename*, *mode*, *flags=0*, *dont_inherit=False*, *optimize=-1*)

将 *source* 编译成代码或 AST 对象。代码对象可以被 [`exec()`](https://docs.python.org/zh-cn/3.6/library/functions.html#exec) 或 [`eval()`](https://docs.python.org/zh-cn/3.6/library/functions.html#eval) 执行。*source* 可以是常规的字符串、字节字符串，或者 AST 对象。*filename* 实参需要是代码读取的文件名；如果代码不需要从文件中读取，可以传入一些可辨识的值。

```python
>>> c = compile('print(123)','','eval')
>>> c
<code object <module> at 0x000002901E931420, file "", line 1>
>>> eval(c)
123
>>> exec(c)
123
```

### eval()

用法：`eval`(*expression*, *globals=None*, *locals=None*)

实参是一个字符串，以及可选的 globals 和 locals。*globals* 实参必须是一个字典。*locals* 可以是任何映射对象。可以用来执行任何代码对象（如 [`compile()`](https://docs.python.org/zh-cn/3.6/library/functions.html#compile) 创建的）。如果省略 *locals* 字典则其默认值为 *globals* 字典。 如果两个字典同时省略，表达式会在 [`eval()`](https://docs.python.org/zh-cn/3.6/library/functions.html#eval) 被调用的环境中执行。

```
>>> x = 10
>>> eval('10 * x +1')
101
```

### exec()

用法：`exec`(*object*[, *globals*[, *locals*]])

这个函数支持动态执行 Python 代码。*object* 必须是字符串或者代码对象。

### repr()

用法：`repr`(*object*)

返回包含一个对象的可打印表示形式的字符串，可以和eval函数来回转化。

```
>>> a = [1,2,3,4,5]
>>> repr(a)
'[1, 2, 3, 4, 5]'
>>> eval(repr(a))
[1, 2, 3, 4, 5]
```



## 对象属性操作

### dir()

用法：`dir`([*object*])

如果没有实参，则返回当前本地作用域中的名称列表。如果有实参，它会尝试返回该对象的有效属性列表。

```
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'c', 'pl']

>>> s = 'aaa'
>>> dir(s)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', ...]
```

### globals()

用法:`globals`()

返回表示当前全局符号表的字典。

### locals()

用法：`locals`()

更新并返回代表当前本地符号表的字典。

### vars()

用法：`vars`([*object*])

返回模块、类、实例或任何其它具有 [`__dict__`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#object.__dict__) 属性的对象的 [`__dict__`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#object.__dict__) 属性。

### delattr()

用法：`delattr`(*object*, *name*)

实参是一个对象和一个字符串。该字符串必须是对象的某个属性。

### getattr()

用法：`getattr`(*object*, *name*[, *default*])

返回对象命名属性的值。

### hasattr()

用法：`hasattr`(*object*, *name*)

该实参是一个对象和一个字符串。如果字符串是对象的属性之一的名称，则返回 `True`，否则返回 `False`。

### setattr()

用法：`setattr`(*object*, *name*, *value*)

与 [`getattr()`](https://docs.python.org/zh-cn/3.6/library/functions.html#getattr) 两相对应。 其参数为一个对象、一个字符串和一个任意值。 字符串指定一个现有属性或者新增属性。

## 格式操作

### format()

用法：`format`(*value*[, *format_spec*])

将 *value* 转换为 *format_spec* 控制的“格式化”表示。

```
>>> format(1.2223,'.2f')
'1.22
```

## 哈希值

### hash()

用法：`hash`(*object*)

返回该对象的哈希值（如果它有的话）。

```
>>> hash('aaa')
4467549855284690621
>>> hash([1,2,3])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
>>> hash((1,2,3))
2528502973977326415
```

## 帮助系统

### help()

用法：`help`([*object*])

启动内置的帮助系统（此函数主要在交互式中使用）。如果没有实参，解释器控制台里会启动交互式帮助系统。主要用于交互模式中。

## id 值

### id()

用法：`id`(*object*)

返回对象的“标识值”。该值是一个整数，在此对象的生命周期中保证是唯一且恒定的。两个生命期不重叠的对象可能具有相同的 [`id()`](https://docs.python.org/zh-cn/3.6/library/functions.html#id) 值。

## 输入输出

### input()

用法：`input`([*prompt*])

如果存在 *prompt* 实参，则将其写入标准输出，末尾不带换行符。接下来，该函数从输入中读取一行，将其转换为字符串（除了末尾的换行符）并返回。

### print()

用法：`print`(**objects*, *sep=' '*, *end='\n'*, *file=sys.stdout*, *flush=False*)

将 *objects* 打印到 *file* 指定的文本流，以 *sep* 分隔并在末尾加上 *end*。 *sep*, *end*, *file* 和 *flush* 如果存在，它们必须以关键字参数的形式给出。

```
>>> print(1,2,3,4,5,sep=':',end='nnn')
1:2:3:4:5nnn>>>
```



## 迭代器操作

### next()

用法：`next`(*iterator*[, *default*])

通过调用 *iterator* 的 [`__next__()`](https://docs.python.org/zh-cn/3.6/library/stdtypes.html#iterator.__next__) 方法获取下一个元素。如果迭代器耗尽，则返回给定的 *default*，如果没有默认值则触发 [`StopIteration`](https://docs.python.org/zh-cn/3.6/library/exceptions.html#StopIteration)。

```
>>> i = iter([1,2,3])
>>> next(i)
1
>>> next(i)
2
>>> next(i)
3
>>> next(i)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration

>>> next(i,0)
1            
>>> next(i,0)
2            
>>> next(i,0)
3            
>>> next(i,0)
0            
# 添加 default 之后，不会产生报错。
```

## 文件操作

### open()

用法：`open`(*file*, *mode='r'*, *buffering=-1*, *encoding=None*, *errors=None*, *newline=None*, *closefd=True*, *opener=None*)

*file* 是一个 [path-like object](https://docs.python.org/zh-cn/3.6/glossary.html#term-path-like-object)，表示将要打开的文件的路径（绝对路径或者当前工作目录的相对路径），也可以是要被封装的整数类型文件描述符。

*mode* 是一个可选字符串，用于指定打开文件的模式。

*buffering* 是一个可选的整数，用于设置缓冲策略。

*encoding* 是用于解码或编码文件的编码的名称。

*errors* 是一个可选的字符串参数，用于指定如何处理编码和解码错误 - 这不能在二进制模式下使用。

*newline* 控制 [universal newlines](https://docs.python.org/zh-cn/3.6/glossary.html#term-universal-newlines) 模式如何生效（它仅适用于文本模式）。它可以是 `None`，`''`，`'\n'`，`'\r'` 和 `'\r\n'`。控制换行符。