---
tags:
  - Knowledge/Operating-Systems/Linux
created: 2025-12-01
author:
  - ln1
status: In Progress
---
- 中断描述符表：保护模式下说法
- 中断向量表：实模式下说法
- **X86保护模式下中断响应过程是什么？**
- SYSCALL_VECTOR:80
- 设置门先构造。
- _set_gate(idt_table+n,15,3,addr); //对应陷阱门，DPL=3_ 具备什么样特权级的可以穿过这道门。
- 外部中断push 0x00 **-256** :用来区分系统调用和外部中断（负数）。
