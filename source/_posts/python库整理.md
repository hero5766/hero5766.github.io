title: python库整理
author: hero576
tags:
  - python
categories:
  - programme
date: 2020-07-15 11:27:00
---
> 整理一些python遇到的库
<!--more-->
# 系统库
## os
- 引入：`import os`
- 函数

|函数|说明|举例|
|--|--|--|
|os.access(path, mode)|检验权限模式||
|os.chdir(path)|改变当前工作目录||
|os.chflags(path, flags)|设置路径的标记为数字标记。||
|os.chmod(path, mode)|更改权限||
|os.chown(path, uid, gid)|更改文件所有者||
|os.chroot(path)|改变当前进程的根目录||
|os.close(fd)|关闭文件描述符 fd||
|os.closerange(fd_low, fd_high)|关闭所有文件描述符，从 fd_low (包含) 到 fd_high (不包含), 错误会忽略||
|os.dup(fd)|复制文件描述符 fd||
|os.dup2(fd, fd2)|将一个文件描述符 fd 复制到另一个 fd2||
|os.fchdir(fd)|通过文件描述符改变当前工作目录||
|os.fchmod(fd, mode)|改变一个文件的访问权限，该文件由参数fd指定，参数mode是Unix下的文件访问权限。||
|os.fchown(fd, uid, gid)|修改一个文件的所有权，这个函数修改一个文件的用户ID和用户组ID，该文件由文件描述符fd指定。||
|os.fdatasync(fd)|强制将文件写入磁盘，该文件由文件描述符fd指定，但是不强制更新文件的状态信息。||
|os.fdopen(fd[, mode[, bufsize]])|通过文件描述符 fd 创建一个文件对象，并返回这个文件对象||
|os.fpathconf(fd, name)|返回一个打开的文件的系统配置信息。name为检索的系统配置的值，它也许是一个定义系统值的字符串，这些名字在很多标准中指定（POSIX.1, Unix 95, Unix 98, 和其它）。||
|os.fstat(fd)|返回文件描述符fd的状态，像stat()。||
|os.fstatvfs(fd)|返回包含文件描述符fd的文件的文件系统的信息，Python 3.3 相等于 statvfs()。||
|os.fsync(fd)|强制将文件描述符为fd的文件写入硬盘。||
|os.ftruncate(fd, length)|裁剪文件描述符fd对应的文件, 所以它最大不能超过文件大小。||
|os.getcwd()|返回当前工作目录||
|os.getcwdu()|返回一个当前工作目录的Unicode对象||
|os.isatty(fd)|如果文件描述符fd是打开的，同时与tty(-like)设备相连，则返回true, 否则False。||
|os.lchflags(path, flags)|设置路径的标记为数字标记，类似 chflags()，但是没有软链接||
|os.lchmod(path, mode)|修改连接文件权限||
|os.lchown(path, uid, gid)|更改文件所有者，类似 chown，但是不追踪链接。||
|os.link(src, dst)|创建硬链接，名为参数 dst，指向参数 src||
|os.listdir(path)|返回path指定的文件夹包含的文件或文件夹的名字的列表。||
|os.lseek(fd, pos, how)|设置文件描述符 fd当前位置为pos, how方式修改: SEEK_SET 或者 0 设置从文件开始的计算的pos; SEEK_CUR或者 1 则从当前位置计算; os.SEEK_END或者2则从文件尾部开始. 在unix，Windows中有效||
|os.lstat(path)|像stat(),但是没有软链接||
|os.major(device)|从原始的设备号中提取设备major号码 (使用stat中的st_dev或者st_rdev field)。||
|os.makedev(major, minor)|以major和minor设备号组成一个原始设备号||
|os.makedirs(path[, mode])|递归文件夹创建函数。像mkdir(), 但创建的所有intermediate-level文件夹需要包含子文件夹。||
|os.minor(device)|从原始的设备号中提取设备minor号码 (使用stat中的st_dev或者st_rdev field )。||
|os.mkdir(path[, mode])|以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制)。||
|os.mkfifo(path[, mode])|创建命名管道，mode 为数字，默认为 0666 (八进制)||
|os.mknod(filename[, mode=0600, device])|创建一个名为filename文件系统节点（文件，设备特别文件或者命名pipe）。||
|os.name|判断现在正在实用的平台，Windows 返回 ‘nt‘; Linux 返回posix||
|os.open(file, flags[, mode])|打开一个文件，并且设置需要的打开选项，mode参数是可选的||
|os.openpty()|打开一个新的伪终端对。返回 pty 和 tty的文件描述符。||
|os.pathconf(path, name)|返回相关文件的系统配置信息。||
|os.pipe()|创建一个管道. 返回一对文件描述符(r, w) 分别为读和写||
|os.popen(command[, mode[, bufsize]])|从一个 command 打开一个管道，从 version 2.6起: 所有的popen*()函数已抛弃. 使用subprocess模块||
|os.read(fd, n)|从文件描述符 fd 中读取最多 n 个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串。||
|os.readlink(path)|返回软链接所指向的文件||
|os.remove(path)|删除路径为path的文件。如果path 是一个文件夹，将抛出OSError; 查看下面的rmdir()删除一个 directory。||
|os.removedirs(path)|递归删除目录。||
|os.rename(src, dst)|重命名文件或目录，从 src 到 dst||
|os.renames(old, new)|递归地对目录进行更名，也可以对文件进行更名。||
|os.rmdir(path)|删除path指定的空目录，如果目录非空，则抛出一个OSError异常。||
|os.stat(path)|获取path指定的路径的信息，功能等同于C API中的stat()系统调用。||
|os.stat_float_times([newvalue])|决定stat_result是否以float对象显示时间戳||
|os.statvfs(path)|获取指定路径的文件系统统计信息||
|os.symlink(src, dst)|创建一个软链接||
|os.system()|执行shell命令|`var=123;os.environ[‘var‘]=str(var) //注意此处[]内得是 “字符串”;`<br>`os.system('echo $var')`|
|os.tcgetpgrp(fd)|返回与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组||
|os.tcsetpgrp(fd, pg)|设置与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组为pg。||
|os.tempnam([dir[, prefix]])|Python3 中已删除。返回唯一的路径名用于创建临时文件。||
|os.tmpfile()|Python3 中已删除。返回一个打开的模式为(w+b)的文件对象 .这文件对象没有文件夹入口，没有文件描述符，将会自动删除。||
|os.tmpnam()|Python3 中已删除。为创建一个临时文件返回一个唯一的路径||
|os.ttyname(fd)|返回一个字符串，它表示与文件描述符fd 关联的终端设备。如果fd 没有与终端设备关联，则引发一个异常。||
|os.unlink(path)|删除文件路径||
|os.utime(path, times)|返回指定的path文件的访问和修改的时间。||
|os.walk(top[, topdown=True[, onerror=None[, followlinks=False]]])|输出在文件夹中的文件名通过在树中游走，向上或者向下。||
|os.write(fd, str)|写入字符串到文件描述符 fd中. 返回实际写入的字符串长度||
|os.path 模块|获取文件的属性信息。||
|os.pardir()|获取当前目录的父目录，以字符串形式显示目录名。||

|os.path 模块|说明|举例|
|--|--|--|
|os.path.abspath(path)|	返回绝对路径||
|os.path.basename(path)|	返回文件名||
|os.path.commonprefix(list)|	返回list(多个路径)中，所有path共有的最长的路径||
|os.path.dirname(path)|	返回文件路径||
|os.path.exists(path)|	路径存在则返回True,路径损坏返回False||
|os.path.lexists	路径存在则返回True,路径损坏也返回True||
|os.path.expanduser(path)|	把path中包含的"~"和"~user"转换成用户目录||
|os.path.expandvars(path)|	根据环境变量的值替换path中包含的`$name`和`${name}`||
|os.path.getatime(path)|	返回最近访问时间（浮点型秒数）||
|os.path.getmtime(path)|	返回最近文件修改时间||
|os.path.getctime(path)|	返回文件 path 创建时间||
|os.path.getsize(path)|	返回文件大小，如果文件不存在就返回错误||
|os.path.isabs(path)|	判断是否为绝对路径||
|os.path.isfile(path)|	判断路径是否为文件||
|os.path.isdir(path)|	判断路径是否为目录||
|os.path.islink(path)|	判断路径是否为链接||
|os.path.ismount(path)|	判断路径是否为挂载点||
|os.path.join(path1[, path2[, ...]])|	把目录和文件名合成一个路径||
|os.path.normcase(path)|	转换path的大小写和斜杠||
|os.path.normpath(path)|	规范path字符串形式||
|os.path.realpath(path)|	返回path的真实路径||
|os.path.relpath(path[, start])|	从start开始计算相对路径||
|os.path.samefile(path1, path2)|	判断目录或文件是否相同||
|os.path.sameopenfile(fp1, fp2)|	判断fp1和fp2是否指向同一文件||
|os.path.samestat(stat1, stat2)|	判断stat tuple stat1和stat2是否指向同一个文件||
|os.path.split(path)|	把路径分割成 dirname 和 basename，返回一个元组||
|os.path.splitdrive(path)|	一般用在 windows 下，返回驱动器名和路径组成的元组||
|os.path.splitext(path)|	分割路径中的文件名与拓展名||
|os.path.splitunc(path)|	把路径分割为加载点与文件||
|os.path.walk(path, visit, arg)|	遍历path，进入每个目录都调用visit函数，visit函数必须有3个参数(arg, dirname, names)，dirname表示当前目录的目录名，names代表当前目录下的所有文件名，args则为walk的第三个参数||
|os.path.supports_unicode_filenames	|设置是否支持unicode路径名||

## sys
- 引入：`import sys`
- 函数

|函数|说明|举例|
|--|--|--|
|sys.argv |接收命令行参数，生成一个List，第一个元素是程序本身路径||
|sys.modules        |返回系统导入的模块字段，key是模块名，value是模块||
|sys.modules.keys()| 返回所有已经导入的模块列表||
|sys.modules.values() |  返回所有已经导入的模块||
|sys.exc_info() |获取当前正在处理的异常类,exc_type、exc_value、exc_traceback当前处理的异常详细信息||
|sys.exit(n) |退出程序，正常退出时exit(0)||
|sys.hexversion |获取Python解释程序的版本值，16进制格式如：0x020403F0||
|sys.version |获取Python解释程序的版本信息||
|sys.maxint |最大的Int值||
|sys.maxunicode |最大的Unicode值||
|sys.modules |返回系统导入的模块字段，key是模块名，value是模块||
|sys.path |返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值||
|sys.platform |返回操作系统平台名称||
|sys.stdout |标准输出| sys.stdout.write('please:')|
|sys.stdin |标准输入| val = sys.stdin.readline()[:-1]|
|sys.stderr |错误输出||
|sys.exc_clear() |用来清除当前线程所出现的当前的或最近的错误信息||
|sys.exec_prefix |返回平台独立的python文件安装的位置||
|sys.byteorder |本地字节规则的指示器，big-endian平台的值是'big',little-endian平台的值是'little'||
|sys.copyright |记录python版权相关的东西||
|sys.api_version |解释器的C的API版本||
|sys.version_info          |‘final’表示最终,也有’candidate’表示候选，serial表示版本级别，是否有后继的发行||
|sys.displayhook(value)   |   如果value非空，这个函数会把他输出到sys.stdout，并且将他保存进`__builtin__._.`指在python的交互式解释器里，’_’ 代表上次你输入得到的结果，hook是钩子的意思，将上次的结果钩过来||
|sys.getdefaultencoding() |   返回当前你所用的默认的字符编码格式||
|sys.getfilesystemencoding()| 返回将Unicode文件名转换成系统文件名的编码的名字||
|sys.setdefaultencoding(name)|用来设置当前默认的字符编码，如果name和任何一个可用的编码都不匹配，抛出 LookupError，这个函数只会被site模块的sitecustomize使用，一旦别site模块使用了，他会从sys模块移除||
|sys.builtin_module_names    |Python解释器导入的模块列表||
|sys.executable              |Python解释程序路径||
|sys.getwindowsversion()    | 获取Windows的版本||
|sys.copyright      |记录python版权相关的东西||

## time

## datetime

## collection
- Python拥有一些内置的数据类型，比如str, int, list, tuple, dict等， collections模块在这些内置数据类型的基础上，提供了许多有用的集合类。[更多详细内容](https://www.jianshu.com/p/47f66ff4ab7b)
  1. namedtuple(): 生成可以使用名字来访问元素内容的tuple子类
  2. deque: 双端队列，可以快速的从另外一侧追加和推出对象
  3. Counter: 计数器，主要用来计数
  4. OrderedDict: 有序字典
  5. defaultdict: 带有默认值的字典

- 引入：`from collection import *`


### Counter
- 初始化
```python
c = Counter()#一个新的，空的counter
>>> Counter()
c = Counter("gallahad")#从可迭代的字符串初始化counter
>>> Counter({'a': 3, 'l': 2, 'h': 1, 'g': 1, 'd': 1})
c = Counter({'red':4,'blue':2})#从映射初始化counter
>>> Counter({'red': 4, 'blue': 2})
c = Counter(cats = 4,dogs = 8)#从args初始化counter
>>> Counter({'dogs': 8, 'cats': 4})
```

- 取值
```python
>>> c = Counter(['eggs','ham'])
# Counter对象类似于字典，如果某个项缺失，会返回0，而不是报出KeyError；
c['bacon']#没有'bacon' 返回0
c['eggs']#有'eggs' 返回1
```

- 删除：将一个元素的数目设置为0，并不能将它从counter中删除，使用del可以将这个元素删除；
```python
Counter({'eggs': 1, 'ham': 1})
c['eggs'] = 0
>>> Counter({'ham': 1, 'eggs': 0})#'eggs'依然存在
del c['eggs']
>>> Counter({'ham': 1})#'eggs'不存在  
```

- Counter对象支持以下三个字典不支持的方法

|函数|说明|举例|
|--|--|--|
|element()|返回一个迭代器，每个元素重复的次数为它的数目，顺序是任意的顺序，如果一个元素的数目少于1，那么elements()就会忽略它；||
|most_common()|返回一个列表，包含counter中n个最大数目的元素，如果忽略n或者为None，most_common()将会返回counter中的所有元素，元素有着相同数目的将会以任意顺序排列；||
|subtract()|从一个可迭代对象中或者另一个映射（或counter）中，元素相减，类似于dict.update()，但是subtracts 数目而不是替换它们，输入和输出都有可能为0或者为负；||
|update()|从一个可迭代对象中或者另一个映射（或counter）中所有元素相加，类似于dict.update，是数目相加而非替换它们，另外，可迭代对象是一个元素序列，而非(key,value)对构成的序列；||

```python3
# element
c = Counter(a=2,b=4,c=0,d=-2,e = 1)
list(c.elements())
>>> ['a', 'a', 'b', 'b', 'b', 'b', 'e']
# most_common
Counter('abracadabra').most_common(3)
>>> [('a', 5), ('r', 2), ('b', 2)]
# subtract
c = Counter(a=4,b=2,c=0,d=-2);d = Counter(a=1,b=2,c=-3,d=4);c.subtract(d);
>>> Counter({'a': 3, 'c': 3, 'b': 0, 'd': -6})
# update
c = Counter(a=4,b=2,c=0,d=-2);d = Counter(a=1,b=2,c=-3,d=4);c.subtract(d);c.update(d)
>>> Counter({'a': 5, 'b': 4, 'd': 2, 'c': -3})
```

- 其他常见操作

|函数|说明|举例|
|--|--|--|
|sum(c.values())| 统计所有的数目|c=Counter({'a': 5, 'b': 4, 'd': 2, 'c': -3})|
|list(c)| 列出所有唯一的元素|['a', 'c', 'b', 'd']|
|set(c)| 转换为set|set(['a', 'c', 'b', 'd'])|
|dict(c)| 转换为常规的dict|{'a': 5, 'c': -3, 'b': 4, 'd': 2}|
|c.items()| 转换为(elem,cnt)对构成的列表|[('a', 5), ('c', -3), ('b', 4), ('d', 2)]|
|c.most_common()[:-4:-1]| 输出n个数目最小元素|[('c', -3), ('d', 2), ('b', 4)]|
| c += Counter()| 删除数目为0和为负的元素|Counter({'a': 5, 'b': 4, 'd': 2})|
|Counter(dict(c.items()))| 从(elem,cnt)对构成的列表转换为counter|Counter({'a': 5, 'b': 4, 'd': 2})|
|c.clear()| 清空counter||
|c = Counter(a=3,b=1,c=-2)<br>d = Counter(a=1,b=2,c=4)|||
|c+d|求和|Counter({'a': 4, 'b': 3, 'c': 2})|
|c-d|求差|Counter({'a': 2})|
|c & d|求交集|Counter({'a': 1, 'b': 1})|
|`c | d`|求并集|Counter({'c': 4, 'a': 3, 'b': 2})|

### deque
 - deque是栈和队列的一种广义实现，deque是"double-end queue"的简称；deque支持线程安全、有效内存地以近似O(1)的性能在deque的两端插入和删除元素，尽管list也支持相似的操作，但是它主要在固定长度操作上的优化，从而在pop(0)和insert(0,v)（会改变数据的位置和大小）上有O(n)的时间复杂度。

|函数|说明|举例|
|--|--|--|
|append(x)|将x添加到deque的右侧；||
|appendleft(x)|将x添加到deque的左侧；||
|clear()| 将deque中的元素全部删除，最后长度为0；||
|count(x)|返回deque中元素等于x的个数；||
|extend(iterable)|将可迭代变量iterable中的元素添加至deque的右侧；||
|extendleft(iterable)|将变量iterable中的元素添加至deque的左侧，往左侧添加序列的顺序与可迭代变量iterable中的元素相反；||
|pop()|移除和返回deque中最右侧的元素，如果没有元素，将会报出IndexError；||
|popleft()| 移除和返回deque中最左侧的元素，如果没有元素，将会报出IndexError；||
|remove(value)|移除第一次出现的value，如果没有找到，报出ValueError；||
|reverse()|反转deque中的元素，并返回None；||
|rotate(n)|从右侧反转n步，如果n为负数，则从左侧反转，d.rotate(1)等于d.appendleft(d.pop())；||
|maxlen|只读的属性，deque的最大长度，如果无解，就返回None；||

- 除了以上的方法之外，deque还支持迭代、序列化、len(d)、reversed(d)、copy.copy(d)、copy.deepcopy(d)，通过in操作符进行成员测试和下标索引，索引的时间复杂度是在两端是O(1)，在中间是O(n)，为了快速获取，可以使用list代替。

- 还有更多有意思的功能，例如实现类似linux的tail的功能
```python
from collections import deque
def tail(filename,n = 10):
    "Return the last n lines of a file"
    return deque(open(filename),n)
print (tail("temp.txt",10))
```

- 使用deque维护一个序列（右侧添加元素，左侧删除元素）中窗口的平均值
```python
from collections import deque
import itertools
def moving_average(iterable,n = 3):
    it = iter(iterable)
    d = deque(itertools.islice(it,n-1))
    # 第一次只有两个元素，再右移的过程中，需要先删除最左端的元素，因此现在最左端加入0
    d.appendleft(0)
    s = sum(d)
    for ele in it:
        # 删除最左端的元素，再加上新元素
        s += ele - d.popleft()
        # 右端添加新元素
        d.append(ele)
        yield s / float(n)
array = [40,30,50,46,39,44]
for ele in moving_average(array,n=3):
    print (ele)
```

### defaultdict
- efaultdict是内置数据类型dict的一个子类，基本功能与dict一样，只是重写了一个方法missing(key)和增加了一个可写的对象变量default_factory。

|函数|说明|举例|
|--|--|--|
|missing(key)|1、如果default_factory属性为None，就报出以key作为遍历的KeyError异常；<br>2、如果default_factory不为None，就会向给定的key提供一个默认值，这个值插入到词典中，并返回；<br>3、如果调用default_factory报出异常，这个异常在传播时不会改变；<br>4、这个方法是当要求的key不存在时，dict类中的getitem()方法所调用，无论它返回或者报出什么，最终返回或报出给getitem()；<br>5、只有getitem()才能调用missing()，这意味着，如果get()起作用，如普通的词典，将会返回None作为默认值，而不是使用default_factory；||
|default_factory|这个属性用于missing()方法，使用构造器中的第一个参数初始化；<br>设置list会创建空的list，遇到key则将对应的value值append到列表中；<br>设置int会提供一个默认为0的计数，遇到相同的key会递增操作；<br>设置set则可以建立一个空的set||

- 使用list作为default_factory，很容易将一个key-value的序列转换为一个关于list的词典；
```python
s = [('yellow',1),('blue',2),('yellow',3),('blue',4),('red',5)]
d = defaultdict(list) //设置default_factory为list，append操作会将value值append这个list
for k,v in s: d[k].append(v)
d.items()
>>> [('blue', [2, 4]), ('red', [5]), ('yellow', [1, 3])]
```

### namedtuple
- 命名的元组，意味给元组中的每个位置赋予含义，意味着代码可读性更强，namedtuple可以在任何常规元素使用的地方使用，而且它可以通过名称来获取字段信息而不仅仅是通过位置索引。
```python
Point = namedtuple('Point',['x','y'],verbose = True)
p = Point(11,y = 22) # 实例化一个对象，可以使用位置或者关键字
p[0] + p[1] # 通过索引访问元组中的元素
x,y = p # 分开，类似于常规的元组
print(x,y)
>>> (11, 22)
p.x + p.y # 通过名称访问元素
p # 可读的__repr__，通过name = value风格
>>> Point(x=11, y=22)
```

- 除了从tuples继承的方法之外，namedtuple还支持三种方法和一个属性

|函数|说明|举例|
|--|--|--|
|`somenamedtuple._make()`| 从已有的序列或者可迭代的对象中创建一个新的对象；|`t = [11,22];Point._make(t)`|
|`somenamedtuple._asdict()`| 返回一个OrderDict，由名称到对应值建立的映射；|`p._asdict() ==> OrderedDict([('x', 11), ('y', 22)])`|
|`somenamedtuple._replace()`| 返回一个新的namedtuple对象，用新值替换指定名称中的值；|`p._replace(x = 33) ==> Point(x=33, y=22)`|
|`somenamedtuple._fields`| 以字符串构成的元组的形式返回namedtuple中的名称，在自省或者基于一个已经存在的namedtuple中创建新的namedtuple时，非常有用；|` p._fields ==> ('x', 'y')`|
|`getattr()`|检索存储在字符串中的名称|`getattr(p,'x')==>11`|
|`**操作符`|将一个字典转换成namedtuple|`d  = {'x':11,'y':22};Point(**d)`|

- 由于namedtuple也是Python中的一个类，因此在子类中，它很容易添加或者修改一些功能，如下是添加一个可计算名称和固定长度的输出格式；子类中的slots是一个空的元组，可以通过避免词典实例的创建来节约内存；
```python
class Point(namedtuple('Point','x y')):
    __slots__ = ()
    @property
    def hypot(self):
        return (self.x ** 2 + self.y**2) ** 0.5
    def __str__(self):
        return "Point:x = %6.3f  y = %6.3f  hypot = %6.3f" %(self.x,self.y,self.hypot)
for p in Point(3,4),Point(14,5/7.):
    print (p)
>>> Point:x =  3.000  y =  4.000  hypot =  5.000
>>> Point:x = 14.000  y =  0.714  hypot = 14.018
```

### OrderedDict
- OrderedDict类似于正常的词典，只是它记住了元素插入的顺序，当在有序的词典上迭代时，返回的元素就是它们第一次添加的顺序。

|函数|说明|举例|
|--|--|--|
|popitem(last=True)|返回和删除一个(key,value)对，如果last=True，就以LIFO方式执行，否则以FIFO方式执行||
|reversed()|反向迭代||
|== |相等测试，OrderedDict对象之间对顺序敏感，和其他的映射对象不敏感||
|update()|OrderedDict构造器和update()方法可以接受关键字变量，但是它们丢失了顺序，因为Python的函数调用机制是将一个无序的词典传入关键字变量||


- 使用举例
- 创建一个有序的词典，可以记住最后插入的key的顺序，如果一个新的元素要重写已经存在的元素，那么原始的插入位置就会改变成末尾，

```python
class LastUpdatedOrderedDict(OrderedDict):
    def __setitem__(self,key,value):
        if key in self:
            del self[key]
        OrderedDict.__setitem__(self, key, value)
obj = LastUpdatedOrderedDict();obj["apple"] = 2;obj["windows"] = 3;
>>> LastUpdatedOrderedDict([('apple', 2), ('windows', 3)])
obj["apple"] = 1
>>> LastUpdatedOrderedDict([('windows', 3), ('apple', 1)])
```

- 一个有序的词典可以和Counter类一起使用，counter对象就可以记住元素首次出现的顺序；
```python3
class OrderedCounter(Counter,OrderedDict):
    def __repr__(self):
        return "%s(%r)"%(self.__class__.__name__,OrderedDict(self))

    def __reduce__(self):
        return self.__class__,(OrderedDict(self))
#和OrderDict一起使用的Counter对象
obj = OrderedCounter()
wordList = ["b","a","c","a","c","a"]
for word in wordList:
    obj[word] += 1
print (obj)
# 普通的Counter对象
cnt = Counter()
wordList = ["b","a","c","a","c","a"]
for word in wordList:
    cnt[word] += 1
print (cnt)
>>> OrderedCounter(OrderedDict([('b', 1), ('a', 3), ('c', 2)]))
>>> Counter({'a': 3, 'c': 2, 'b': 1})
```

## heapq
- 堆是一种特殊的树形结构，通常我们所说的堆的数据结构指的是完全二叉树，并且根节点的值小于等于该节点所有子节点的值
- 堆是一种优先队列。优先队列让你能够以任意顺序添加对象，并随时（可能是在两次添加对象之间）找出（并删除）最小的元素。相比于列表方法min，这样做的效率要高得多。

|函数|说明|举例|
|--|--|--|
|`heappush(heap,item)`|往堆中插入一条新的值，构造成最小堆||
|`heappop(heap)`|从堆中弹出最小值||
|`heapreplace(heap,item)`|从堆中弹出最小值，并往堆中插入item||
|`heappushpop(heap,item)`|Python3中的heappushpop更高级||
|`heapify(x)`|以线性时间将一个列表转化为堆||
|`merge(*iterables,key=None,reverse=False)`|合并对个堆，然后输出||
|`nlargest(n,iterable,key=None)`|返回可枚举对象中的n个最大值并返回一个结果集list||
|`nsmallest(n,iterable,key=None)`|返回可枚举对象中的n个最小值并返回一个结果集list||

- 创建堆：可以遍历列表，然后通过heapq.heappush()函数把值加入堆中，还可以使用heap.heapify(list)转换列表成为堆结构。
- 访问堆内容：堆创建好后，可以通过`heapq.heappop()`函数弹出堆中最小值。
- 获取堆最大或最小值：如果需要获取堆中最大或最小的范围值，则可以使用`heapq.nlargest()`或`heapq.nsmallest()`函数

- heapq支持最大堆：
|函数|说明|举例|
|--|--|--|
|`_heapify_max(list)`|生成最大堆||
|`_heappop_max(heap)`|从堆中弹出最大值||
|`_heapreplace_max(heap,item)`|从对中弹出最大值，并插入item||

- 但是heapq不支持maxpush，push数值得到的堆不是最大堆。
- 改进方法可以在数据添加负号来解决调性。

## itertools











# 其他