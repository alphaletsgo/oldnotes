### 注释
```python
print("hello world")#这里是注释部分
```
### 三引号
使用`""""`或`'''`可以用来定义多行字符串
```python
'''
hello world!
hello python!
'''
```
### 格式化函数 `format()`
```python
age = 20
name = "Levi"
print("{0} age is {1}".format(name, age))
```
不过如果我们要达到如上目的我们还可这样：
```python
print(name + " age is " + age) #
```
format函数除了能格式化模板外，还可以
```python
print('{0:.3f}'.format(1.0 / 3))  # 对于浮点数保留(.)后三位
# 使用下划线填充文本，并保持文本处于中间位置
# 使用（^）定义'___hello___'字符串长度为11
print('{0:_^11}'.format('hello'))
# 基于关键词输出'Swaroop wrote A Byte of Python'
print('{name} wrote {book}'.format(name='Swaroop', book="A Byte of Python"))
```
### 条件控制
#### if
```python
number = 23
guess = int(input("Enter an integer : "))
if guess == number:
    print('{0} = {1}'.format(guess, number))
elif guess < number:
    print('{0} < {1}'.format(guess, number))
else:
    print('{0} > {1}'.format(guess, number))
print("done")
```
#### while
```python
number = 23
running = True
while running:
    guess = int(input("Enter an integer : "))
    if guess == number:
        running = False
    else:
        print("again")
else:
    print('while 同样可以拥有else子句'
```
#### for...in
```python
for i in range(1,5):
    print(i)
else:
    print('The for loop is over')
```
### 函数
#### 无参函数
```python
# 无参函数
def say_hello():
    #函数体
    print('hello world')
```
#### 带参的函数
```python
def print_max(a=1, b=2):
    if a > b:
        print(a, 'is maximum')
    elif a == b:
        print(a, " is equal to ", b)
    else:
        print(b, "is maximum")
```
```python
def func(a, b=5, c=10):
    print("a is ", a, "and b is ", b, "and c is ", c)
 
func(3, 7)  # a is  3 and b is  7 and c is  10
func(25, c=25)   # a is  25 and b is  5 and c is  25
func(c=50, a=100)   # a is  100 and b is  5 and c is  50
```
> 只有那些位于参数列表末尾的参数才能被赋予默认参数值，意即在函数的参数列表中拥有默认参数值的参数不能位于没有默认参数值的参数之前。
这是因为值是按参数所处的位置依次分配的。举例来说，`def func(a, b=5)`是有效的，但 `def func(a=5, b)`是无效的。
 
#### 可变参数
`*param`表示从此处开始到结束的所有位置参数都将被汇集成为一个"param"的元组。
`**param`表示从此处开始直到结束的所有参数都将被汇集成一个"param"的字典。
```python
def total(a=5, *numbers, **phonebook):
    print("a", a)
    # 遍历元组中的数据
    for item in numbers:
        print(item)
    # 遍历字典中的数据
    for first, second in phonebook.items():
        print(first, second)
```
更多函数内容参考：[函数的参数 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431752945034eb82ac80a3e64b9bb4929b16eeed1eb9000)

### 模块
- import 导入指定模块，`import sys`导入sys模块；
- `form..import`导入模块中的某一个函数或者属性等等，一般不常用；