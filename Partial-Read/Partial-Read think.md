SOML考虑相同plane中不同block的partial read，没有考虑：

1. 选取的位是哪个位：如TCL的LSB、MSB、CSB的读取延迟相差较大
2. 选取block的磨损情况，数据的存储时间
3. 选取不同位置的page其RBER也不相同，比如3D TLC中，各个层次的page的RBER有着差异
   上述情况都会导致读取延迟取决于选取的page的读取延迟的最大值，导致延迟提升

LRR考虑block当中的partial read，优化了SOML存在的问题，毕竟一个Block中磨损基本是一样，且考虑的3D TLC的层次差异，但也存在以下问题：

1. 同样没有考虑选取哪个页，如TCL的LSB、MSB、CSB的读取延迟相差较大
2. 没有考虑3D TLC所才用的Two Pass Programming。对于一个写命令，NAND 芯片首先将数据存储到cache register，再将cache register中的数据刷到指定位置。LRR对于一个full page 的写划分成4份分配到4个个page中，数据暂存DRAM（cache register一般一个Plane也就两个page大小），如果是写到LSB中。该写请求需要等待4个page都写满了，再逐个从DRAM传输到cache register，再写入NAND芯片；如果是MSB或CSB，则需要等待8个page都写满，才从DRAM传输到cache register，再写入NAND芯片。是否引起为延迟？过分占用DRAM空间（因为现在追求速度，会将一个大写分配到多个chip的多个Plane当中，提升读写并行性）？
