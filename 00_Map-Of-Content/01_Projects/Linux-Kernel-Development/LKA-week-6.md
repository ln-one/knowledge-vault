---
tags:
  - Knowledge/Operating-Systems/Kernel
  - Knowledge/Operating-Systems/Linux
created: 2025-10-13
author:
  - ln1
status: Done
---
![[Pasted image 20251013104227.png]]

### 特权级(Privilege Level):
![[Pasted image 20251013105200.png]]
#### DPL(Descriptor Privilege Level)：
描述符特权级
#### RPL(Requested Privilege Level):
请求特权级
#### CPL(Current Privilege Level):
当前执行的程序或任务的特权级，被存储在**CS和SS的第0位和第1位上**。

---
- 处理器使用特权级机制来避免低特权级的任务在不被允许的情况下访问位于高特权级的段
- 一般来说处理器由PC指向下一条指令的地址，Intel微处理器是`CS:EIP`(保护模式)`CS：IP`(实模式) 选择字:偏移量  来指向下一个地址

### 控制权转移的特权级检查
在将控制权从一个代码段转移到另一个代码段之前，目标代码段的段选择子必须被加载到CS中
- 控制转移： 需要执行的指令序列发生了变化
- 控制权转移可以由指令JMP, CALL, RET,SYSENTER, SYSEXIT, INT n 和IRET 引起，也可以由中断和异常机制引起
- JMP和CALL直接转移：
	- 段内进行近跳转(Near)：不进行特权级检查
	- 段间进行远跳转(Far)：进行特权级检查
### 调用门描述符
堆栈：
- 保存上下文，保存现场
- 传递参数
门描述符号：特权级检查
### 短调用堆栈的变化
![[Pasted image 20251013115854.png]]
![[Pasted image 20251013115917.png]]
### 长调用堆栈的变化
![[Pasted image 20251013120012.png]]
![[Pasted image 20251013120021.png]]
为了从ring 0 跳转到 ring 3,我们并不使用CALL而直接执行RET。
retf : return far