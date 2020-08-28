# python知识点

## 内置函数 64个

迭代器操作
>all(), any(), enumerate(), filter(), len(), map(), max(), min(), reversed(), sorted(), sum(), zip(), next(), tuple()

编码解码
>ascii(), chr(), ord()

数值操作
>abs(), divmod(), pow(), round()

编译执行
>compile(), exec(), eval()

格式控制
>format()

获取变量y

>globals(), locals(), vras()

获取帮助
>help()

进制转换
>bin(),oct(),hex()

输如输出
>input(), print()

内存视图()
>memoryview()

文件操作
>open()

range 函数
>range()

class
>bool(), bytearray(), bytes(), complex(), dict(), float(), frozenset(), int(), list(), object, set(), slice(), str(), type()

class 操作
>@classmethod, dir(), isinstance(), issubclass(), property(), staticmethod(), super()

object 操作
>callable(), delattr(), getattr(), hasattr(), hash(), id(), iter(), repr(), setattr()

导入模块
>\__import__()

## 内置常量

>False, True, None, NotImplemented(二进制特殊方法返回的值), Ellipsis(与...相同), __debug__(没有已 -0 选项启动即为 True)

site 模块添加的常量
>quit, exit, copyright, credits, license

## 内置类型

### 数字类型

int, float, complex,可以进行比较运算和数学运算，整数类型支持位运算(负整数不可)  
整数类型的附加方法有 bit_length(),to_bytes(),from_bytes()  
浮点类型的附加方法有 as_integer_ratio()，is_integer()，hex(),fromhex(s)

### 序列类型

list, tuple, range,分为可变序列和不可变序列,序列的通用操作为 in,+,*,切片,len,max,min,index,count。  
讨论可变不可变，仅指其直接包含的对象的编号，而谈论一个容器的值时，我们是指所包含对象的值而不是其编号，因此，如果一个不可变容器 (例如元组) 包含对一个可变对象的引用，则当该可变对象被改变时容器的值也会改变。  
不可变序列类型普遍实现而可变序列类型未实现的唯一操作就是对 hash() 内置函数的支持。  
可变序列类型支持的操作有修改元素值(使用 index 和切片都可以)、删除元素值、clear,copy,insert,pop,remove,reverse。代表为列表。  
不可变序列类型支持序列的一般操作，支持序列的一般操作。代表为元祖和 range(不支持拼接和重复)。  

### str 类型

#### 字符串常量

ascii_letters，ascii_lowercase，ascii_uppercase，digits，hexdigits，octdigits，punctuation，printable，whitespace  
字符串辅助函数 `string.capwords(s, sep=None)`  

#### 正则匹配

> . ^ $ * + ? {m} {m,n} {m,n} ? \ [] | (...) (?…)扩展标记法'?' 后面的第一个字符决定了这个构建采用什么样的语法

re 模块函数
compile(),search(),match(),fullmatch(),split(),findall()...  
编译后的正则表达式对象  正则对象 方法：search(),match(),fullmatch(),split(),findall()...  
匹配后的对象为匹配对象 方法：expand(),group(),groups(),groupdict(),start(),end()

可以匹配字符串和 bytes 对象
字符串也为不可变序列。字符串实现了很多方法。  

#### 字符串的格式化

% 取模运算符  
format()方法 可以按照位置参数、名称参素访问，也可以访问参数的项和属性，还可指定格式化。  
格式 `"{" [field_name] ["!" conversion] [":" format_spec] "}"`  
format_soec `[[fill]align][sign][#][0][width][grouping_option][.precision][type]`  
模板字符串  主要用例是文本国际化 (i18n)，使用 string.Template 类

二进制序列类型  bytes, bytearray, memoryview  
bytes 对象是单个字节构成的不可变序列，使用形式 `b''`，字面值只允许 ASCLL 字符,字面值的语法与字符串字面值的大致相同，只是添加了一个 b 前缀。是由整数构成的序列。

bytearray 是可变的 bytes。支持可变序列操作。

bytes 和 hex 之间的互相转换使用 bytes.fromhex() 和 .hex() 来完成。他的方法与字符串大致相同。

字符串和 bytes 的strip(chars),会移除参数的所有组合

memoryview 允许 Python 代码访问一个对象的内部数据，只要该对象支持 缓冲区协议 (包括bytes 和 bytearray)而无需进行拷贝。

### 集合类型 set, frozenset

集合的元素必须是 hashable

set 对象是由具有唯一性的 hashable 对象所组成的无序多项集。 常见的用途包括成员检测、从序列中去除重复项以及数学中的集合类计算,支持 in、len 和 for遍历。

set 类型是可变的，可以使用 add(), remove()。frozenset 类型不可变，可用作字典的键或其他集合的元素。  
两者都可用集合运算 |,<,>,union,&,-,^  
set 可用的集合操作 |=,&=,-=,^=,add,remove,discard,pop,clear

#### hashable

hashable：一个对象的哈希值如果在其生命周期内绝不改变，就被称为 可哈希 （它需要具有 __hash__() 方法），并可以同其他对象进行比较（它需要具有 __eq__() 方法）。可哈希对象必须具有相同的哈希值比较结果才会相同。如果一个元素 hashable，它可以作为字典键或者集合成员使用，这些数据结构在内部使用哈细致。  
所有内建不可变对象都是 hashable，可变的容器不是。  
用户自定义的实例默认为 hashable

### 映射类型 dict

dict 键需要 hashable  
dict 操作 len,d\[key],in,iter(d),clear,copy(浅拷贝),get,items,keys,pop,popitem,setdefault,update,values  
keys,values,items 返回值时字典视图对象。

### 上下文管理器

with 语句提供支持，由两个方法实现  
>contextmanager.\__enter__() #进入运行状态的上下文管理器  
>contextmanager.\__exit__()  #退出运行状态的上下文

### 其他的内置类型

包括 模块、类与类实例、函数、方法、代码对象、类型对象、空对象、省略符对象、未实现对象、布尔值、内部对象

## 拷贝问题

Python 中赋值语句不复制对象，而是在目标和对象之间创建绑定 (bindings) 关系

```
li1 = [1,2,3]
li = li1
```
此时 li 和 li1 的 id 和 值都相同。li is li1 为 True

```
li1 = [[1,2,3],4]
li = li1.copy()  
```

浅拷贝：重新分配一块内存，创建一个新的对象，但里面的元素是原对象中各个子对象的引用。li is li1 为 False，此时如果 li1[0] 发生了变化，li 也会变化。

```
import copy
li1 = [[1,2,3],4]
li = copy.deepcopy(li1)
```

深拷贝，li 和 li1 完全独立。没有任何联系。  
可能会导致两个问题：

- 递归对象可能会导致递归循环  
- 由于深层复制会复制所有内容，因此可能会过多复制（例如本应该在副本之间共享的数据）

## 数据类型

### datetime

有两种日期和时间的对象：“简单型“和”感知型“  
简单型对象所代表的是世界标准时间(UTC)、当地时间或者是其它时区的时间完全取决于程序.  
感知型带有其他的时间信息，例如时区和夏令时，tzinfo 是一个感知型抽象基类， timezone 实现了 tzinfo 抽象基类的子类，用于表示相对于 世界标准时间（UTC）的偏移量。  
timedelta 对象表示两个 date 或者 time 的时间间隔。日期的运算结果为 timedelta 类  
建立附带 tzinfo 的日期对象，创建时指定 tzinfo =  tzinfo子类实例  

### calendar

模块中有 Calendar(生成日历对象) TextCalendar(生成纯文本日历对象) HTMLCalendar(生成HTML日历) LocaleTextCalendar LocaleHTMLCalendar(返回本地语言环境的对象),calendar 模块中也有一些函数可以使用判断日期信息。

### collections 模块

实现了一些特定目标的容器。

#### ChainMap 对象

将多个字典组合在一起成立一个新对象，可更新， ChainMap 通过引用合并底层映射，所以字典成员和组合对象的更新会彼此影响。  
对组合对象操作时，搜索查询底层映射，直到一个键被找到。不同的是，写，更新和删除只操作第一个映射。  


#### Counter 对象

是 dict 的子类,键由值来排序，值为整数，统计键出现的次数。适用于重复数据的统计。  
形式为 `Counter({'a': 3, 'b': 0, 'c': -3, 'd': -6})`  
counter 支持 +-&| 操作，输出会忽略掉结果为0或小于0的键  
支持一元操作符 +c 查看 c 中value为整数， -c 查看 c 中value 为负数  

#### deque 对象

双向队列，从两端弹出和添加元素，复杂度都为 O(1)

#### defaultdict 对象

是 dict 的子类 建立时指定字典 value 的类型  
如 d = defaultdict(list)，d 的 value 默认是 []，对 value 的操作就简单许多，不需要将它初始化为 []

#### namedtuple() 对象

命名元祖，赋予每个元素一个名字，他的作用是返回一个元祖子类，用于创建元祖对象。类名由 typename 指定，field_name 指定每个元素的名称，可以使列表或用空格或逗号分开的字符串。  
命名元祖的操作方法，前面带_ 符号 通过 _fields 可以查看域名和添加域名。

#### OrderedDict 对象

有序字典可以记录元素插入的顺序，返回时按照顺序。  
元素修改时，位置不变，删除后重新添加会使元素位置到末尾。  
比较两个有序字典是否相等，实现是 `list(od1.items())==list(od2.items())`

#### UserDict 对象

模拟一个字典类。这个实例的内容保存为一个正常字典， 可以通过 UserDict 实例的 data 属性存取。  
data 属性用于保存 UserDict 类的内容。

#### UserList 对象

与 UserDict 对象类似，是 list 的封装， data 属性为 list 对象。

#### UserString 对象

类似，是 字符串或Unicode字符串对象的封装， data 属性访问。

### bisect 数组二分查找算法

操作列表的前提是特本来就是有序列表。使用二分查找算法来实现。  
这个模块提供有序列表支持，在插入元素时仍然保持有序。  
按照插入元素和已存在元素相等时的处理方式可以分为 left 和 right 两种解决思路。bisect 找到插入位置，insort 找到位置插入。  

### array 数值数组

数组是序列类型，与列表相似，存储类型受限制，在创建时由类型码来指定。数据类型和 c 语言中类似。  
u 表示 Unicode 字符，f,d 表示浮点数，其他对应 int ，字节不同。
array 对象支持索引、切片、拼接，但是在操作时需要注意数据类型需要和创建时相同。  

### pprint 数据美化输出

使用 pprint 函数，指定 indent(每个层级的缩进量) width(希望的输出宽度,对象不可拆分时会全部显示) compact(bool 值，为假时将长序列的每一项格式化为单独的行，为真时在width可容纳的情况下将尽可能多的项放入输出行) depth(可打印的层级数量)。  
pprint 函数是通过实例化 PrettyPrinter 来实现的。
模块中的  PrettyPrinter 是一个类，可以通过实例化此类来打印。实例化后的类使用类方法格式化数据。  

### enum 枚举类型

枚举是一组符号名称（枚举成员）的集合，枚举成员应该是唯一的、不可变的。  
枚举使用 class 语法创建
```
@enum.unique 不允许有多个名称作为相同值的别名
class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3
```
成员值可以为任意类型: int, str 等等。  
枚举成员的特性

- 使用 repr 函数可访问  
- 枚举成员的 type 就是它所从属的枚举  
- 枚举成员有一个 name 属性
- 枚举可以按照定义顺序进行迭代
- 枚举成员是可哈希的，可以用在集合和字典键中
- 枚举成员不可以重名，值可以相同(允许两个枚举成员有相同的值。 假定两个成员 A 和 B 有相同的值（且 A 先被定义），则 B 就是 A 的一个别名。 按值查找 A 和 B 的值将返回 A。 按名称查找 B 也将返回 A)
- 对枚举成员的迭代不会迭代别名
- 枚举成员之间不可以排序比较(<,>),支持 is == 比较
- 枚举没有定义任何成员时才能派生子类，但是支持在只定义类方法时派生子类

对枚举成员的访问

- Color(1) 按值进行访问
- 通过 name 属性访问 Color['RED']
- 访问值 Color.RED.value

可以直接调用 Enum 类生成枚举
>Animal = Enum('Animal', 'ANT BEE CAT DOG') #第一个参数是枚举名称，第二个是枚举值，取值可以是用空格分隔的名称字符串、名称序列、键/值对 2 元组的序列，或者名称到值的映射（例如字典）,， module 关键字参数显示的指定模块名称， qualname 指定Enum 类在模块中的具体位置，type 指定加入新 Enum 类的类型。start 指定只传入参数时使用的起始数值。  

#### 枚举的派生类型

IntEnum：也是 int 的一个子类。成员支持与整数比较，不同类型的整数成员也可以比较，不可与标准 Enum 比较。  
IntFlag：基于 int 。成员可以使用按位运算符，并且能在任何使用 int 的场合被使用，并合并结果仍为 IntFlag 成员，其他的运算会使它失去成员资格。  
Flag：成员可以使用按位运算符，但是不可与任何其它 Flag 枚举或 int 进行组合或比较，虽然可以直接指定值，但推荐使用 auto 作为值以便让 Flag 选择适当的值。

### 二进制数据

#### struct

大端模式：数据的高字节保存在内存的低地址中，而数据的低字节保存在内存的高地址中  
小端模式：数据的高字节保存在内存的高地址中，而数据的低字节保存在内存的低地址中  

struct 库执行 Python 值和以 Python bytes 对象表示的 C 结构之间的转换，选择此种行为的目的是使得被打包结构的字节能与相应 C 结构在内存中的布局完全一致。  

format 格式字符串

- 第一个字符可用于指示打包数据的字节顺序，大小和对齐方式(@,=,<,>,!)
- 后接格式字符(x,c,b,B,?,h,H,i,I,l,L,q,Q,n,N,e,f,d,s,p,P)
- 格式字符之前可以带整数代表重复，例如'4h' 的含义与 'hhhh' 完全相同  

pack 将传入的值压缩返回一个 bytes 对象  
pack_into 传入的值打包后的字节串写入缓冲区 buffer 从 offset 开始的位置，offset 是必须的  
unpack 按照格式字符串解包 buffer 返回一个元祖，buffer 的字节大小需要符合格式匹配的大小  
unpack_from  从位置 offset 开始根据格式字符串 format 进行解包  
iter_unpack  根据格式字符串 format 以迭代方式从缓冲区 buffer 解包,返回一个迭代器。  
calcsize  返回与格式字符串 format 相对应的结构的大小

#### codecs 编解码器注册和相关基类

定义了标准 Python 编解码器（编码器和解码器）的基类，并提供接口用来访问内部的 Python 编解码器注册表，该注册表负责管理编解码器和错误处理的查找过程。  
CodeInfo 类 查找编解码器注册表所得到的编解码器细节信息。  
lookup 在 Python 编解码器注册表中查找编解码器信息，并返回一个 CodecInfo 对象。  
encode，decode 通过指定的 encoding 进行编码解码  
getencoder，getdecoder  查找给定编码的编解码器并返回其编/解码器函数  
getincrementalencoder，getincrementaldecoder 查找给定编码的编解码器并返回其增量式编/解码器类或工厂函数  
getreader，getwriter 查找给定编码的编解码器并返回其 StreamReader 类或工厂函数  
register 注册一个编解码器搜索函数，并返回一个 CodecInfo 对象  
open 操作二进制文件时使用更多各类的编解码器，使用给定的 mode 打开已编码的文件并返回一个 StreamReaderWriter 的实例  

编解码器的基类  每种编解码器必须定义四个接口以便用作 Python 中的编解码器：无状态编码器、无状态解码器、流读取器和流写入器。  
无状态的编码和解码 Codec 类  
增量式的编码和解码 IncrementalEncoder 和 IncrementalDecoder 类，通过对增量式编码器/解码器的 encode()/decode() 方法的多次调用，在方法调用期间跟踪编码/解码过程。实现的方法有 encode/decode，reset，getstate，setstate  
流式的编码和解码  StreamWriter 和 StreamReader 和 StreamReaderWriter类，是 Codec 的子类。

##### 编码格式和 Unicode

字符串的存储范围为 0x0–0x10FFFF(0-1114111)  
存储每个 Unicode 码位的简单而直接的办法就是将每个码位存储为四个连续的字节。会因为不同机器上的大小端模式存在存储差别，而UTF-32 避免了这个问题：字节的排列将总是使用自然顺序，在具有不同字节顺序的 CPU 读取时，则必须进行字节翻转。检测是否需要翻转使用 BOM (“字节顺序标记”)判断。  
另一种为 UTF-8 ,这是一种变长(1-6 个字节)的编码表达方式  

- 对于单字节的 ascii 占一个字节，第一位为 0
- 对于n字节的符号（n > 1），第一个字节的前n位都设为1，第n + 1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的码点。

## 函数式编程

### itertools 创建迭代器

itertools 模块中的函数均创建并返回迭代器  
无穷迭代器 count cycle repeat  
根据输入序列长度停止的迭代器 accumulate chain  chain.from_iterable compress dropwhile filterfalse islice starmap groupby takewhile tee zip_longest  
排列组合迭代器 product permutations combinations combinations_with_replacement  
这些定义的迭代器性能和内存利用率很高，在创建函数时可以利用这些函数。  

### functools 函数工具

cmp_to_key  将 Python 2 程序转换至新版的转换工具，以保持对比较函数的兼容  
@functools.lru_cache 缓存装饰器，缓存 maxsize 组传入参数，在下次以相同参数调用时直接返回上一次的结果。当调用 cache_info() 函数时，返回一个具名元组，包含命中次数 hits，未命中次数 misses ，最大缓存数量 maxsize 和 当前缓存大小 currsize。  
@functools.total_ordering  给定一个声明一个或多个全比较排序方法的类，这个类装饰器实现剩余的方法  
functools.partial 返回一个新的 部分对象，当被调用时其行为类似于 func 附带位置参数 args 和关键字参数 keywords 被调用。  
partialmethod 返回一个新的 partialmethod 描述器，其行为类似 partial 但它被设计用作方法定义而非直接用作可调用对象。  
reduce

### operator 标准运算符替代函数

operator 模块提供了一套与Python的内置运算符对应的高效率函数。  
比较运算： lt, le, eq, ne, ge, gt  
逻辑运算： not, truth, is_, is_not  
数学运算和按位运算： abs, add, and, floordiv, index, inv, lshift, mod, mul, matmul, neg, or_, pos, pow, rshift, sub,truediv, xor  
序列操作： concat, contains, countof, delitem, getitem, indexof, setitem, length_hint  
赋值运算： iadd(iadd(a,b)等价与 a += b), iand, iconcat...

## python 缓存机制

python 中，整数，bool，浮点数，字符串会被缓存。

整数范围在 [-5,256]中是小整数，会被解释器缓存，在整个生命周期内，需要用到时，就去获取，会被重复引用。 bool 类型也是这样。  
大整数也有缓存，在同一代码块中，会缓存用到的大整数，在同一代码快中会被重复引用。在交互式模式和文件模式中有差异，因为交互模式中每一行被认为是一个代码块。大于 0 的浮点数也是这样。  
字符串类型，会在被创建之后缓存，再使用时不需要再创建(全局缓存)。字符串缓存需要满足条件：

- 字符串的长度>1,且只含有大小写字母，数字，下划线时，才会默认驻留。
- 字符串的长度为0或者1，默认都采用了驻留机制（小数据池）。
- 使用乘法得到字符串分为两种情况：
  - 乘数为1时，默认驻留;
  - 乘数>=2时：仅含大小写字母，数字，下划线，总长度<=20,默认驻留  
- 小于 0 的浮点数和小于 -5 的整数不会被缓存。

## 数学模块

### math 模块

数值运算  
ceil(x) 返回大于或者等于 x 的最小整数。  
copysign(x,y) 返回一个基于 x 的绝对值和 y 的符号的浮点数。  
fabs(x) 返回 x 的绝对值。  
factorial(x) 返回正整数 x 的阶乘。  
floor(x) 返回小于或等于 x 的最大整数。  
fmod(x,y)  浮点数取余，y 对于某个整数 n ，使得结果具有 与 x 相同的符号和小于 abs(y) 的幅度  
frexp(x) 返回 x 的尾数和指数作为对``(m, e)``,使 x == m *2 \*\*e  
fsum(iterable) 返回迭代中的精确浮点值。通过跟踪多个中间部分和来避免精度损失  
gcd(a,b) 返回a,b的最大公约数  
isclose(a, b, \*, rel_tol=1e-09, abs_tol=0.0) 若 a 和 b 的值比较接近则返回 True，否则返回 False  
isfinite(x) 如果 x 既不是无穷大也不是NaN，则返回 True ，否则返回 False  
ifinf(x) 如果 x 是正或负无穷大，则返回 True ，否则返回 False  
isnan(x) 如果 x 是 NaN（不是数字），则返回 True ，否则返回 False  
ldexp(x, i) 返回 x \* (2**i) 。 这基本上是函数 frexp() 的反函数。  
modf(x) 返回 x 的小数和整数部分。两个结果都带有 x 的符号并且是浮点数  
trunc(x) 返回 Real 值 x 截断为 Integral （通常是整数）

幂函数和对数函数  
exp(x) 返回 e 的 x 次方  
expm1(x) 返回 exp(x) -1  
log(x[, base]) 返回 x 的自然对数,或者 给定的 base 的对数 x ，计算为 log(x)/log(base)  
log1p(x) 返回 1+x (base e) 的自然对数。以对于接近零的 x 精确的方式计算结果。  
log2(x) 返回 x 以2为底的对数。这通常比 log(x, 2) 更准确。  
log10(x) 返回 x 以10为底的对数。这通常比 log(x, 10) 更准确。  
pow(x, y) 将返回 x 的 y 次幂。  
sqrt(x) 返回 x 的平方根  

三角函数  
acos, asin, atan, stan2, cos, hypot, sin, tan

角度转换  
degrees, radians  

双曲函数  
erf, erfc gamma, lgamma  

常量  
pi(π), e, tau(τ), inf(浮点正无穷大，负无穷大，使用 -math.inf), nan(浮点“非数字”（NaN）值)

### numbers 模块。数字的抽象基类

Number 数字的层次结构的基础  
Complex  描述了复数和它的运算操作。
Real 相对于 Complex，Real 加入了只有实数才能进行的操作。  
Rational 子类型 Real 并加入 numerator 和 denominator 两种属性。  
Integral 子类型 Rational 加上转化至 int。

### cmath 模块，关于复数的数学函数

### decimal 模块 十进制定点和浮点运算

decimal 模块为快速正确舍入的十进制浮点运算提供支持。特点为

- 基于考虑人类习惯的浮点数模型，计算机必须提供与人们在学校所学习的算术相一致的算术。  
- 他表示的数字是精确的  
- 算数类操作也是精确的，如 0.1 + 0.1 + 0.1 - 0.3 会精确地等于零。  
- 包含了有效位的概念，使得 1.30 + 1.20 是 2.50  
- 与基于硬件的二进制浮点数不同，decimal 模块具有用户可更改的精度（默认为28位),getcontext().prec = 6 可以修改为 6 位
- decimal 模块公开了标准的所有必需部分。  
- decimal 模块旨在支持“无偏差，精确无舍入的十进制算术（有时称为定点数算术）和有舍入的浮点数算术”。 —— 摘自 decimal 算术规范说明。  

getcontext() 查看当前信息，通过它也可以设置参数  

Decimal 数字可以与其他类型的数字转换，利用内置函数也可以操作 Decimal 数字。  
BasicContext 和 ExtendedContext 是模块中自带的管理器,自己创建使用 setcontext() 函数。  
decimal 中有常量,可以用来设置 Content 中的 rounding traps 参数。  

### fractions 分数

支持分数运算。可以由一对整数，一个分数，或者一个字符串构建而成。  
字符串的构成为 [sign] numerator ['/' denominator]， sign 可以为 ‘+’ 或 ‘-‘ 并且 numerator 和 denominator (如果存在) 是十进制数码的字符串。  

### random 伪随机数生成

核心生成器是 Mersenne Twister，是完全确定性的，不适用于所有目的，并且完全不适合s加密目的。  
seed(a=None, version=2) 初始化随机数生成器。a 相同时，生成的随机数相同。  
getstate() 返回捕获生成器当前内部状态的对象。  
setstate(state) state 应该是从之前调用 getstate() 获得的，并且 setstate() 将生成器的内部状态恢复到 getstate() 被调用时的状态。  
getrandbits(k) 返回带有 k 位随机的Python整数。  

整数使用的函数  
randrange(stop) 或 randrange(start, stop[, step]) 从 range(start, stop, step) 返回一个随机选择的元素。  
randint(a, b) 返回随机整数 N 满足 a <= N <= b  

序列使用函数

choice(seq) 从非空序列 seq 返回一个随机元素  
choices(population, weights=None, *, cum_weights=None, k=1) 从*population*中选择替换，返回大小为 k 的元素列表。  
shuffle(x[, random]) 将序列 x 随机打乱  
sample(population, k) 返回从总体序列或集合中返回 k 长度的列表。 用于无重复的随机抽样。  

实值分布

random() 返回 [0.0, 1.0) 范围内的一个随机浮点数。  
uniform(a, b) 返回一个随机浮点数 N ，当 a <= b 时 a <= N <= b ，当 b < a 时 b <= N <= a 。  
triangular(low, high, mode) 返回一个[a,b]之间随机浮点数 N ，mode 参数默认为边界之间的中点，给出对称分布。  
normalvariate(mu, sigma) 正态分布。 mu 是平均值，sigma 是标准差。  

### statistics 数学统计函数

此模块提供了一些数值类型统计的相关函数。  

所有的 data 要求不能为空，否则会引起 StatisticsError  
mean(data) 返回 data 的样本算术平均数，数据可是是一个序列或迭代器。  
harmonic_mean(data) 返回 data 的调和均值，调和均值,也叫次相反均值，比如说，数据 a ， b ， c 的调和均值等于 3/(1/a + 1/b + 1/c) 。  
median(data) 取中间两数平均值  
median_low(data) 取中间两数平均值，当其为偶数时，将返回两个中间值中较小的那个。  
median_high(data) 取中间两数平均值，当其为偶数时，将返回两个中间值中较小的那个。  
median_grouped(data, interval=1) 使用插值返回分组的连续数据的中位数  
mode(data) 从离散或标称数据返回最常见的数据点。返回值是唯一的，并且可以应用于其他数据类型。  
pstdev(data, mu=None) 返回总体标准差（总体方差的平方根）。  
pvariance(data, mu=None) 返回数据的总体方差, mu 参数如果给出，是数据的平均值。  
stdev(data, xbar=None) 返回样本标准差（样本方差的平方根）。  
variance(data, xbar=None) 返回包含至少两个实数值的可迭代对象 data 的样本方差。  

## 文件和目录访问

### pathlib

提供表示文件系统路径的类，适用于不同的操作系统。  

PurePath(*pathsegments) 通用的类，代表当前系统的路径风格（实例化为 PurePosixPath 或者 PureWindowsPath）,pathsegments 的元素可能是一个代表路径片段的字符串，一个返回字符串的实现了 os.PathLike 接口的对象，或者另一个路径对象，这三个对象提供的操作不做系统调用  
路径是不可变并可哈希的。相同风格的路径可以排序与比较。  
支持运算符操作，/ 操作创建子路经，`p / 'bin'` 可进入 p 下的 bin 目录  
属性

- parts 返回元祖，将路径拆分多个部分  
- drive 表示驱动器盘符或命名的字符串，如果存在  
- root 表示（本地或全局）根的字符串  
- anchor 驱动器和根的联合
- parents 返回此路径的多个父层路径  
- parent 此路径的逻辑父路径
- name 表示最后路径组件的字符串
- suffix 最后一个组件的文件扩展名  
- suffixes 路径的文件扩展名列表  
- stem 最后一个路径组件，除去后缀
- as_posix() 返回使用正斜杠（/）的路径字符串
- is_absolute() 返回此路径是否为绝对路径
- joinpath(*other) 调用此方法等同于将每个 other 参数中的项目连接在一起
- match(pattern) 将此路径与提供的通配符风格的模式匹配。如果匹配成功则返回 True，否则返回 False
- relative_to(*other) 计算此路径相对 other 表示路径的版本。
- with_name(name) 返回一个新的路径并修改 name
- with_suffix(suffix) 返回一个新的路径并修改 suffix

Path() 实例化一个具体路径类,PurePath 的子类，此类以当前系统的路径风格表示路径（实例化为 PosixPath 或 WindowsPath  
方法

- cwd() 返回一个新的表示当前目录的路径对象,对应 os.getcwd()
- home() 返回一个表示当前用户家目录的新路径对象 对应  os.path.expanduser()
- stat() 返回有关此路径的信息,对应os.stat（）
- chmod(mode) 改变文件的模式和权限，对应 os.chmod()
- exists() 此路径是否指向一个已存在的文件或目录
- glob(pattern) 在此路径表示的目录中返回匹配到的文件，“**” 模式表示 “此目录以及所有子目录，递归”，例如 `Path('.').glob('**/*.py')`匹配目录下所有的 py 文件
- expanduser() 返回展开了包含 ~ 和 ~user 的字符串，对应 os.path.expanduser()，例如将 '~/films/Monty Python' 展开为 '/home/eric/films/Monty Python'
- group() 返回拥有此文件的用户组
- is_dir() 如果路径指向一个目录（或者一个指向目录的符号链接）则返回 True，如果指向其他类型的文件则返回 False。
- is_file() 如果路径指向一个正常的文件（或者一个指向正常文件的符号链接）则返回 True，如果指向其他类型的文件则返回 False。
- is_symlink() 如果路径指向符号链接则返回 True， 否则 False
- is_socket() Unix socket 文件
- is_fifo() 进先出存储（或者指向先进先出存储的符号链接)
- is_block_device() 指向一个块设备
- is_char_device() 指向一个字符设备
- iterdir() 当路径指向一个目录时，产生该路径下的对象的路径
- lchmod(mode) 就像 Path.chmod() 但是如果路径指向符号链接则是修改符号链接的模式，而不是修改符号链接的目标。
- lstat() 就和 Path.stat() 一样，但是如果路径指向符号链接，则是返回符号链接而不是目标的信息。
- mkdir(mode=0o777, parents=False, exist_ok=False) 新建给定路径的目录。
- open(mode='r', buffering=-1, encoding=None, errors=None, newline=None) 打开路径指定的文件，对应内置函数 open()
- owner() 返回拥有此文件的用户名
- read_bytes() 以字节对象的形式返回路径指向的文件的二进制内容
- read_text(encoding=None, errors=None) 以字符串形式返回路径指向的文件的解码后文本内容。
- rename(target) 使用给定的 target 将文件重命名,target 可以是一个字符串或者另一个路径对象
- replace(target) 使用给定的 target 重命名文件或目录。
- resolve(strict=False) 将路径绝对化，解析任何符号链接。返回新的路径对象
- rglob(pattern) 就像调用Path.glob（）并在给定模式前面添加“ **”
- rmdir() 移除此目录。此目录必须为空的。
- samefile(other_path) 返回此目录是否指向与可能是字符串或者另一个路径对象的 other_path 相同的文件。
- symlink_to(target, target_is_directory=False) 将此路径创建为指向 target 的符号链接。
- touch(mode=0o666, exist_ok=True) 将给定的路径创建为文件
- unlink() 移除此文件或符号链接。如果路径指向目录，则用 Path.rmdir() 代替。
- write_bytes(data) 将文件以二进制模式打开，写入 data 并关闭
- write_text(data, encoding=None, errors=None) 将文件以文本模式打开，写入 data 并关闭


## 数据库编程

### 为什么需要数据库编程

数据持久化存储

### DB-API

DB-API为不同的数据库提供了一致的接口，定义了必须的对象和数据库存储方式，python各个数据库模块需要兼容DB-API，包含几个对象：

1. 数据库连接对象：connection

2. 数据库交互对象：cursor（游标对象）

3. 数据库异常类：exceptions

数据库对象与数据库通信，有close、commit、rollback、cursor等方法。

游标对象可以执行数据库命令和得到查询结果，execute(sql)、fetchone()得到结果集的下一行、fetchmany(size)、fetchall(),在sql语句中，要确认一下数据类型，方式sql语句执行时类型不匹配

使用数据库的过程包括

1. 创建connection对象，获取cursor

2. 使用cursor执行SQL

3. 使用cursor获取数据、判断执行状态

4. 提交事务 或者 回滚事务

### 对象-关系映射（ORM）

考虑对象，不是sql。避免编写sql语句，将数据表转换为python类，它封装了数据库操作，只需面向对象。python中的ORM模块有很多种，django框架用的是自己的组件，不同模块之间的模型操作方法不同。

映射关系：

- 数据库的表（table） --> 类（class）
- 记录（record，行数据）--> 对象（object）
- 字段（field）--> 对象的属性（attribute）

ORM将数据库的增删改查封装成方法，表之间的关系设置为字段属性。

## 多线程编程

需要多线程完成的任务：异步的，各个事物的运行顺序不重要，可以是不确定的。

### 线程与进程

进程是内存中的程序，有属于自己的地址、内存、数据栈和其他信息，由操作系统来管理进程，但是进程之间共享信息需要通过辅助工具，不能直接共享。

线程是进程的分支，同一进程中的线程运行在同一环境中，共享数据空间。它有开始，顺序执行和结束三部分，在运行时可能被中断或者挂起，让其他线程运行，这叫做让步。

### 全局解释器锁GIL

python代码执行由虚拟机来控制，同一时刻只能由一个线程在执行，线程间的调度由GIL控制，线程在等待I/O时或运行15ms（py3，py2是执行1000字节码）后释放GIL。

### 多线程模块 threading

其中Thread类创建线程,创建进程对象后，使用start方法开始进程，join方法用于主线程等待线程完成(如果给了timeout参数会等到超时)。