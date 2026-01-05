---
tags:
  - Knowledge/Operating-Systems/Linux
created: 2025-11-24
author:
  - ln1
status: In Progress
---
外部中断：CPU之外的部件发出的中断
ISR：中断服务程序（Interrupt Service Routine）就是代码
![[Pasted image 20251124110350.png]]
- 中断向量：中断服务程序的提供的入口地址![[Pasted image 20251124112148.png]]
- 实模式下，中断向量4字节：最多256个中断  段地址：偏移量 每个16位，两个32位 4字节  256个 。一个四字节，一共1024。必考。
- 描述符都是8字节。 
- ![[Pasted image 20251124112906.png]]
- IDT 的存在本质上就是 **为了系统能快速、确定地响应各种中断**。**中断是随机发生的，不知道哪一刻哪个中断会来**，所以需要一个统一的索引表来快速定位处理程序。
- 门：保护；