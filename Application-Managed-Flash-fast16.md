**题目： Application-Managed Flash（AMF)**

会议：FAST 16

**目前SSD缺点**：1. SSD内部需要内存和算力 2.与高层应用无沟通（高层应用不知垃圾回收时间，以及映射表） 3. 与用户功能重复（有的高层应用自身避用更新写，如LSM-Tree，把数据添加，而不是就地更新）

AMF基本规则，不允许用户覆盖写，除非原本数据已经被明确删除了

优点：1.高效排序I/O 2.准确分布冷热数据 3.直接映射到物理地址，而不需要映射表

**FTL功能** 减少到bit错误纠正，数据并行，坏块跟踪，磨损均衡（？？？）

管理密度，以块为单位，减少mapping table

**AMF Block I/O Interface**

![img](image/Application-Managed-Flash-fast16/1634819112054.png)

三种命令：READ,WRITE and TRIM（以segment为单位）

一个sector（4KB）只能被写一次，除非其所在segment被命令TRIM

一个segment被分配到多个不同通道上的块上，提高带宽
