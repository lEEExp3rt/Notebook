---
tags:
  - ZJU-Courses
icon: 1️⃣
---

# Chapter 1: Fundamentals of Computer Design

## Performance

- 响应时间
- 数据吞吐量

> The main goal of architecture improvement is to improve the performance of the system.

## The Improvement of Computer Architecture

- I/O 设备提升
- 存储设备发展
- 指令集架构
- 并行处理技术

## Quantitative Approaches

### CPU性能

$$\text{Performance}=\dfrac{1}{\text{Execution Time}}$$

$$\text{CPU Execution Time}=\text{CPU Clock Cycles}\times\text{Clock Period}=\dfrac{\text{CPU Clock Cycles}}{\text{CPU Clock Rate}}$$

### Amdahl定律

Amdahl定律表述了系统性能提升由被提升的部分的提升时间占总体的提升部分比例决定

$$\text{Improved Execution Time}=\dfrac{\text{Affected Execution Time}}{\text{Amount of Improvement}}+\text{Unaffected Execution Time}$$

基于此，系统性能的**加速比**可以归纳为：

$$\text{Speedup}=\dfrac{\text{Performance for entire task}_\text{Using Enhancement}}{\text{Performance for entire task}_\text{Without Enhancement}}=\dfrac{\text{Total Execution Time}_\text{Without Enhancement}}{\text{Total Execution Time}_\text{Using Enhancement}}$$

更进一步，可以得到：

$$\text{Execution Time}_\text{new}=\text{Execution Time}_\text{Not Enhanced}+\text{Execution Time}_\text{Enhanced}=\text{Execution Time}_\text{Not Enhanced}\times[(1-\text{Fraction}_\text{Enhanced})+\dfrac{\text{Fraction}_\text{Enhanced}}{\text{Speedup}_\text{Enhanced}}]$$

所以，

$$\text{Speedup}_\text{Overall}=\dfrac{\text{Execution Time}_\text{old}}{\text{Execution Time}_\text{new}}=\dfrac{1}{(1-\text{Fraction}_\text{Enhanced})+\dfrac{\text{Fraction}_\text{Enhanced}}{\text{Speedup}_\text{Enhanced}}}$$

> [!example]+ Amdahl定律
> > [!info] 来源
> > - [课件例题1](Chapter1.pdf#page=62)
> > - [课件例题2](Chapter1.pdf#page=63)
> 
> > [!example] 例题1
> > Increasing the processing speed of a certain function in the computer system to **20 times** the original, but the processing time of this function only accounts for **40%** of the running time of the entire system. After adopting this method to improve performance, how much can the performance of the entire system improve?
> > 
> > > [!tip]- 答案 
> > > - $\text{Fraction}_\text{Enhanced}=40\%$
> > > - $\text{Speedup}_\text{Enhanced}=20$
> > > 
> > > 那么$\text{Speedup}=\dfrac{1}{0.6+\dfrac{0.4}{20}}=1.613$
> 
> > [!example] 例题2
> > After a computer system adopts floating-point arithmetic components, the floating-point arithmetic speed is increased by 20 times, and the overall performance of a certain program of the system is increased by 5 times. Try to calculate the proportion of the floating-point operations in this program.
> > > [!tip]- 答案
> > > - $\text{Speedup}_\text{Overall}=5$
> > > - $\text{Speedup}_\text{Enhanced}=20$
> > > 
> > > 可得$\dfrac{1}{(1-\text{Fraction})+\dfrac{\text{Fraction}}{20}}=5$
> > > 
> > > 解得$\text{Fraction}=84.2\%$

## 良好的设计想法

好的体系结构设计应该遵循以下8个想法：

- Design for **Moore's law**
- Use **abstraction** to simplify design
- Make the **common case fast**
- Improve performance via **parallelism**
- Improve performance via **pipelining**
- Improve performance via **prediction**
- Use **a hierarchy of memories**
- Improve dependability via **redundancy**
