## 实验原理与环境
### 实验原理

本次实验与之前的单元实验室层层推进的, 而本次实验过程也是逐步推进.在前几次单元实验的基础上, 结合学过的知识, 不难设计出单周期CPU的逻辑电路图.
单周期CPU由PC、指令存储器IM、寄存器堆RF、运算器ALU、数据存储器DM,  ControlUnit五个主要功能部件组成.

其电路原理图如[图 2.1](#mips)

![图 2.1 Single-cycle MIPS Processor](./pic/2.1.png)
<center><span id="env">图2.1 Single-cycle MIPS Processor</span></center>


<div style="page-break-after: always;"></div>
### 实验环境
具体测试环境见[表 2.1](#env).

<center><span id="env">表2.1 测试环境配置</span></center>

Deploy                  | Describe
---|---
System                  | Mac OSX EI Capitan
Model Name              | MacBook Air
Model Identifier        | MacBookAir6,2
Processor Name          | Intel Core i5
Processor Speed         | 1.3 GHz
Number of Processors    | 1
Total Number of Cores   | 2
L2 Cache(per Core)      | 256 KB
L3 Cache                | 3 MB
Memory                  | 4 GB
Development tools       | logisim-2.7.1
