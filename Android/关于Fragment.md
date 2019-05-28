## getFragmentManager与getSupportFragmentManager的区别？

因为3.0以下版本  是没有fragment的api  所以必须借助V4包里面的getSupportFragmentManager()方法来间接获取FragmentManager()对象。3.0版本之后，有了Fragment的api，就可以直接使用getFragmentManager()这个方法来获取了。

## getFragmentManager与getChildFragmentManager的区别？

getFragmentManager到的是activity对所包含fragment的Manager，而如果是fragment嵌套fragment，那么就需要利用getChildFragmentManager()了。

## 搞不明白的`commit()`、`commitAllowingStateLoss()`、`commitNow()`和`commitNowAllowingStateLoss()`？

在`onSaveInstanceState()`方法之前可以使用：commit和commitNow，如果需要同步执行用commitNow，如果在onSaveInstanceState方法之后调用了这两个方法就会抛出异常`java.lang.IllegalStateException: Can not perform this action after onSaveInstanceState`。如果要在`onSaveInstanceState()`之后提交可以使用：commitAllowingStateLoss()和commitNowAllowingStateLoss()，commitNowAllowingStateLoss用于同步提交。这里需要注意一点commitNow和commitNowAllowingStateLoss方法不会将要提交的fragment添加到回退栈中。



强调一点**每个事务（FragmentTranscation）只能被commit一次**


## 参考

- [Fragment进阶-commit使用细节及源码分析](https://www.jianshu.com/p/f50a1d7ab161)