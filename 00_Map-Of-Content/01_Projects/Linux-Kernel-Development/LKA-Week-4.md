---
tags:
  - Knowledge/Operating-Systems/Linux
  - Knowledge/Operating-Systems
  - Knowledge/Operating-Systems/Kernel
created: 2025-09-29
author:
  - ln1
status: In Progress
---
 - ORG 7C00H 是告诉汇编器：这段程序将被加载到内存的 **0x7C00** 地址处开始执行，这是 **BIOS 启动加载引导扇区的标准位置**。
 - org 0100h可以现在DOS上执行。预留0100h -> 256字节
 - 一个GDT描述符定义是8个字节，求GdtLen就是定义的GDT描述数量 ✖️ 8bytes
 - 一个`$`当前的地址 ，两个是`$`离当前最近的节的地址。
 
 - lodsb：将[si/esi] 指向的字节加载到al ![[Pasted image 20250929115102.png]]
 - test拿两个操作数做与运算。