# new_note
一、 iSCSI 
又称为IP-SAN，是一种基于因特网及SCSI-3协议下的存储技术,iSCSI利用了TCP/IP的port 860 和 3260 作为沟通的渠道。透过两部计算机之间利用iSCSI的协议来交换SCSI命令，让计算机可以透过高速的局域网集线来把SAN模拟成为本地的储存装置。（可路由）
用途：存储集成，灾难恢复
SCSI总线通过SCSI控制器来和硬盘之类的设备进行通信, SCSI控制器称为Target，访问的客户端应用称为Initiator。
iSCSI Initiator，iSCSI启动器（客户端设备）用于请求连接并启动到服务器 Target
iSCSI Target，服务器组件
工作过程：
![工作过程](_v_images/20200314183935808_13996.png)
![工作过程](_v_images/20200314184000668_18307.png)

FC 与 iSCSI 的工作原理差别：
![FC 与 iSCSI 的工作原理差别](_v_images/20200314184018528_32328.png)

SAN与NAS / iSCSI与NFS
![SAN与NAS / iSCSI与NFS](_v_images/20200314184033588_9930.png)
2、WWID, UUID
根据SCSI标准，每个SCSI磁盘都有一个WWID，类似于网卡的MAC地址，要求是独一无二。 通过WWID标示SCSI磁盘就可以保证磁盘路径永久不变，Linux系统上/dev/disk/by-id目录包含每个SCSI磁盘WWID访问路径。 
查看磁盘设备wwid方法1： ll /dev/disk/by-id/ total 0 lrwxrwxrwx. 
查看磁盘设备wwid方法2：scsi_id --whitelist 
磁盘标识符与WWID绑定：

UUID ：通用唯一识别码。重要用途包括 ext2/ext3/ext4 文件系统用户空间工具（e2fsprogs 使用 util-linux 提供的 libuuid），LUKS 加密分区，GNOME，KDE 和 Mac OS X，其中大部分源自 曹子德(Theodore Ts'o) 的实现。唯一地址，保证路径不变
Solaris 中 UUID 的一种用途（使用Open Software Foundation实现）是识别正在运行的操作系统实例，以便在内核崩溃的情况下将故障转储数据与故障管理事件配对。
查看UUID ： ll /dev/disk/by-uuid/ total 0 lrwxrwxrwx
与文件系统绑定：

三、CHAP 询问握手协议
是一个用来验证用户或网络提供者的协议。负责提供验证服务的机构，可以是互联网服务供应商，又或是其他的验证机构。通过三次握手周期性的校验对端的身份，可在初始链路建立时完成时，在链路建立之后重复进行。





