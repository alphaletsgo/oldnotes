> python 中内置了四种数据结构——列表（List）、元组（Tuple）、字典（Dictionary）和集合（Set）
 
## 列表List
```python
# This is mu shopping list
shopList = ['apple', 'mango', 'carrot', 'banana']
print(len(shopList))    # 计算列表长度
shopList.append('rice')     # 添加
print(shopList)
shopList.sort()     # 排序
print(shopList)
print(shopList[0])      # 访问第0个位置上的元素
del shopList[0]     # 删除第0个位置上的元素
print(shopList)
```
## 元组
```python
# 元组是不可改变的 不能编辑、删除、添加
zoo = ('python', 'elephant', 'penguin')
print(zoo)
new_zoo = 'monkey', 'camel', zoo
print(new_zoo)
print(new_zoo[2][2])
```
> 包含 0 或 1 个项目的元组
一个空的元组由一对圆括号构成，就像 myempty = () 这样。然而，一个只拥有一个项目的元组并不像这样简单。你必须在第一个（也是唯一一个）项目的后面加上一个逗号来指定它，如此一来 Python 才可以识别出在这个表达式想表达的究竟是一个元组还是只是一个被括号所环绕的对象，也就是说，如果你想指定一个包含项目 2 的元组，你必须指定 singleton = (2, ) 。
 
## 字典
```python
ab = {
    'Swaroop':'swaroop@isif.cn',
    'Larray':'larray@isif.cn',
    'Levi':'levi@isif.cn',
    'Spammer':'spammer@isif.cn'
}
print(ab)
ab_pro = {
    'add':ab
}
print(ab_pro)
del ab['Spammer']   # 删除对应key的项
print(ab)
# 遍历
for name, address in ab.items():
    print(name, '-', address)
# 添加一对
ab['new'] = 'newValue'
print(ab)
# 判断是否存在
if 'new' in ab:
    print("new : ", ab['new'])
```
## 集合
集合（Set）是一个无序不重复元素集，基本功能包括关系测试和消除重复元素。集合对象还支持union(联合)， intersection(交)，difference(差)和sysmmetric difference(对称差集)等数学运算。
sets 支持 x in set, len(set),和 for x in set。作为一个无序的集合，sets不记录元素位置或者插入点。因此，sets不支持 indexing, slicing, 或其它类序列（sequence-like）的操作。
```python
x = set("spam")
y = set(['h', 'a', 'm'])
 
print(x & y)  # 交集
print(x | y)  # 并集
print(x - y)  # 差集
```
 
## 序列
列表、元组和字符串可以看着是序列的某种表现形式。序列的主要功能是资格测试（Membership Test）（也就是 in 与 not in 表达式）和索引操作（Indexing Operations）。
上面所提到的序列的三种形态——列表、元组与字符串，同样拥有一种切片（Slicing）运算符，它能够允许我们序列中的某段切片——也就是序列之中的一部分。
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
### 列表生成式
```python
import os

# 创建一个list
li = list(range(1, 10))
print(li)
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
 
 