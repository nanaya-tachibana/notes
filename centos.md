# 防火墙

重启firewall
``` shell
firewall-cmd --reload
```

停止firewall
``` shell
systemctl stop firewalld.service
```

禁止firewall开机启动
``` shell
systemctl disable firewalld.service
```
