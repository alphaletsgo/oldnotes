# Centos安装nginx
**## 添加Nginx到YUM源**
添加CentOS 7 Nginx yum资源库,打开终端,使用以下命令:
```sh
sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
**## 安装Nginx**
在你的CentOS 7 服务器中使用yum命令从Nginx源服务器中获取来安装Nginx：
```shell
sudo yum install -y nginx
```
Nginx将完成安装在你的CentOS 7 服务器中。

**## 启动Nginx**
刚安装的Nginx不会自行启动。运行Nginx:

```shell
sudo systemctl start nginx.service
```
如果一切进展顺利的话，现在你可以通过你的域名或IP来访问你的Web页面来预览一下Nginx的默认页面；
**## 开机自动启动**
```shell
sudo systemctl enable nginx.service
```