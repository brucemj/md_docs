# Python基础教程第二版 笔记


## 第一章 基础知识 ##

## 第二章 列表和元组 ##
1. 序列概览
	- 6种内建的序列： 列表，元组，字符串，Unicode字符串，buffer对象 和 xrange对象 
	- 容器( container )数据结构：容器基本上是包含其他对象的任意对象。 
	- 序列（如列表和元组）和映射（如字典）是两类主要的容器。 至于即不是序列，也不是映射的容器类型，集合(set)就是一个例子。

2. 通用序列操作
	- 索引（indexing），分片（sliceing），加（adding），乘（multiplying）
	- 以及检查某个元素是否属于序列的成员（成员资格）。
	- 此外还有，序列长度，最大元素，最小元素 的内建函数。
	
	- 成员资格 in：
		- 'w' in 'rw' 
		- True
	
3. 列表： Python 的 “苦力” 
	1. list函数 ，根据字符串创建列表
		- list('hello')
		- ['h','e','l','l','o']
		- 【注】 list适用于所有类型的序列，不只是字符串
	2. 基本的列表操作：
		- 改变列表，元素赋值 （不能为不存在的元素赋值）
		- 删除元素
			- names = ['alice', 'bill']
			- del names[0]
			- ['bill']
		- 分片赋值
			- name = list('Perl')
			- name[1:] = list('ython')
			- ['P', 'y', 't', 'h', 'o', 'n']
	3. 列表方法
		- append 在列表末尾追加新的对象
			- a = [1,2,3]
			- b = [4,5,6]
			- a.append(b)
			- [1,2,3 , [4,5,6] ]
		- count 统计某个元素在列表中出现的次数
		- extend 在列表末尾追加另外一个序列中的多个值，类似加法，但会改变序列值
			- a = [1,2,3]
			- b = [4,5,6]
			- a.extend(b)
			- [1,2,3 ,4,5,6 ]
			- #########################
			- a = [1,2,3]
			- b = [4,5,6]
			- a + b  ## [1,2,3 ,4,5,6 ]
			- # a 的值没有改变
			- # a = a + b  连接操作的效率会比 extend 方法低。
		- index 从列表中找出某个值的第一个匹配项的索引
			- name = ['i' , 'we' , 'you', 'we']
			- name.index('we')
			- 1
		- insert 将对象插入到列表中
			- name = [1,2,3,4,5]
			- name.insert(3,'four')
			- [1,2,3, 'four' ,4,5]
		- pop 移除列表中的一个元素（默认是最后一个），且返回该元素值
			- x = [1,2,3]
			- x.pop()
			- 3
			- x
			- [1,2]
			- 【注】 pop 方法是唯一一个既能修改列表又返回元素值（除了None）的列表方法 
			- 使用pop可以实现一种常见的数据结构--栈。 LIFO 先进先出
		- remove 移除列表中某个值的第一个匹配项
			- x = ['to', 'be' , 'or', 'be']
			- x.remove('be')
			- x
			- ['to', 'or', 'be']
			- ## remove 是一个没有返回值的原位置改变方法，它修改了列表却没有返回值，这与pop方法相反。
		- reverse 将列表中的元素反向存放
			- x = [1,2,3]
			- x.reverse()
			- x
			- [3,2,1]
			- ## 如需对一个序列进行反向迭代，可以试用reversed函数。这个函数并不返回一个列表，而是返回一个迭代器(iterator)对象。 可以试用list函数把返回的对象转换成列表也是可行的。
		- sort 在原位置对列表进行排序。
			- “原位置排序”意味着改变原来的列表，而不是简单的返回一个已排序的副本。
			- x = [1,3,2,4]
			- x.sort()
			- x
			- [1,2,3,4]
			- ### sort 返回空值
			- x = [1,3,2,4]
			- y = x.sort() # Don't do this !
			- print y
			- None
			- ### 正确的获得x副本的做法
			- x = [1,3,2,4]
			- y = x[:]
			- y.sort()
			- x
			- [1,3,2,4]
			- y
			- [1,2,3,4]
			- ### 调用 x[:]得到包含了x所有元素的分片，是一种很有效率的复制整个列表的方法。
			- ### 只是简单的把x赋值给y是没用的，因为这样，x和y都指向同一个列表了。
			- 另一种获取已排序的列表副本的方法是，使用sorted函数：
			- x = [1,3,2,4]
			- y = sorted(x)
			- x
			- [1,3,2,4]
			- y
			- [1,2,3,4]
			- sorted实际上可以用于任何序列，却总是返回一个列表
			- sorted('Python')
			- ['P','h','n','o','t','y']
			- ### 如果想相反的顺序排序，可以调用 reverse 方法。
		- 高级排序
			- 可以为 sort 或 sorted 函数指定参数，来自定义排序
			- ### 比较函数 ,可自定义
			- cmp(42,32)
			- 1
			- cmp(99,100)
			- -1
			- cmp(10,10)
			- 0
			- num = [5,2,9,7]
			- num.sort(cmp)
			- num
			- [2,5,7,9]
			- ### sort 返回还有另外两个可选的参数-- key 和 reverse
			- 参数key和 参数cmp类似，必须提供一个在排序过程中使用的函数
			- x = ['bb' , 'a' , 'aaaa']
			- x.sort(key=len)
			- x
			- ['a', 'bb', 'aaa']
			- reverse是简单的布尔值，指明列表是否要进行反向排序。
	4. 元组： 不可变序列
		- 元组创建
			- ### 也可以不用括号
			- 1，2，3
			- (1，2，3)
			- ### 用括号也可以
			- (1，2，3)
			- (1，2，3)
			- ### 一个值的元组 , 逗号很重要
			- 42
			- 42
			- 42，
			- (42,)
			- (42,)
			- (42,)
			- ### (42)和42是完全一样的，但是，一个逗号却能彻底改变表达式的值
			- 3*(40+2)
			- 126
			- 3*(40+2,)
			- (42,42,42)
		- tuple 函数
			- 与list函数基本是一样的，以一个序列作为参赛，并把它转换为元组
			- 如果参数数元组，则原样返回
			- tuple([1,2,3])
			- (1,2,3)
			- tuple('abc')
			- ('a','b','c')
			- tuple((1,2,3))
			- (1,2,3)
		- 基本元组操作
			- 元组其实并不复杂，除了元组创建和访问元组元素之外，没有太多其他操作。
			- x = 1,2,3
			- x[1]
			- 2
			- x[0:2]
			- (1,2)
			- ### 元组的分片还是元组
		- 元组，意义何在
			- 有两个重要原因，元组是不可替代的
				- 元组可以在映射(和集合的成员)中当作键使用-- 而列表则不行
				- 元组作为很多内建函数和方法的返回值存在，也就是说你必须对元组进行处理。
				  只有不尝试修改元组，那么“处理”元组在绝大多数情况下就是把他们当作列表来进行操作。

## 第三章 使用字符串 ##
1. a
2. b








## 第四章 字典：当索引不好用时 ##
1. dict函数
	- 可以用 dict，通过其他映射(比如其他字典)或者 (键,值)这样的序列建立字典。
		- items = [('name','Gumby'), ('age', 42)]
		- d = dict(items)
		- d
		- {'name':'Gumby', 'age':42}
		- ### 还可以通过关键字来创建字典
		- d = dict(name='Gumby', age=42)
		- d
		- {'age':42, 'name':'Gumby'}

2. 基本字典操作
	- 字典的基本行为在很多方面与序列(sequence)类似：
	- len(d)
	- d[k]
	- d[k] = v
	- del d[k]
	- k in d

	- 重要区别
	- 键类型： 整数类型，或者其他不可变类型，比如：浮点型，字符串或者元组
	- 自动添加：可以为不存在的键分配值，会建立新的项。
	- 成员资格：表达式 k in d 查找的是键，而不是值。
	- 提示： 在字典中检查成员资格比在列表中更高效，数据越大，差距越明显。

3. 字典的格式化字符串
	- phonebook
	- {'Beth':'9102' , 'Cecil':'3258'}
	- "Cecil's phone number is %(Cecil)s." % phonebook
	- "Cecil's phone number is 3258."

4. 字典方法
	- clear 清楚字典中的所有的项。 原地操作，所以无返回值（或者说返回None）
		- >>> x = {}
		- >>> y = x
		- >>> x['key'] = 'v'
		- >>> y
		- {'key': 'v'}
		- >>> x = {}
		- >>> y
		- {'key': 'v'}
		- ###########################
		- >>> x = {}
		- >>> y = x
		- >>> x['key'] = 'v'
		- >>> y
		- {'key': 'v'}
		- >>> x.clear()
		- >>> y
		- {}

	- copy 返回一个具有相同键-值对的新字典。 （这个方法实现的是浅复制（shallow copy），因为值本身就是相同的，而不是副本）。
		- >>> x  = {'username': 'admin', 'machines': ['foo', 'bar', 'baz']}
		- >>> y = x.copy()
		- >>> y['username'] = 'mlh'
		- >>> y['machines'].remove('bar')
		- >>> y
		- {'username': 'mlh', 'machines': ['foo', 'baz']}
		- >>> x
		- {'username': 'admin', 'machines': ['foo', 'baz']}
		- 可以看到，当在副本中替换值的时候，原始字典不受影响，但是，如果修改了某个值（原地修改，而不是替换），原始的字典也会改变，因为同样的值也存储在原字典中。
		- 避免这个问题的一种方法可以试用深复制（deep copy）

	- fromkeys 使用给定的键建立新的字典，每个键默认对应的值为None
		- {}.fromkeys(['name', 'age'])
		- {'aget': None, 'name': None}
		- #######
		- {}.fromkeys(['name', 'age'], 'unknown')
		- {'aget': 'unknown', 'name': 'unknown'}
		
	- get 是一个更宽松的访问字典项的方法。 访问不存在的项时不会出错
		- d = {}
		- print d.get('name')
		- None
		- ### 还可以自定义‘默认’值
		- print d.get('name' , 'N/A')
		- 'N/A'
	
	- has_key 检查字典中是否含有给出的键。相当于 k in d. 但python3.0不包括这个函数
	
	- items 和 iteritems
		- items 将所有的字典项以列表方式返回，列表的每一项都来自于(键，值).但返回时没有特殊顺序。
		- iteritems 的作用大致相同，但是会返回一个迭代器对象而不是列表。
		- ### 很多情况下使用 iteritems 更高效。
	
	- keys 和 iterkeys
		- 将字典中的键以列表形式返回
		- iterkeys 则返回针对键的迭代器
	
	- pop 获取对应于给定键的值，然后将这个键-值对从字典中删除。
		- >>> d = {'x':1, 'y':2}
		- >>> d.pop('x')
		- 1
		- >>> d
		- {'y': 2}
		- >>>

	- popitem 类似于list.pop,后者会弹出列表的最后一个元素。但不同的是，popitem弹出随机的项，因为字典并没有'最后的元素'

	- setdefault 类似get方法，就是能获取与给定键相关的值，除此之外，还能在字典中不含有给定键的情况下设定相应的键值。
	
	- update 可以利用一个字典项更新另外一个字典：
		- >>> d = {'x':1, 'y':2}
		- >>> e = {'y':3}
		- >>> d.update(e)
		- >>> d
		- {'y': 3, 'x': 1}

	- values 和 itervalues 以列表的形式返回字典中的值（itervalues返回值的迭代器）。 与返回键不同的是，返回值的列表中可以包含重复的元素。
	
## 第五章 条件，循环和其他 ##