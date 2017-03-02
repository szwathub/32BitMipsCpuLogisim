## 数据重定向的流水冲突处理
相比插入气泡的流水冲突处理, 数据重定向在冲突检测中, 新加两个输出, 用于处理未及时写回的数据,具体见[图 4.8](#conflict1)

数据重定向的流水冲突处理具体实现见[图 4.9](#conflict_bypass)

![conflict1](./pic/4.8.png)
<center><span id="conflict1">图 4.8 数据重定向的流水冲突处理</span></center>

![conflict_bypass](./pic/4.9.png)
<center><span id="conflict_bypass">图 4.9 数据重定向的流水冲突处理CPU</span></center>
