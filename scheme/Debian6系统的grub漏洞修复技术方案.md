# Debian6系统的grub漏洞修复技术方案

grub2是一个来自GNU项目的启动引导程序，与公司产品相互独立，他们之间不影响。


## 验证漏洞  

启动无线网关3.0.0，进入grub的编辑模式，输入用户名之后输入密码，在输入密码处输入至少28个backspace键，Enter之后系统重启，证明系统存在漏洞。


## 漏洞的修复

```
--- a/grub-core/lib/crypto.c 
+++ b/grub-core/lib/crypto.c 
@@ -468,7 +468,7 @@ grub_password_get (char buf[], unsigned buf_siz
e) 
      break; 
    } 

- if (key == '\b') 
+ if (key == '\b' && cur_len) 
    { 
      cur_len--; 
      continue; 
  diff --git a/grub-core/normal/auth.c b/grub-core/normal/auth.c 
  index c6bd96e..5782ec5 100644 
  --- a/grub-core/normal/auth.c 
  +++ b/grub-core/normal/auth.c 
  @@ -172,7 +172,7 @@ grub_username_get (char buf[], unsigned buf_siz
  e) 
      break; 
    } 

- if (key == '\b') 
+ if (key == '\b' && cur_len) 
    { 
      cur_len--; 
      grub_printf ("\b"); 
  --  
```


## 补丁包修复方法

只需要更新 /boot/grub/crypto.mod 文件。

1 .下载源码：安装更新的Deb包，官网查询与[漏洞相关的包](http://ftp.cn.debian.org/debian/pool/main/g/grub2/grub­pc_1.98+20100804­14+squeeze2_amd64.deb)，解压安装包内容到 grubpc 的文件夹中。

2 .通过shell脚本获取 ./grubpc/usr/lib/grub/i386­pc目录下与 grub_username_get()和grub_password_get() 函数相关的模块。

```
#!/bin/sh
mods=($(ls ./ | grep '.mod')) 

for mod in ${mods[@]}; do 

    username=($(nm ${mod} | grep grub_username_get)); 
    password=($(nm ${mod} | grep grub_password_get)); 
    if [[ -n ${username} || -n ${password} ]]; then 
        echo "${mod}"; 
        echo "${username}"; 
        echo "${password}"; 
    fi; 
done; 
```

3 .安装更新后的Deb包，用find命令知道/boot/grub/中的crypto.mod被更新，影响开机的选项的文件也是放在/boot/grub/文件夹下面，所以只要将更新后的 crypto.mod 模块替代 W系列网关，V系列网关，无线网关3.0.0中/boot/grub/下相应的模块。因为只更新了一个模块，所以不需要更新grub.cfg文件。

4 .发布形态  




