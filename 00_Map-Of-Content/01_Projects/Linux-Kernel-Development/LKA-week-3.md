---
tags:
  - Knowledge/Operating-Systems
created: 2025-09-22
author:
  - ln1
status: Done
---
![[Pasted image 20250922101926.png]]
BaseAddress:offset
GDT:保护模式->全局
- dw:2 Bytes
- db:1 Byte
- dd:4 Bytes
- Macro:宏->宏汇编：
	- 第一个：端基址
	- 第二个：段限界
	- 第三个：属性
- LABEL_GDT和LABEL_DESC_NULL的地址是一致的。
- $:当前的地址
- Selecor，选择子：索引、下标
### Selector:
![[Pasted image 20250922104828.png]]
- 8:1 0 00->1
- 16:10 0 00->2
- 24:11 0 00->3
GDTR:GDT寄存器
![[Pasted image 20250922110046.png]]
- 大端法：低地址位置放高字节
- 小端法：低地址位置放低字节
引用：[[Chapter-2-信息的表示和处理#1.3.寻址和字节顺序]]


![[Pasted image 20250922111055.png]]
### CR0:Control Register 0;
- PE：若为1,则为保护模式
- CS:Code Segment
- IP：英特尔下指令指针寄存器
- DS:Data Segment
- SP：Stack Pointer
- XOR eax,eax -> 将eax置为0
- 
