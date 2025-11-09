---
tags:
  - Knowledge/Operating-Systems/Linux
created: 2025-11-03
author:
  - ln1
status: In Progress
---
text file -> executable file/binary

- static linking:静态链接，
- dynamic linking:动态链接
	- \*.dll
	- \*.exe
- framework: c# -> .net c->library javascript -> vue
- ![[Pasted image 20251103102235.png]]
- 段(segments)：保存了文件运行时需要的信息。
- 节(sections)：包含了链接和重定位的数据。
- 重定位(relocation)： **把代码中引用的地址修改为实际运行时的地址。** 逻辑地址->物理地址。地址映射。
![[Pasted image 20251103104204.png]]
- .text放的code ELF是**二进制**的。
- 序列：有穷序列为元组。
`nums = (2, 4, 6)`
![[Pasted image 20251103105220.png]]
```asm
myprint:
	mov edx, [esp + 8] ; len
	mov ecx, [esp + 4] ; msg
	mov ebx, 1             ->打开文件，写文件
	mov eax, 4 ; sys_write 
	int 0x80 ; 系统调用 interrupt 软中断
	ret
  ```
**考试考**：为什么C语言一般不打开文件，stdin stdout stderr：因为子进程会继承父进程打开的文件。
- 父进程打开了，所以子进程继承了，因此没有直接写。
--- 
- esp放**返回地址**
- esp+4放参数
- esp+8放参数...
