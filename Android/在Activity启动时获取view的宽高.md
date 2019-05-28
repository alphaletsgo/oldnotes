由于view的测量和绘制并不是与Activity生命周期同步，所以我们无法保证在onStart、onCreate、onResume中正确获取view的宽高，如下有四种方案获取view的宽高：

* Activity/View#onWindowFocusChanged

onWindowFocusChanged这个方法的含义是：View已经初始化完毕了，宽_高已经准备好了，这个时候去获取宽_高是没问题的。需要注意的是，onWindowFocusChanged会被调用多次，当Activity的窗口得到焦点和失去焦点时均会被调用一次。具体来说，当Activity继续执行和暂停执行时，onWindowFocusChanged均会被调用，如果频繁地进行onResume和onPause，那么onWindowFocusChanged也会被频繁地调用。典型代码如下：
```java
public void onWindowFocusChanged(boolean hasFocus) {
        super.onWindowFocusChanged(hasFocus);
        if (hasFocus) {
                int width = view.getMeasuredWidth();
                int height = view.getMeasuredHeight();
        }
    }
```

* view.post(runnable)

通过post可以将一个runnable投递到消息队列的尾部，然后等待Looper调用此runnable的时候，View也已经初始化好了。典型代码如下：
```java
protected void onStart() {
        super.onStart();
        view.post(new Runnable() {
                @Override
                public void run() {
                        int width = view.getMeasuredWidth();
                        int height = view.getMeasuredHeight();
                }
        });
    }
```

* ViewTreeObserver

使用ViewTreeObserver的众多回调可以完成这个功能，比如使用OnGlobalLayoutListener这个接口，当View树的状态发生改变或者View树内 部的View的可见性发现改变时，onGlobalLayout方法将被回调，因此这是获取View的宽/高一个很好的时机。需要注意的是，伴随着View树的状态改变等，onGlobalLayout会被调用多次。典型代码如下：
```java
protected void onStart() {
        super.onStart();
        ViewTreeObserver observer = view.getViewTreeObserver();
        observer.addOnGlobalLayoutListener(new OnGlobalLayoutListener() {
                @SuppressWarnings("deprecation")
                @Override
                public void onGlobalLayout() {
                        view.getViewTreeObserver().removeGlobalOnLayoutListener(this);
                        int width = view.getMeasuredWidth();
                        int height = view.getMeasuredHeight();
                }
        });
    }
```

* view.measure(int widthMeasureSpec,int heightMeasureSpec)。

通过手动对View进行measure来得到View的宽/高。这种方法比较复杂，这里要分情况处理，根据View的LayoutParams来分：

**match_parent**

直接放弃，无法measure出具体的宽/高。原因很简单，根据View的measure过程，如表4-1所示，构造此种MeasureSpec需要知道parentSize，即父容器的剩余空间，而这个时候我们无法知道parentSize的大小，所以理论上不可能测量出View的大小。

**具体的数值（dp/px）**

比如宽/高都是100px，如下measure：
```java
int widthMeasureSpec = MeasureSpec.makeMeasureSpec(100,MeasureSpec.
    EXACTLY);
    int heightMeasureSpec = MeasureSpec.makeMeasureSpec(100,MeasureSpec.
    EXACTLY);
        view.measure(widthMeasureSpec,heightMeasureSpec);
```

**wrap_content**
```java
int widthMeasureSpec = MeasureSpec.makeMeasureSpec( (1 << 30) -1,
    MeasureSpec.AT_MOST);
    int heightMeasureSpec = MeasureSpec.makeMeasureSpec( (1 << 30) -1,
    MeasureSpec.AT_MOST);
        view.measure(widthMeasureSpec,heightMeasureSpec);
```
注意到(1 << 30)-1，通过分析MeasureSpec的实现可以知道，View的尺寸使用30位二进制表示，也就是说最大是30个1（即2^30 - 1），也就是(1 << 30) - 1，在最大化模式下，我们用View理论上能支持的最大值去构造MeasureSpec是合理的。
