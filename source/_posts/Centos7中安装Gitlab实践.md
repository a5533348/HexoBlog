---
title: Centos7中安装Gitlab实践
date: 2018-06-06 11:17:49
tags: Gitlab
categories: Linux
---

#### 介绍
Gitlab 是一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，可通过Web界面进行访问公开的或者私人项目，非常适合在团队内部使用。
在gitlab中有三个版本，分别是CE（社区版）、EE（企业版）、OM（RPM包完整版），这里安装的是CE版。

使用的服务器环境是阿里云的Centos7，1核2G。

#### 开启SWAP

由于Gitlab运行需要较多的内存，官方推荐的服务器2核4G，但是这个配置在阿里云上是在是太贵了，一年要1-2k，实在是负担不起，因此我们可以通过开启SWAP来缓解内存的压力。

如果不开启SWAP的话，2G内存很难把Gitlab跑起来，而且在安装过程中会出现各种问题，卡机。


##### 什么是SWAP呢？

Swap分区在系统的物理内存不够用的时候，把硬盘中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap分区中，等到那些程序要运行时，再从Swap分区中恢复保存的数据到内存中。

##### SWAP设置

1、创建用于交换分区的文件：

```
dd if=/dev/zero of=/mnt/swap bs=block_size count=number_of_block  
```
注：block_size、number_of_block 大小可以自定义，比如 bs=1M count=1024 代表设置 1G 大小 SWAP 分区。

这里推荐分配4G的内存：

```
dd if=/dev/zero of=/mnt/swap bs=1M count=4096 
```

2、设置交换分区文件：

```
mkswap /mnt/swap
```

3、立即启用交换分区文件

```
swapoff /mnt/swap
swapon /mnt/swap
```
为了避免出现设备或资源占用的错误，建议先关闭，后打开

```
注：如果在 /etc/rc.local 中有 swapoff -a 需要修改为 swapon -a 
```

4、设置开机时自启用 SWAP 分区：  

需要修改文件 /etc/fstab 中的 SWAP 行，在文件的最后一行添加

```
/mnt/swap swap swap defaults 0 0
```

5、修改 swpapiness 参数

在 Linux 系统中，可以通过查看 /proc/sys/vm/swappiness 内容的值来确定系统对 SWAP 分区的使用原则。当 swappiness 内容的值为 0 时，表示最大限度地使用物理内存，物理内存使用完毕后，才会使用 SWAP 分区。当 swappiness 内容的值为 100 时，表示积极地使用 SWAP 分区，并且把内存中的数据及时地置换到 SWAP 分区。

这里推荐设置为50，当物理内存使用大于50%时尽可能得使用Swap内存，设置方法如下：

临时修改：

```
echo 10 >/proc/sys/vm/swappiness
```

永久修改（推荐）：
	
```
# vim /etc/sysctl.conf
vm.swappiness=10
# sysctl -p
```

这样SWAP分区就设置好了，输入free -m查看：

```
free -m
```

如果想要关闭的话使用：

```
swapoff /mnt/swap  
```
然后将以上的设置去掉就可以了。

#### 安装Gitlab

这里按照官网的安装指引

1、安装和配置必要的依赖

```
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo systemctl start firewalld.service
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```
2、添加Gitlab的repository

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
```

3、安装

```
sudo EXTERNAL_URL="http://gitlab.example.com" yum install -y gitlab-ee
```
这里的EXTERNAL_URL指定成访问的url，没有域名的可以指定IP地址。

安装的时候会一直保持在同一个界面，不要怀疑它是不是卡了，静候5分钟左右，出现安装成功的提示。

#### Gitlab使用
安装成功后就可以打开上述指定的EXTERNAL_URL打开Gitlab登录了，第一次打会让你设置管理员root的密码。接下来就可以做：

  * 创建用户
  * 创建组，将相关用户分配到该组
  * 创建项目

#### 邮件配置
邮件也是Gitlab的一个重要的功能，比如新建用户后系统会发送一封激活邮件给用户，当项目有更新的时候也会发一封提醒邮件。

打开gitlab配置文件:

```
vi /etc/gitlab/gitlab.rb
```
下面以腾讯企业邮箱为例子，修改以下参数：

```
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.exmail.qq.com"
gitlab_rails['smtp_port'] = 587
gitlab_rails['smtp_user_name'] = "xxxx@xx.com"
gitlab_rails['smtp_password'] = "password"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = true
gitlab_rails['gitlab_email_from'] = 'xxxx@xx.com'
gitlab_rails['smtp_domain'] = "exmail.qq.com"
```
保存后运行:

```
gitlab-ctl reconfigure
```
其他邮箱可以参考官方给出的例子[SMTP SETTING](https://docs.gitlab.com/omnibus/settings/smtp.html#qq-exmail)

#### 引用

[云服务器 ECS Linux SWAP 配置概要说明](https://help.aliyun.com/knowledge_detail/42534.html)

[Gitlab Installer](https://about.gitlab.com/installation/#centos-7)