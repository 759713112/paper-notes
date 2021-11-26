**题目**：Reducing Solid-State Storage Device Write Stress Through Opportunistic In-Place Delta Compression

会议：fast16

关键词： partial program， SLC-mode， delta compression

base：将一部分block置为SLC mode，用其作为cache存储热数据，提高写速度

some tips：为了保证磨损均衡，作为SLC mode的block轮换着来

**问题**：因为一些位置容易频繁更新具有时间局部，且更新的数据量很小

**当前方法**：使用增量压缩，将更新的数据写入不同的物理页，但读取时，需把这些增量都读出来产生读放大，且还需保存原始数据以及增量的数据表

提出方法：

**基本思路**：将以sub-page的更新写在同一物理页中，当超过阈值，则更新到一个新的页

解决问题：1.预留空间给delta compression会造成空间浪费 2. delta compression数据较小，无法应用一般ECC

1.将原本物理页内的数据无损压缩，剩余的用来存储delta compression
