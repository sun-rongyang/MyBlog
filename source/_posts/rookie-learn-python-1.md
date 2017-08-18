---
title: 菜鸟学Python(1) - 变量与基本数据类型
date: 2016-08-12 19:23:00
tags: [python]
categories: 
    - learn coding
    - python
---

作为一个非IT专业的菜鸟, 喜欢折腾, 没事学学python自娱自乐. 本文介绍了Python的基本变量类型并重点分析了list与tuple这两种数据类型背后的关于Python中数据存储和引用的特点.
<!-- more -->

> \# 所有的测试在python3.5.2中进行, 并使用IPython shell


### 变量和数据类型
变量区分大小写, 不能用数字开头.

- 整数
- 浮点数
    ```python
    # 以上两种数据类型在python中是不区分的
    In [1]: 3+2
    Out[1]: 5

    In [2]: 3+3.1
    Out[2]: 6.1
    ```
- 字符串
    用单引号或双引号引起来的任意文本. python中转义字符为\ .

    ```python
    In [27]: print('I\'m \"ok\"')
    I'm "ok"
    ```
- 布尔值
    True 和 False. 逻辑运算符为 and, or, not

    ```python
    In [38]: True
    Out[38]: True

    In [39]: 3 > 4
    Out[39]: False
    ```

- 空值
    和 matlab 比较相似, 为None.

- 列表 List
    语法是 [ element1, element2, element3 ]

    ```python
    In [47]: list1 = [2,3,4]
    # list 索引是从0开始的
    In [50]: list1[0]
    Out[50]: 2
    
    In [48]: list1[1]
    Out[48]: 3
    # 灵活运用倒数的索引
    In [49]: list1[-2]
    Out[49]: 3
    # 在list尾部追加一个元素
    In [58]: list1.append(5)
    In [60]: list1
    Out[60]: [2, 3, 4, 5]
    # 在list的指定位置插入元素
    In [61]: list1.insert(0,1)
    In [62]: list1
    Out[62]: [1, 2, 3, 4, 5]
    # 在list中删除某些元素, 没有指定索引时默认最后一个元素
    In [75]: list1.pop()
    Out[75]: 5
    In [76]: list1.pop(0)
    Out[76]: 1
    # list中的元素也可以是不同的数据类型, 也可以是list, 因此可以嵌套形成高维数组.
    In [77]: list3 = [1, 'Alice', True, None, [2,4] ]
    In [78]: list3
    Out[78]: [1, 'Alice', True, None, [2, 4]]
    # 测量list长度的函数len()
    In [81]: len(list1)
    Out[81]: 3
    ```
    
- 元组 tuple
    tuple和list非常相似, 区别在于tuple的不可变性. tuple一旦被初始化里面的元素就不能变动了(如果元素有list, 变动list中的元素那就另当别论了), 除非重新初始化(整体赋值)这个tuple. tuple的语法是 tuple(element1, element2, element3).

    ```python
    In [111]: tuple3 = (1,) #注意赋值一个element的tuple时要加一个"," 以区分数学运算中的小括号
    In [112]: tuple3
    Out[112]: (1,)
    In [113]: tuple3[0]
    Out[113]: 1
    ```
    
**从list和tuple的区别看python中的引用问题**
>1. python不允许程序员选择采用传值还是传引用。Python参数传递采用的肯定是“传对象引用”的方式。实际上，这种方式相当于传值和传引用的一种综合。如果函数收到的是一个可变对象（比如字典或者列表）的引用，就能修改对象的原始值——相当于通过“传引用”来传递对象。如果函数收到的是一个不可变对象（比如数字、字符或者元组）的引用，就不能直接修改原始对象——相当于通过“传值'来传递对象。
>2. 当人们复制列表或字典时，就复制了对象列表的引用，如果改变引用的值，则修改了原始的参数。
>3. 为了简化内存管理，Python通过引用计数机制实现自动垃圾回收功能，Python中的每个对象都有一个引用计数，用来计数该对象在不同场所分别被引用了多少次。每当引用一次Python对象，相应的引用计数就增1，每当消毁一次Python对象，则相应的引用就减1，只有当引用计数为零时，才真正从内存中删除Python对象。

```python
# example 1
In [104]: list1
Out[104]: [2, 3]

In [105]: list2
Out[105]: [1, 2, 3]

In [106]: list2 = list1

In [107]: list2
Out[107]: [2, 3]

In [108]: list1.insert(0,1)

In [109]: list1
Out[109]: [1, 2, 3]

In [110]: list2
Out[110]: [1, 2, 3]
# 这是一个关于list的例子. 在内存中我们假设"list1"这个名字存放在A处, 其值[2, 3]存放在B处, "list2"这个名字存放在C处, 其值[1, 2, 3]存放在D处. 开始时 A-->B C-->D, list2 = list1 相当于C-->B, 这时A和C同时指向了B. 当使用.insert操纵A时, 改变了B, 所以在调用C时, C的值发生了变化.
# 相比于list, tuple 因为一旦被初始化就不能改变了(除非重新初始化), 所以没有这个问题.
```
- 字典 dict
python提供的 key-value 形式数据类型, 语法为 dict = {'key1': value1, 'key2': value2, 'key3': value3}. 

    ```python
    # dict的初始化
    In [7]: dict1 = {'key1': 1, 'key2': 'value2', 'key3': True}

    In [8]: dict1
    Out[8]: {'key1': 1, 'key2': 'value2', 'key3': True}
    # dict的value可以是任意的对象
    In [9]: not dict1['key3']
    Out[9]: False
    # 判断某个key是否存在
    In [10]: 'key4' in dict1
    Out[10]: False
    
    In [11]: dict1.get('key4', -1) # 使用dict自带的get方法判断某个key是否存在, 如果存在就返回对应的value, 如果不存在就返回指定的值(这里是 -1)
    Out[11]: -1
    # 删除某个key, 对应的value也同时被删除.
    In [12]: dict1.pop('key2')
    Out[12]: 'value2'

    In [13]: dict1
    Out[13]: {'key1': 1, 'key3': True}
    # key也可以不是字符串, 但必须是"不可变对象"
    In [14]: dict1[666] = 888 

    In [15]: dict1
    Out[15]: {'key1': 1, 'key3': True, 666: 888}
    ```

- 集合 set
一种类似于dict的数据类型, 只存储不排序的key, 具有数学中集合的某些性质(自动无视重复元素). 语法是 set1 = set(一个list) 

    ```python
    # 初始化一个set
    In [19]: set1 = set([1,2,3,3])

    In [20]: set1
    Out[20]: {1, 2, 3} # list中的重复元素无用
    # 在set中添加key
    In [21]: set1.add('key')

    In [22]: set1
    Out[22]: {1, 2, 3, 'key'}
    # 在set中移除元素
    In [24]: set1.remove(1)

    In [25]: set1
    Out[25]: {2, 3, 'key'}

    In [26]: set2 = set([1,2,3])
    # 多个set的并集运算
    In [27]: set1 & set2
    Out[27]: {2, 3}
    # 多个set的交集运算
    In [28]: set1 | set2
    Out[28]: {1, 2, 3, 'key'}
    ```
