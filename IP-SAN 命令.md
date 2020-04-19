#### IP-SAN 命令
##### iscsiadm
```shell
## 获取initiaorname
# cat /etc/iscsi/initiatorname.iscsi
## 发现target，信息保存在 /var/lib/iscsi/nodes/
# iscsiadm -m discovery -t st -p IP
## 登录某一个target
# iscsiadm -d2 -m node -T iqn.1994-05.com.redhat:wsfnk -p 192.168.1.55 --login
## 登出某一个target
# iscsiadm -d2 -m node -T iqn.1994-05.com.redhat:wsfnk -p 192.168.1.55 --logout
# iscsiadm -m node -T iqn.1994-05.com.redhat:wsfnk -u
## 登录/退出 所有Target
# iscsiadm -m node -L all
# iscsiadm -m node -U all
## 查看当前会话
# iscsiadm -m session
## 查看Target
# iscsiadm -m node
## 删除某一个Target
# iscsiadm -m node -o delete -T iqn.1994-05.com.redhat:wsfnk -p 192.168.0.4:3260
## 刷新，如果某个Target挂载了新资源
# iscsiadm –m session –R
## 查看通过iscsi连接过来的卷
# cat /proc/scsi/scsi
## 设置开机自动挂载SAN资源
# systemctl enable iscsid.service
# iscsiadm -m node –T iqn.1994-05.com.redhat:scst1 -p 192.168.1.40 -o update -n node.startup -v automatic
# vim /etc/fstab

####### 设置CHAP认证
## 开启认证
iscsiadm -m node -T iqn.1994-05.com.redhat:wsfnk -p 192.168.1.55 -o update --name node.session.auth.authmethod --value=CHAP
## 添加yonghu
iscsiadm -m node -T iqn.1994-05.com.redhat:wsfnk -p 192.168.1.55 -o update --name node.session.auth.username --value=yonghu
## 设置密码
iscsiadm –m node –T iqn.1994-05.com.redhat:wsfnk -p 192.168.1.55 -o update --name node.session.auth.password --value=yonghu-password
```
##### iscsi设置
连接参数，超时参数啥的
```shell
# yum install iscsi-initiator-utils
# cat /etc/iscsi/initiatorname.iscsi
# grep "Attached SCSI" /var/log/messages

# iscsiadm -m node -P 0
```
##### Multipath
```shell
# modprobe dm-multipath

# cp /usr/share/doc/device-mapper-multipath-0.4.9/multipath.conf /etc/multipath.conf
# /sbin/mpathconf --enable

# systemctl restart multipathd
# 路径没有合并，使用下面命令
# multipathd -k"reconfigure"

```


##### Multipath设置
多路径配置等等













##### 扫描新增磁盘（或扩容磁盘）




