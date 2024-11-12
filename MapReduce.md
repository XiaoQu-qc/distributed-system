- MapReduce 是一种用于处理和生成大型数据集的编程模型和相关实现。
## 2对模型的基本介绍
介绍由用户编写的map(key,value) reduce(key,valuelist)
map函数返回中间键值对，而reduce的作用就是将这些中间键值对合并，如果键相同
## 3本节介绍针对谷歌广泛使用的计算环境的实施方案：通过交换式以太网连接在一起的大型商品 PC 集群
输入文件会被分为64MB的多个块，map任务的worker负责每一个块
*当还原工作者读取完所有中间数据后，它会根据中间键对数据进行排序，以便将所有出现相同键的数据归为一组。*
- 整个模型的执行过程
中间键值对文件被分成了r份任务，每一台被分配了reduce任务的机器都将完成其中一项reduce任务成功完成后，mapreduce 执行的输出可在 R 个输出文件中找到（每个还原任务一个，文件名由用户指定）。
这R个输出文件可能还含有未完全合并的键值对，也就是说不同的输出文件可能含有相同的键，对应不同的value值集合。接下来可以再次进行Mapreducce操作或者是从另一个能够处理分割成多个文件的输入的分布式应用程序中使用这些文件
- Consider map task M and reduce tasks R1 and R2.  Let e(Ri) be the execution of Ri that committed (there  is exactly one such execution). The weaker semantics  arise because e(R1) may have read the output produced  by one execution of M and e(R2) may have read the  output produced by a different execution of M .没太弄明白在说什么
- 输入数据存储在组成集群的计算机的本地磁盘上，以节约网络带宽，MapReduce 主程序会考虑输入文件的位置信息，并尝试在包含相应输入数据副本的机器上调度map任务
- 当 MapReduce 操作接近完成时，主控程序会安排剩余正在进行的任务的备份执行。只要主任务或备份任务执行完成，任务就会被标记为已完成。
