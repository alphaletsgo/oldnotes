# Centos 7.0 部署Shadowsocks服务
## 安装ShadowSocks服务器
```Terminal
# yum update
# yum install python-setuptools && easy_install pip
# pip install shadowsocks
```

## 配置shadowsocks.json
```Terminal
# vim /etc/shadowsocks.json
```
根据如下模板进行修改：
```json
{
"server":"your_server_ip",#服务器地址
"server_port":8888,
"password":"yourpassword",
"timeout":300,
"method":"aes-256-cfb",
"fast_open":false,
"workers": 1
}
```
## 启动Shadowsocks
```sh
# ssserver -c /etc/shadowsocks.json -d start
```
## 开机启动
```sh
 # vim  /etc/rc.d/rc.local
```
加入下面内容
```sh
/usr/bin/ssserver -c /etc/shadowsocks.json -d start
```
保存并推出 vim, CentOS 7正打算抛弃/etc/rc.d/rc.local，重启前需要运行以下命令获得权限，否则rc.local不会执行     
```sh
# chmod +x /etc/rc.d/rc.local
```
