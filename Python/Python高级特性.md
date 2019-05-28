## 切片
```python
shopList = ['apple', 'mango', 'carrot', 'banana']
str = "string"
# 索引或下标操作
print(shopList[0])
print(shopList[1])
print(shopList[-1])     # 取列表中的倒数第一个元素
print(shopList[3-2])
# 列表切片操作
print(shopList[1:3])
print(shopList[2:])
print(shopList[1:-1])
print(shopList[:])
# 字符串切片操作
print(str[1:3])
print(str[2:])
print(str[1:-1])
print(str[:])
```
## 迭代
`for..in`是一个强大的迭代语句。
## 列表生成式
列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来创建list的生成式。
```python:n
import os
# 创建一个list
li = list(range(1, 10))
# 生成一个[1x1, 2x2, 3x3, ..., 9x9]的list
li2 = [x * x for x in li]
print(li2)
# 我们还可以加速条件
li3 = [x * x for x in li if x % 2 == 0]  # 这里我们筛选出偶数的平方
print(li3)
# 还可以使用双层for循环
li4 = [m + n for m in "ABC" for n in "XYZ"]
print(li4)
# 列出当前目录下所有的文件和目录名
li5 = [d for d in os.listdir('.')]
print(li5)
# 练习题
ll = ['Hello', 'World', 18, 'Apple', 'None']
print([x.lower() for x in ll if isinstance(x,str)])
```
## 生成器
生成器是迭代器。
要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：
```python
g = (x * x for x in range(10))
```
使用next(g)依次生成每一个元素
```python
for i in range(10):
     print(next(g))
```
当然我们更多的是如下使用
```python
for i in g:
    print(i)
```
斐波拉契数列 1，1，2，3，5，8...
```python
def fib(max):
    a, b, n = 1, 0, 0
    while n < max:
        print(a)
        b, a = a, a + b
        n = n + 1
    return 'done'
fib(12)
```
将`fib(max)`函数变成一个generator，只需要将`print(a)`改为`yield a`就可以了
```python
def fib(max):
    a,b,n = 1,0,0
    while a < max:
        yield a
        b,a = a, a+b
        n = n =1
    return "done"
f = fib(6)
for i in f:
    print(i)
```
最难理解的就是generator和函数的执行流程不一样。函数是顺序执行，遇到return语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。
## 迭代器
```python
from collections import Iterable

# 判断一个对象是否Iterable对象
print(isinstance("", Iterable))  # 执行结果是True
print(isinstance([], Iterable))  # 执行结果是True
print(isinstance((x for x in range(10)), Iterable))  # 执行结果是True
print(isinstance({}, Iterable))  # 执行结果是True
print(isinstance(100, Iterable))  # 执行结果是False
```
为什么list、dict、str等数据类型不是Iterator？
这是因为Python的Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。
Iterator甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的。
