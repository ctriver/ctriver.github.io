# SSS_PTS_Enterprise_v1.1
![测试类型](_v_images/20200324232727274_22209.png)
![图2](_v_images/20200324232818471_25891.png)
3个SNIA基本测试 - IOPS, TP(Throughput Test)带宽, LAT(Latency)延迟

饱和性测试 - WSAT(Write Saturation)写入饱和

优化性测试 - DIRTH(Demand Intensity/Response Time Histogram)需求强度/响应时间直方图

2个持续性测试 - XSR(Cross Stimulus Recovery)随机/持续和块大小交叉负载恢复, HIR(Host Idle Recovery)主机空闲恢复

## 测试背景
SSD典型的性能过程，开始使用（FOB，State of SSS prior to being put into service.）、加压工作负载、短暂的性能上升、逐渐过渡到稳定性能。SSS PTS确保测试过程在性能稳定的阶段，代表SSD真实的工作寿命期间的性能。
![NAND-based SSS Performance States for 8 Devices (RND 4KiB Writes)](_v_images/20200324162920297_25936.png)
## 范围
- 预处理方法
- 性能测试
- 测试报告的要求

## 测试过程关键概念


## 测试流程
SSS PTS （IOPS、Throughput、Latency）稳态测试都适用于下面的流程：

### IOPS
#### 测试流程
- 对100%的LBA进行预处理
- 进行WIPC，采用 128KiB SEQ W 对2倍的用户容量
- 进行WDPC，采用IOPS 循环模式，知道达到稳定状态
- 正式测试
#### IOPS测试伪代码
对于全部LBA：
1. 清除设备
2. 进行WIPC
    - 设置测试条件：关闭缓存、OIO/Thread、Thread Count、Data Pattern（随机）（相当于写入的数据每次都是随机的，而不是随机写入）
    - 运行SEQ WIPC，128KiB顺序写，两倍用户容量，全部的ActiveRange（没有LBA限制）
3. 进行WDPC和测试，设置测试参数、记录数据
    - 测试条件：写缓存关闭、OIO/Thread要求与2中一样、Tread Count要求与2中一致、随机数据
    - 运行下面的循环，直到达到稳定状态或者25次循环：
    for (R/W Mix %=100/0,95/5,65/35,50/50,35/65,5/95,0/100)
    for (Block Size=1024KiB,128KiB,64KiB,32KiB,16KiB,8KiB,4KiB,0.5KiB)
    run RND IO ,per (R/W Mix%,Block Size),for 1 minute,
        记录 Ave IOPS (R/W Mix%,Block Size)
        用（R/W Mix %=0/100,4KiB)的IOPS来检查是否达**稳态**
        如果25次循环后还没达到稳态，则继续执行知道达到稳态，反之，Round-x达到了稳态就停止；测量时间窗口为Round X-4 到Round X
     退出Block Size循环
    退出 Mix 循环
   绘制图形，参考7.3
   结束测试

换成自己的理解：
测试在不同block size与不同的read/write 比的混合下随机IO的性能
block size：100/0, 95/5, 65/35, 50/50, 35/65, 5/95, 0/100
read/write：1024KiB, 128KiB, 64KiB, 32KiB, 16KiB, 8KiB, 4KiB, 0.5KiB
磁盘IO的范围：100% volume size

（1）purge
（2）WIPC：128k顺序写2倍的磁盘空间
（3）WDPC：上述工作负载（bs，r/w） loop运行
（4）稳态判定：取相邻4轮的4kb 随机 0/100的iops，相对均值上下浮动在10%以内，即达到稳态，即取这四轮的测试数据为稳态窗口

测试数据，取稳态窗口的均值



#### Throughput测试
测试在稳态的128k与1m的顺序读与顺序写的吞吐
磁盘IO的范围：100% volume size

（1）purge
（2）WIPC：128k顺序写或1M的顺序写2倍的磁盘空间
（3）WDPC：bs(128k,1M)和rw(100/0, 0/100) 顺序IO，loop，记录吞吐量
（4）稳态判定：取相邻4轮的吞吐，相对均值上下浮动在10%以内，即达到稳态，即取这四轮的测试数据为稳态窗口

测试数据，取稳态窗口的均值

#### 延迟测试
测试在3种block sizes（0.5k，4k和8k），与三种读/写 比（100/0，65/35，0/100）混合下IO响应时间
磁盘IO的范围：100% volume size

（1）purge
（2）WIPC：128k顺序写2倍的磁盘空间
（3）WDPC：bs(0.5k, 4k, 8k)和rw(100/0, 65/35, 0/100) 随机IO，loop，记录最大延迟与平均延迟
（4）稳态判定：取相邻4轮的平均延迟（0/100，4k），相对均值上下浮动在10%以内，即达到稳态，即取这四轮的测试数据为稳态窗口

测试数据，取稳态窗口的均值

#### 稳定性测试
测试SSD性能是随着时间以及写入量的增加是如何变化的（1min的检测穿插着30分钟的WAST测试）

（1）purge
（2）4k的随机写（100%），持续写入达到4倍的磁盘大小或者达到24h或者达到5轮的稳态（满足其一即可）
（3）iostat统计磁盘读写性能数据，取fio延迟波动方差的数据