今天突发奇想，想在mac的终端下查询单词，最终想达到如下效果：
```shell
yd [word]
```
为了达到我们想要的效果，于是就Google了一下，居然找到了一个w3m这样的工具，这个工具是日本东北大学开发并开源的项目，可以在终端浏览网页。

首先我们先安装`w3m`工具
```shell
brew install w3m
```
然后我们再编写一个`yd`的指令脚本
```shell
touch yd
chmod +x yd
ln -s /yd /usr/local/bin/yd
```
在脚本文件里面添加如下内容：
```shell
#! /bin/sh
if [ -z "$1" ]
then
    echo 'Usage yd <word>'
else
    w3m http://dict.youdao.com/search?q=$1
fi
```
好了，到此我们就完成了在终端中查询单词的功能。