---
title: 菜鸟学Python(2) - 基本I/O与流程控制 
date: 2016-08-13 18:13:33
tags: [python]
categories:
    - learn coding
    - python
---

继续学习Python, 自娱自乐. 本文介绍了如何利用Python实现最基本的输入/输出以及条件判断, 循环等流程控制.
<!-- more -->

> \# 所有的测试在python3.5.2中进行, 并使用IPython shell


### 基本I/O
#### 简单的输入(input)和输出(print)
- print()
print() 函数在括号中加上字符串就可以将字符串输出到屏幕.
```python
In [1]: print("Too young", "too simple !") # 括号中可以接多个字符串, 之间用逗号隔开, python遇到一个逗号就会替换为一个空格输出.
Too young too simple !

In [2]: print("Too young", "too simple !", 666) # 可以同时输出不同的数据类型.
Too young too simple ! 666

In [3]: print(3>5) # 计算式会先被计算后输出结果
False
```

- input()
input()可以让我们用键盘输入一个字符串并付给指定变量, 这时shell会等着用户输入, 当用户输入后按下回车程序才能向下运行.
```python
In [4]: name = input()
Thorough

In [5]: name
Out[5]: 'Thorough'
```

### 流程控制
#### 条件判断
语法格式为:
```python
if <条件判别式1>:
    <执行1>
    # 如果没有else部分, 到此结束即可
elif <条件判别式2>: # elif是else if的缩写
    <执行2>
elif <条件判别式3>:
    <执行3>
else:
    <执行4>
```
**注意** Python解释器是用缩进判断执行代码块范围的, 因此一定要注意缩进的范围. 还有就是":" 不要忘记.
```python
# coding: utf-8
# 提示输入年龄并判断年龄段
print("Please input your age:")
age = input()
age = int(age) # int()函数将字符串转换为整数
if age >= 18: 
    print("adult")
elif age >= 6:
    print('teenager')
else:
    print('kid')
```
#### 循环
- for...in 循环

    语法格式为:
    ```python
    # for...in循环会将var中的每一个元素带入loopvar然后执行循环体
    var = a list or a tuple 
    for loopvar in var:
        <执行>
    ```
    for...in循环会将var中的每一个元素带入loopvar然后执行循环体, 举个例子
``` python
# coding: utf-8
# 1到100的求和
sum = 0
for x in range(101): # range(int)输出一个从0到小于int的整数组成的list
    sum = sum + x
print(sum)
```

- while 循环
语法格式为:
    
    ```python
    while <条件判别式>:
        <执行>
    ```
    只要条件判别式为True, 就执行循环体, 将上一个例子用while循环再写一遍
```python
# coding: utf-8

sum = 0
n = 1
while n <= 100:
    sum = sum + n
    n = n+1
print(sum)
```
