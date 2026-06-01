---
title: 全流程
parent: 流片笔记
nav_order: 4
has_children: true
---

# 主要流程
# 一切的开始
## 1. 定义需求
1. 任务负载
2. 面积，功耗和算力
3. 特殊功能模块

## 2. 设计架构

1. 核心原则: 必须非常明确，形成书面的 Specification;
2. 系统的层级结构必须清晰;
3. 模块的功能定义必须清晰;

# 前端
## 1. RTL设计
### 1.1 数据在SRAM Buffer的组织形式

### 1.2 计算单元的数据流

[流水线原理与握手机制](https://xuptarch.com/wiki/lectures/c/ch10-pipeline/)

*如何使计算单元变成Pipeline?*

1. 可以先设计纯组合逻辑 (以流程图的形式)
2. 然后在合适的地方插入 reg
3. 对应的控制信号也要 Pipeline

*握手协议*

设当前级是 mm，下一级是 dn，上一级是 up：

| signal         | 解释                                   |
|----------------|----------------------------------------|
| mm_valid       | 本级当前是否持有有效事务。             |
| mm_ready_go    | 本级内部计算是否完成（与下游无关）。   |
| dn_allowin     | 下游是否允许本级送入新事务。           |
| mm_allowin     | 本级是否允许上游覆盖本级段寄存器。     |
| mm_to_dn_valid | 本级此拍是否向下游发射有效事务。       |
| up_to_mm_valid | 上游此拍给本级的数据是否有效。         |

```verilog
// 公式: 
assign mm_allowin     = !mm_valid || (mm_ready_go && dn_allowin);
assign mm_to_dn_valid =  mm_valid && mm_ready_go;
/*
* 解释: 
* 1. 本级为空，当然可以收（!mm_valid）。
* 2. 本级不空时，只有“本级完成 + 下游能收”才能收新的，避免覆盖旧数据。
* 3. 能发射的前提是“当前持有有效事务且本级完成”。
*/
```


### 1.3 控制流与控制单元
1. 用好各个模块的 done 信号，可以很好地避免计数器 
2. 在综合后，修setup违例时，常常需要添加寄存器。这意味着数据流水级数的增加。进而导致对应控制信号的流水级数增加，顶层控制模块如果没能很好地落实上一条，修改起来会很麻烦。


## 2. 仿真(SIM)
### 2.1 TestBench

假设要给一个 两操作数的Adder 输入激励:
```verilog
// 必须在 clk 的上跳沿给入，禁止使用 #10; 类似的表述
// 必须是 非阻塞赋值( <= ) 
@(posedge clk) begin
    op_a <= input_data[0];
    op_b <= input_data[1];
end
```

假设 dut 模块有较多的信号值，但是还没写控制模块:
```verilog
initial begin
        .....
    give_in_act(in_mem[3]); //在initial块中，方便地给入激励
        .....
end
//通过 task 将复杂的控制进行包装
task automatic give_in_act;
    input input_data;
@(posedge clk) begin
    input_valid <= '1;
    op_a        <= input_data[0];
    op_b        <= input_data[1];
end
endtask
```


### 2.2 准备 Input Data 和 Golden Result
### 2.3 使用脚本自动化执行全部Case

## 3. 挂载到CPU上
### 3.1 修改 hardware 文件夹下的内容
### 3.2 修改 software 文件夹下的内容
### 3.3 测试数据与测试脚本


## 4. 综合(SYN)
### 4.1 修 setup 违例

## 5. 综合后仿真(SIM_POST_SYN)
### without sdf
### with sdf


# 后端