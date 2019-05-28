# Activity启动模式&常用属性
Android 有两种设置启动模式的方式：**静态**和**动态**。
## 静态
**standard**：这是android默认的启动模式，即Activity每次都会创建一个新的实例。
**singleTop**：这种模式也叫栈顶复用，即将要被打开的Activity已经存在且处于栈顶，该模式下Activity不会创建新的Activity，会调用其实例的`protected void onNewIntent(Intent intent)`方法，其他情况与standard一致。
**singleTask**：栈内复用，即只要该Activity栈里已存在，那么就会复用。其处理方式分一下几种情况：
- 处于栈顶时，如果实例处于栈顶时，不再创建新的实例，而是回调已存在实例的`protected void onNewIntent(Intent intent)`方法。
- 未处于栈顶时，会溢出栈内该实例之上的所有实例，然后再调用实例的`protected void onNewIntent(Intent intent)`方法。
**singleInstance**：在该模式下，**我们会为目标Activity分配一个新的affinity，并创建一个新的Task栈，将目标Activity放入新的Task，并让目标Activity获得焦点。新的Task有且只有这一个Activity实例。如果已经创建过目标Activity实例，则不会创建新的Task，而是将以前创建过的Activity唤醒（对应Task设为Foreground状态）**。
## 动态（列举常见FLAG）
**Intent.FLAG_ACTIVITY_NEW_TASK**：根据affinity确定是否新建Task。
- 不设置affinity，则跟standard的情况一样
- 设置affinity
    - 如果affinity不存则创建新task，并在新的task中创建Activity实例
    - 如果affinity已经存在，刚直接把这个栈整体移动到前台，并保持栈中的状态不变，即栈中的activity顺序不变
**Intent.FLAG_ACTIVITY_SINGLE_TOP**：与singleTop情况一样。
**Intent.FLAG_ACTIVITY_CLEAR_TOP**：当设置此Flag时，目标Activity会检查Task中是否存在此实例，如果没有则添加压入栈，如果有，就将位于Task中的对应Activity其上的所有Activity弹出栈，此时有以下两种情况：
- 如果同时设置Flag_ACTIVITY_SINGLE_TOP，则直接使用栈内的对应Activity，
- 没有设置Flag_ACTIVITY_SINGLE_TOP，则将栈内的对应Activity销毁重新创建。
## 一些关于Activity的一些属性
### launchMode
用来静态设置Activity的启动模式。
### taskAffinity
taskAffinity属性应和FLAG_ACTIVITY_NEW_TASK标志及allowTaskReparenting属性结合使用，与FLAG_ACTIVITY_NEW_TASK的结合使用我们上面已经介绍了，与allowTaskReparenting的作用向下看。
### allowTaskReparenting
这个属性用于设定Activity能够从启动它的任务中转移到另一个与启动它的任务有亲缘关系的任务中，转移时机是在这个有亲缘关系的任务被带到前台的时候。如果设置了true，则能够转移，如果设置了false，则这个Activity必须要保留在启动它的那个任务中。它的默认值是false。
通常，当Activity被启动时，它会跟启动它的任务关联，并它的整个生命周期都会保持在那个任务中。但是当Activity的当前任务不在显示时，可以使用这个属性来强制Activity转移到与当前任务有亲缘关系的任务中。这种情况的典型应用是把应用程序的Activity转移到与这个应用程序相关联的主任务中。
例如，如果一个电子邮件消息中包含了一个网页的链接，点击这个链接会启动一个显示这个网页的Activity。但是，由e-mail任务部分启动的这个Activity是由浏览器应用程序定义的。如果把它放到浏览器的任务中，那么在浏览器下次启动到前台时，这个网页会被显示，并且在e-mail任务再次显示时，这个Activity有会消失。
Activity的亲缘关系是由taskAffinity属性定义的。通过读取任务的根Activity的亲缘关系来判断任务的亲缘关系。因此，通过定义，任务中的根Activity与任务有着相同的亲缘关系。因此带有singleTask或singleInstance启动模式的Activity只能是任务的根节点，Activity的任务归属受限于standard和singleTop模式。
> 经典理解：就是说，一个activity1原来属于task1，但是如果task2启动起来的话，activity1可能不再属于task1了，转而投奔task2去了。 当然前提条件是allowTaskReparenting，还有affinity设置 有点像，你捡到一条狗，在家里喂养几天觉得不错，当自己家的了；但是突然有一天他的主人找上门来了，小狗还是乖乖和主人走了。。。
### alwaysRetainTaskState
**这个属性用来标记应用的task是否保持原来的状态，“true”表示总是保持，“false”表示不能够保证，默认为“false”。**此属性只对task的根Activity起作用，其他的Activity都会被忽略。 默认情况下，如果一个应用在后台呆的太久例如30分钟，用户从主选单再次选择该应用时，系统就会对该应用的task进行清理，除了根Activity，其他Activity都会被清除出栈，但是如果在根Activity中设置了此属性之后，用户再次启动应用时，仍然可以看到上一次操作的界面。 这个属性对于一些应用非常有用，例如Browser应用程序，有很多状态，比如打开很多的tab，用户不想丢失这些状态，使用这个属性就极为恰当。 

### clearTaskOnLaunch
这个属性用来标记是否从task清除除根Activity之外的所有的Activity，“true”表示清除，“false”表示不清除，默认为“false”。同样，这个属性也只对根Activity起作用，其他的Activity都会被忽略。 如果设置了这个属性为“true”，每次用户重新启动这个应用时，都只会看到根Activity，task中的其他Activity都会被清除出栈。如果我们的应用中引用到了其他应用的Activity，这些Activity设置了allowTaskReparenting属性为“true”，则它们会被重新宿主到有共同affinity的task中。
**如果这个属性和allowTaskReparenting属性都被设置为true，那些被设置了亲缘关系的Activity会被转移到它们共享的亲缘任务中，然后把剩下的Activity都给删除。**

### finishOnTaskLaunch
这个属性和android:allowReparenting属性相似，不同之处在于allowReparenting属性是重新宿主到有共同affinity的task中，而finishOnTaskLaunch属性是销毁实例。

### configChanges
在Activity中添加了 android:configChanges属性，目的是当所指定属性(Configuration Changes)发生改变时，通知程序调用 onConfigurationChanged()函数。

最后，附上官网[链接](https://developer.android.com/guide/topics/manifest/activity-element)。