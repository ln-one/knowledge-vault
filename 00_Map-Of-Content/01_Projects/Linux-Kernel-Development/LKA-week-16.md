---
tags:
  - Knowledge/Operating-Systems/Linux
created: 2025-12-22
author:
  - ln1
status: In Progress
---
- 每个进程都有一个**TSS(Task State Segment)**
- 描述符都是八个字节
- task_struct 西文下划线
- 系统(空间)堆栈的大小**不能在运行时动态扩展**
- pending：**待处理**
- 文件是某个用户通过运行某种程序来形成的。
- offspring ：后代
- orphan：孤儿
- fork vfork clone 创建进程的三个方式：sys_fork sys_vfork sys_clone
- 不同的系统调用的中断号都是0x80,通过eax传入系统调用号
- ![[Pasted image 20251222110322.png]]
- <0 调用失败 =0转移给子进程 >0转移给父进程
- ![[Pasted image 20251222111014.png]]
- 什么是cow（copy on write)。
- execve：调用另一个可执行文件
- 