execve：执行其他的可执行程序
CLONE_VFORK:直到execve和_exit执行 释放空间。
CLONE_VM：调用进程 子进程运行在同一地址空间。
dup_mmap(mm)：拷贝的是指针指向的内容。
fs:file system 文件系统的相关信息
fork()执行的代码多，vfork()执行的代码少
实模式寻址(Addressing)
![[Pasted image 20251229115055.png]]![[Pasted image 20251229115637.png]]![[Pasted image 20251229120429.png]]
![[Pasted image 20251229120615.png]]
填空题 选择题 判断题 读程序题 简答题 （40分）  编程题（10分，编程题）fork vfork 区别 为什么 