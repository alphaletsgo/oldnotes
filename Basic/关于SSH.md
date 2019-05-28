# 关于SSH
ssh基本上是linux的标配，通常我们购买的linux vps主机都采用ssh登录，一般我们都是使用密码登录，今天我们就来了解一下如何使用免密码登录。
**首先，本地生成一对私钥文件**
```shell
ssh-keygen 
#该命令等价于 ssh-keygen -t rsa     -t:指定秘钥类型，默认为SSH-2的RSA
```
运行上面的命令后，系统会出现一系列提示，可以一路回车。特别说明，其中有一个问题是，要不要对私钥设置口令（passphrase），如果担心私钥的安全，可以设置一个。运行结束以后，会在` ~/.ssh/ `目录下新生成两个文件：id_rsa.pub和id_rsa。前者公钥，后者是私钥。
**然后，将公钥传送到远程主机上**
```shell
ssh-copy-id user@host
```
经过以上两步之后，就可以实现无密码远程登录了(远程主机将用户的公钥保存在`~/.ssh/authorized_keys`文件中)。 

## 参考
[Linux命令之远程登录/无密码登录](~https://blog.csdn.net/wangjunjun2008/article/details/20037101~)