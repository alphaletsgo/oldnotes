# HelloWorld

```kotlin
fun main(args: Array<String>) {
    print("hello kotlin!")
}
```

**创建一个类**

```kotlin
package cn.isif.kotlin

class Person(val name: String){
    fun printName(){
        println("Name is :"+name)
    }
}
```
**在main方法中调用**
```kotlin
import cn.isif.kotlin.Person

fun main(args: Array<String>) {
    Person("levi").printName()
}
```
通过第一个helloworld我们可以看到kotlin还是比较有趣，语法比较现代化，期待后续的学习。

# 基础语法

## 变量
```kotlin
//var 定义变量 val定义常量
var quantity = 5 //自推断类型
val price: Double = 20.3
val name: String = "大米"

println("单价：$price")
println("数量：$quantity")
println("名称：$name 总价：${price * quantity}")
```
使用`var`定义变量，`val`来定义常量，自动类型推断并不实用所有情况，比如将int类型赋值给long类型，编译器会报错，需要我们显示的进行类型转换。如：
```kotlin
var a:Int = 3
//var l:Long = a 错误使用
var l:Long = a.toLong()
```
## 字符串与其模板表达式
原始字符串可以使用"""来分割表示，如：
```kotlin
    val rawString = """
    var a:Int = 3
    //var l:Long = a 错误使用
    var l:Long = a.toLong()
"""
    println(rawString)
```
字符串可以包含模板表达式。模板表达式以美元符号$开始:
```kotlin
println("单价：$price")
println("数量：$quantity")
println("名称：$name 总价：${price * quantity}")
```
## if表达式
```kotlin
    val a = 5
    var b = 6
    //1）作为表达式，kotlin中没有三元表达式，如果想要实现类似效果可以如下使用
    val max = if (a > b)a else b
    print(max)
    //2）传统用法 ...略
    //3）另外，if的分支可以是代码块，最后的表达式作为该块的返回值
    var max1 = if (a>b){
        "a>b"
    }else{
        "a<b"
    }
    print(max1)
```

## in语法
```kotlin
for (a in 1..5) {
    println(a)
}
if (5 in 1..10){
    println("ok")
}
```
`in`即可以用在`for`语句中又可以用在`if`句中，在`for`语句中表示循环取值，在`if`句中表示是否包含的关系。

## when
```kotlin
fun cases(obj: Any) {
    when (obj) {
        1,2 -> print("number is:$obj")
        "hello" -> print("string with:$obj")
        is String -> print("string with:$obj")
        is Double -> print("double with:$obj")
        in 10..20 -> print("")
        parseInt(obj) -> print("")//可以使用任意的表达式作为分支条件（不只是常量）
        else -> print("not cases!!!")
    }
}
```
`when`相当于`switch`语句。

## is
```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        //做过类型判断以后，obj会被系统自动转换为String类型
        return obj.length;
    }
    //可以使用!is来取反
    if (obj !is String) {

    }
    // 代码块外部的obj仍然是Any类型的引用
    return null
}
```
判断一个对象是不是某个类型的实例我们可以使用`is`，这个跟`instanceof`类似，不过在我们通过`is`断定为某个类型后的语句块中可以直接作为这个类型使用。

## label
```kotlin
//label相当于java中的label的概念
fun returnDemo_1(){
    println("START "+::returnDemo_1.name)
    val intArray = intArrayOf(1,2,3,4,5)
    intArray.forEach here@{
        if (it == 3)return@here
        println(it)
    }
    println(" END " + ::returnDemo_1.name)
}
//使用隐式标签--与lambda函数同名
fun returnDemon_2(){
    println(" START "+::returnDemon_2.name)
    val intArray = intArrayOf(1,2,3,4)
    intArray.forEach {
        if (it == 3)return@forEach//返回到@forEach 即lambda函数
        println(it)
    }
    println(" END " + ::returnDemon_2.name)
}
//使用break
fun breakDemo_1(){
    for (a in 1..5){
        here@for (b in 1..5){
            if (a==3)break@here
            println(a)
        }
    }
}
```

## throw
```kotlin
fun main(args: Array<String>) {
//    fail("hello")
    val ex:Nothing = throw Exception("h1") //如果要将一个throw表达式的值赋给一个变量，需要显示声明类型为Nothing
}

/**
* Nothing 相当于Java中的void 可以用来标识函数无返回值
* Nothing 没有任何值，所以无法当做参数传给函数
*
*/
fun fail(msg:String):Nothing{
    throw IllegalArgumentException(msg)
}
```

## this
```kotlin
class Hello{
    val thisis = "THIS IS"

    fun whatIsThis():Hello{
        println(this.thisis)
        this.howIsThis()
        return this
    }

    fun howIsThis(){
        println("How is This")
    }
}

fun thisDemo_1(){
    val sum = fun Int.(x: Int) = this + x//这里的this表示点左侧传递的接收者参数
    println(3.sum(10))
}

class Outer{
    val h = "oh"
    inner class Inner{
        fun h(){
            val outer = this@Outer
            val inner = this@Inner
            val pthis = this
            println(outer)
            println(inner)
            println(pthis)
        }
        val fun1 = hello@ fun String.(){
            val d1 = this
            println("d1"+d1)
        }
        fun test(){
            "hello".fun1()
        }
    }
}
```

## 空值检测
```kotlin
//当data不为空的时候执行
data?.let{
    //...
}
//为空判断
fun notNull(data: Person?):String {
    data?.let{
        print("ajsld")
    }
    //当?:左边非空返回左边的值，左边为空则返回右边的值
    data?:0
    return data?.getSomeThing()?:""
}
```
kotlin最主要的特色之一就是空指针安全。

## 函数
### 函数的声明
函数使用关键字fun声明，如下代码创建了一个名为say()的函数，它接受一个String类型的参数，并返回一个String类型的值
```kotlin
fun say(str: String): String {
    return str
}
```
同时，在 Kotlin 中，如果像这种简单的函数，可以简写为
```kotlin
fun say(str: String): String = str
```
如果是返回Int类型，那么你甚至连返回类型都可以不写
```kotlin
fun getIntValue(value: Int) = value
```
又比如：
```kotlin
fun main(args: Array<String>) {
    print(parseInt(4).invoke())
    print(parseInt1(5))
}
fun parseInt1(a: Int) = a//在kotlin中，可以直接使用=来直接返回一个函数的值
fun parseInt(a: Int) = {a}
```

### 函数的默认参数
你也可以使用默认参数来实现重载类似的功能
```kotlin
fun say(str: String = "hello"): String = str
```
这时候你可以调用say()，来得到默认的字符串 "hello"，也可以自己传入参数say("world")来得到传入参数值。

有时参数非常多的时候，也可以使用多行参数的写法，它们是相同的
```kotlin
fun say(firstName: String = "Tao",
        lastName: String = "Zhang"){
}
```
### 可变长参函数
同 Java 的变长参数一样，Kotlin 也支持变长参数
```java
//在Java中，我们这么表示一个变长函数
public boolean hasEmpty(String... strArray){
    for (String str : strArray){
        if ("".equals(str) || str == null)
            return true;
    }
    return false;
}
```
```kotlin
//在Kotlin中，使用关键字vararg来表示
fun hasEmpty(vararg strArray: String?): Boolean{
    for (str in strArray){
        if ("".equals(str) || str == null)
            return true    
    }
    return false
}
```
### 扩展函数
你可以给父类添加一个方法，这个方法将可以在所有子类中使用

例如，在 Android 开发中，我们常常使用这样的扩展函数：
```kotlin
fun Activity.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, duration).show()
}
```
这样，我们就可以在每一个Activity中直接使用toast()函数了。

### 扩展属性
在kotlin中除了能扩展函数之外我扩展属性
```kotlin
val String.hasChar: (Char) -> Boolean
    get() = { c: Char -> this.any { it == c } }
val String.leng: Int
    get() = this.length
val Person.age: Int
    get() = 10
fun main(args: Array<String>) {
    println("hello".hasChar('h'))
    println("what".leng)
    println(Person("Levi").age)
}
```

### 将函数作为参数
Kotlin 中，可以将一个函数作为参数传递给另一个函数
```kotlin
kotlin fun lock<T>(lock: Lock, body: () -> T ) : T { lock.lock() try { return body() } finally { lock.unlock() } } 
```
上面的代码中，我们传入了一个无参的 body() 作为 lock() 的参数

## 空指针安全
kotlin不能直接使用一个值为null的变量，如果想要使用为null的变量时必须使用`!!.`语句否则会报错，同样的再我们不知道一个变量是否为null时，如果我们要防止出现空指针可以使用`?.`：
```kotlin
//声明一个可以为null的变量
    var a: String? = "abc"
    println(a?.length)//安全调用
    a = null//只有可以为空的变量才能赋值为null
    println(a!!.length)//非安全调用
    a?.let {
        //当a!=null的时候执行
    }
    a!!.let {
        //当a=null的时候执行
    }
```