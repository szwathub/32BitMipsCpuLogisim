## 构建单周期CPU
### 主要功能部件实现
#### 程序计数器PC
##### 选用的元器件
- 寄存器(数据位宽: 32, 触发方式: 上升沿, 标签: Reg)
- 多路选择器

##### 输入
- 时钟信号
- 地址输入: 下条指令的地址
- 选择信号

##### 输出
- 寄存器的输出端.

##### 具体实现
计数器的时钟端连线时钟隧道,时钟隧道连接一个非门,然后接到寄存器的时钟端,
清零隧道直接连接计数器和寄存器的清零端,计数器的输出端连接寄存器的数据段,具体电路见[图 4.1](#pc).

![程序计数器PC实现图](./pic/4.1-3.png)

<center><span id="pc">图4.1 程序计数器PC实现图</span>  <span id="InstrDecoder">图4.3 InstrDecoder</span></center>


<div style="page-break-after: always;"></div>
#### 运算器ALU
一个32位运算器,可支持算数加、减、乘、除,逻辑与、或、非、异或运算、逻辑左移、逻辑右移,算术右移运算,
支持常用程序状态标志(有符号溢出OF、无符号溢出CF,结果相等Equal),运算器功能以及输入输出引脚见[表 4.1](#alu_des)

<center><span id="alu_des">表4.1 运算器引脚与功能描述</span></center>

   PIN   | I/O | Bit-width | Describe
---------|-----|-----------|---------
 X       | IN  | 32        | 操作数X
 Y       | IN  | 32        | 操作数Y
 ALU_OP  | IN  | 4         | 运算器功能码,具体功能见下表
 Result  | OUT | 32        | ALU运算结果
 Result2 | OUT | 32        | ALU结果第二部分,用于乘法指令结果高位或除法指令的余数位,其他操作为零
 OF      | OUT | 1         | 有符号加减溢出标记,其他操作为零
 CF      | OUT | 1         | 无符号加减溢出标记,其他操作为零
 Equal   | OUT | 1         | Equal=(x==y)?1:0, 对所有操作有效

硬件原理如[图 4.2](#alu_struct)所示.

#### InstrDecoder

指令译码器将指令译码取出各个部分, 具体见[图 4.3](#InstrDecoder). 此处不再赘述.

#### ControlUnit
##### PcControl
- I-Type
<center>
$$ PcControl1 = \overline{op5} \cdot \overline {op3} \cdot \overline {op2}$$
</center>
<center>
$$ PcControl0 = \overline{op5} \cdot \overline {op3} \cdot op2 $$
</center>

- R-Type
<center>
$$ PcControl1 = \overline{Fun4} \cdot Fun3 \cdot \overline{Fun1} $$
</center>
<center>
$$ PcControl0 = \overline{Fun4} \cdot Fun3 \cdot \overline{Fun1} $$
</center>

##### 写寄存器位置
- I-Type

![alu](./pic/4.2.png)
<center><span id="alu_struct">图4.2 运算器硬件原理</span></center>

<center>
$$ RegWriteDst1 = 0 $$
</center>
<center>
$$ RegWriteDst0 =\overline{op5}  \cdot \overline{op4} \cdot \overline{op3} \cdot \overline{op2} \cdot op1 \cdot
op0 $$
</center>

- R-Type
<center> $$ RegWriteDst1 = 1 $$ </center>
<center> $$ RegWriteDst0 = 0 $$ </center>

##### R1
- I-Type
<center> $$ R1 = 1 $$ </center>

- R-Type
<center> $$ R1 =  \overline{Fun5} \cdot \overline{Fun4} \cdot \overline{Fun3} $$ </center>

##### R2
<center> $$ R2 = \overline{Fun2} + Fun5 $$ </center>

##### DM_Din
<center> $$ DMDin1 = op1 $$ </center>
<center> $$ DMDin0 = \overline{op1} \cdot op0 $$ </center>

##### MemWrite
<center>
$$ MW = op5 \cdot \overline{op4} \cdot op3 \cdot \overline{op2} \cdot \overline{op1} + op5 \cdot \overline{op4} \cdot op3 \cdot \overline{op2} \cdot op0 $$
</center>

##### RF_Din
- I—Type
<center> $$ RFDin1 = 0 $$ </center>
<center> $$ RFDin0 = \overline{op3} $$ </center>

- R-Type
<center> $$ RFDin1 = Fun4 $$ </center>
<center> $$ RFDin0 = Fun4 \cdot Fun0 $$ </center>


##### ALUop
- I-Type
<center> $$ ALUOP_3 = op1 \cdot \overline{op0} + op2 \cdot op0 $$ </center>
<center> $$ ALUOP_2 = \overline{op1} \cdot \overline{op0} + \overline{op2} \cdot op0 $$ </center>
<center> $$ ALUOP_1 = op1 \cdot \overline{op0} + op2 \cdot \overline{op0} $$ </center>
<center> $$ ALUOP_0 = \overline{op2} + \overline{op0} $$ </center>

- R-Type
<center> $$ ALUOP_3 = Fun2 \cdot Fun0 + \overline{Fun4} \cdot Fun3 $$ </center>
<center> $$ ALUOP_2 = Fun2 \cdot \overline{Fun0} + Fun3 \cdot Fun0 + Fun5 \cdot \overline{Fun3} \cdot \overline{Fun2} $$ </center>
<center> $$ ALUOP_1 = Fun1 \cdot \overline{Fun0} + Fun2 \cdot \overline{Fun0} + Fun2 \cdot Fun1 + Fun4 \cdot \overline{Fun0} $$ </center>
<center> $$ ALUOP_0 = \overline{Fun3} \cdot \overline{Fun2} \cdot Fun0 + Fun3 \cdot \overline{Fun0} + Fun5 \cdot \overline{Fun1} \cdot \overline{Fun0} $$ </center>

##### RegWriteBack
- I-Type
<center> $$ RWB = \overline{op5} \cdot \overline{op4} \cdot op3 + op5 \cdot \overline{op4} \cdot \overline{op3} \cdot \overline{op1} + op5 \cdot \overline{op4} \cdot \overline{op3} \cdot \overline{op2} \cdot op0 $$ </center>

- R-Type
<center>
$$
RWB = \overline{Fun5} \cdot \overline{Fun3} \cdot \overline{Fun2} \cdot \overline{Fun0} + \overline{Fun4} \cdot \overline{Fun3} \cdot \overline{Fun0} + \overline{Fun4} \cdot \overline{Fun3}
\cdot Fun1
$$
</center>
<center>
$$ + \overline{Fun5} \cdot \overline{Fun4} \cdot Fun3 \cdot \overline{Fun2} \cdot
\overline{Fun1} \cdot Fun0
$$
</center>
<center>
$$+ Fun5 \cdot \overline{Fun4} \cdot \overline{Fun3} + Fun5 \cdot
\overline{Fun4} \cdot \overline{Fun2} \cdot Fun1
$$
</center>

##### ALUsrc
- I-Type
<center> $$ ALU_1 = op3 \cdot op2 $$ </center>
<center> $$ ALU_0 = \overline{op2} + op5 $$ </center>

- R-Type
<center> $$ ALU_1 = \overline{Fun5} \cdot \overline{Fun3} \cdot \overline{Fun2} $$ </center>
<center> $$ ALU_0 = \overline{Fun5} \cdot \overline{Fun3} \cdot \overline{Fun2} $$ </center>

### 总体结构
单周期CPU总体结构如[图 4.4](#Single_Cpu), 已经除去多余信息(如数码管显示等)

![Single_Cpu](./pic/4.4.png)
<center><span id="Single_Cpu">图4.4 单周期CPU结构图</span></center>
