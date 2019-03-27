# 用户管理

## 添加sudo
切换到root，使用visudo编辑/etc/sudoers，去掉wheel前的注释#。

``` shell
## Allows people in group wheel to run all commands
# %wheel  ALL=(ALL)       ALL
```

使用usermod将用户加入到wheel中。

``` shell
usermod -aG wheel username
```

# 系统安全

临时关闭selinux
``` shell
sudo setenforce 0
```
