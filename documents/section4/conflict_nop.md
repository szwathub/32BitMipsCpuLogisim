## 插入气泡的流水冲突处理

当检测到数据冲突, 检测器向后插入气泡, 并且前段停止.当发生跳转时清空IF/ID, ID/EX.

具体实现见[图 4.7](#conflict_nop)

![conflict_nop](./pic/4.7.png)
<center><span id="conflict_nop">图 4.7 插入气泡的流水冲突处理CPU</span></center>
