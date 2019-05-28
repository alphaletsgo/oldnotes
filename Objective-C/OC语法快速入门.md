# OC语法快速入门

## 一、XCode、Objective-C、Cocoa说的是几样东西？
答案：三样东西。

**XCode**：你可以把它看成是一个开发环境，就好像Visual Studio或者Netbeans或者SharpDevelop一样的玩意。你可以将Interface Builder认为是Visual Studio中用来画界面的那部分功能单独提出来的程序。

**Objective-C**：这是一种语言，就好像c++是一种语言，Java是一种语言，c#是一种语言，莺歌历史也是一种语言一样。

**Cocoa**：是一大堆函数库，就好像MFC、.NET、Swing这类玩意，人家已经写好了一堆现成的东西，你只要知道怎么用就可以了。

有些人会比较容易混淆Objective-C和Cocoa，就好像有些人会混淆c#和.NET一样。这两个东西真的是两个不一样的东西。

## 二、Objective-C是什么？
你可以把它认为是语法稍稍有点不一样的c语言。虽然第一眼望上去你可能会认为它是火星语，和你所认知的任何一种语言都不一样。

先简单列出一点差别：
- 问题一：我在程序中看到大量的减号、中括号和NS****这种东西，他们是什么玩意儿？

### 1、减号（或者加号）
减号表示一个函数、或者方法、或者消息的开始，怎么说都行。
比如Java中，一个方法的写法可能是：

```Java
private void hello(boolean ishello)
{
//OOXX
}
```
用Objective-C写出来就是

```Objective-C
-(void) hello:(BOOL)ishello
{
//OOXX
}
```
挺好懂的吧？
不过在Objective-C里面没有public和private的概念，你可以认为全是public。
而用加号的意思就是其他函数可以直接调用这个类中的这个函数，而不用创建这个类的实例。

### 2、中括号
中括号可以认为是如何调用你刚才写的这个方法，通常在Objective-C里说“消息”。
比如java里你可以这么写：
```java
this.hello(true);
```
在Objective-C里，就要写成：
```Objective-C
[self hello:YES];
```
### 3、NS****
老乔当年被人挤兑出苹果，自立门户的时候做了个公司叫做NextStep，里面这一整套开发包很是让一些科学家们喜欢，而现在Mac OS用的就是NextStep这一套函数库。
这些开发NextStep的人们比较自恋地把函数库里面所有的类都用NextStep的缩写打头命名，也就是NS****了。比较常见的比如：
```Objective-C
NSLog
NSString
NSInteger
NSURL
NSImage
…
```
你会经常看到一些教学里面会用到：
```Objective-C
NSLog (@"%d",myInt);
```
这句话主要是在console里面跟踪使用，你会在console里面看到myInt的值（在XCode里面运行的时候打开dbg窗口即可看到）。而我们在其他开发环境里面可能会比较习惯使用MessageBox这种方式进行调试。
你还可以看到其他名字打头的一些类，比如CF、CA、CG、UI等等，比如
```Objective-C
CFStringTokenizer //这是个分词的东东
CALayer //这表示Core Animation的层
CGPoint //这表示一个点
UIImage //这表示iPhone里面的图片
```
CF说的是Core Foundation，CA说的是Core Animation，CG说的是Core Graphics，UI说的是iPhone的User Interface……还有很多别的，等你自己去发掘了。

- 问题二、#import、@interface这类玩意说的是什么？

### 1、#import
你可以把它认为是#include，一样的。但是最好用#import，记住这个就行了。
### 2、@interface等等
比如你在Java中写一个抓孩子类的定义：
```java
public class Kids extends System {
private string kidName="mykid";
private string kidAge="15";

private bool isCaughtKid()
    {
    return true;
    }

}
```
当然，上面的写法不一定对，就是个用于看语法的举例。
在Objective-C里就得这么写：
先写一个kids.h文件定义这个类：
```Objective-C
@interface Kids: NSObject {
	NSString *kidName;
	NSString *kidAge;
}
-(BOOL) isCaughtKid:;
@end
```
再写一个kids.m文件实现：
```Objective-C
#import “kids.h”

@implementation Kids
-(void) init {
    kidName=@”mykid”;
    kidAge=@”15”;
}

-(BOOL) isCaughtKid:{
return YES;
}

@end
```
这个写法也不一定对，主要是看看语法就行了。-_-b

- 问题三、一个方法如何传递多个参数？
一个方法可以包含多个参数，不过后面的参数都要写名字。
多个参数的写法：`(方法的数据类型) 函数名: (参数1数据类型) 参数1的数值的名字 参数2的名字: (参数2数据类型) 参数2值的名字 … ;`

举个例子，一个方法的定义：
```Objective-C
-(void) setKids: (NSString *)myOldestKidName secondKid: (NSString *) mySecondOldestKidName thirdKid: (NSString *) myThirdOldestKidName;
```
实现这个函数的时候：
```Objective-C
-(void) setKids: (NSString *)myOldestKidName secondKid: (NSString *) mySecondOldestKidName thirdKid: (NSString *) myThirdOldestKidName{
	大儿子 = myOldestKidName;
	二儿子 = mySecondOldestKidName;
	三儿子 = myThirdOldestKidName;
}
```
调用的时候：
```Objective-C
Kids *myKids = [[Kids alloc] init];
	[myKids setKids: @”张大力” secondKid: @”张二力” thirdKid: @”张小力”];
```
而如果你用Java写这个方法，大致的写法可能是
```java
public void setKids( string myOldestKidName, string mySecondOldestKidName, string myThirdOldestKidName)
{
    …
}
```
调用的时候大概的写法可能是：
```java
Kids myKids = new Kids();
myKids.setKids (“张大力”, “张二力”, “张小力”);
```
明白了吧？其实不怎么难看懂。
基本上，如果你能了解下面这段代码的转换关系，你Objective-C的语法也就懂了八成了：
```Objective-C
[[[MyClass alloc] init:[foo bar]] autorelease];
```
转换成Java的语法也就是：
```java
MyClass.alloc().init(foo.bar()).autorelease();
```
## 三、其他的一些东西
其实这些本站之前的文章有所提及，这里再详细解释一下。
### 1、 id：
Objective-C有一种比较特殊的数据类型是id。你可以把它理解为“随便”。
在Objective-C里，一切东西都是指针形式保存，你获取到的就是这个对象在内存的位置。那么id就是你知道这个位置，但是不知道里面是啥的时候的写法。

### 2、 同一个数组可以保存不同的对象：
比如一个数组NSArray，这种数组里面可以保存各种不同的对象，比如这个数组里：
```Objective-C
NSArray *array1 = [NSArray arrayWithObject @"iphone", (float) 234.33f, (NSImage *) img,nil ];
```
这是一个由4个东西组成的数组，这个数组包括一个浮点数，两个字符串和一个图片。

### 3、BOOL，YES，NO：
你可以认为YES表示C#或者Java里的true，NO表示false。而实际上YES是1，NO是0，BOOL本身就是个char。

### 4、IBOutlet、IBAction是啥玩意，总能看到
这两个东西其实在语法中没有太大的作用。如果你希望在Interface Builder中能看到这个控件对象，那么在定义的时候前面加上IBOutlet，在IB里就能看到这个对象的outlet，如果你希望在Interface Builder里控制某个对象执行某些动作，就在方法前面加上(IBAction)。
而这两个东西实际上和void是一样的。

### 5、nil
Objective-C里的NULL（空）就这么写，表示空指针。

### 6、为什么是@”字符串”而不是”字符串”
”字符串”是C的字符串,@””是把C的字符串转成NSString的一个简写.
在需要NSString的地方才需要这个转化,例如NSLog里面.
在需要C string的地方,还是用”字符串”的.

## 四、Objective-C 2.0
Objective-C 2.0是Leopard新增加的一门语言，其实和原来的Objective-C是一样的。主要是增加了属性。

## 五、总结
- 现在来总结一下怎么看Objective-C的代码和怎么开始学Objective-C吧。
- 记住Objective-C就是C，不是火星语，这个很关键。
- 记住你自己看不懂不表示脑子迟钝，大部分人第一次看Objective-C的代码可能比你还要迟钝。
- 看不明白代码就来再看一遍这篇开宗明义的好文。
- 文档很关键，当你看不懂某些东西说的是什么的时候，先查Cocoachina，再看英文文档里面的API说明，尤其这个类是以NS开头的时候。再不行就去google搜，直接把你要查的方法贴进google，通常能找到不少人也在问同样的问题，自然也有热心人活雷锋帮助回答。
- 可以看hello world例子，但是不能总看，看多了真的会晕。另外，千万要放弃苹果官方的Currency Converter货币转换的例子，那个例子是毒药，刚学的时候越看越蒙。
- 学习一门语言最好的方法是先用，和学外语一样，当你会说的时候自然会读。给自己设立一个简单的目标，比如做一个简单的程序，然后一点点解决问题。这样学习起来比只看例子快得多。