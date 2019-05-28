# C++操作符
### “.”和“->”
箭头`->`：左边必须为指针
点号`.`：左边必须为实体
### 单冒号“:”
#### 1、表示继承关系
```c++
class A : public B {};
```
####  2、访问控制
```c++
class A {
private:
    int member;
public:
    int getMember(){
        return member;
    }
};
```
#### 3、列表初始化
```c++
class A {
private:
    int member;
public:
    A () : member(0) {}
    int getMember(){
        return member;
    }
};
```
#### 4、位域（即变量占几个bit的空间）
```c++
class A {
public:
    unsigned int first : 1;//表示int类型first的占位
    unsigned int second : 2;
};
```
### 双冒号“::”
#### 1、表示“域操作符”
例：声明了一个类A，类A里声明了一个成员函数`void f()`，但没有在类的声明里给出f的定义，那么在类外定义f时， 就要写成`void A::f()`，表示这个f()函数是类A的成员函数。
#### 2、直接用在全局函数前，表示是全局函数
例：在VC里，你可以在调用API 函数里，在API函数名前加 ::
#### 3、表示引用成员函数及变量，作用域成员运算符
例：`System::Math::Sqrt() `相当于`System.Math.Sqrt()`
