---
tags:
  - Papers
icon: TiNotes
---

# Compiler Techniques for Reducing Data Cache Miss Rate on a Multithreaded Architecture

---

## Introduction

提升性能和减少能耗的两种方法：

- 缓存
- 多线程

但是二者结合的情况会导致复杂和难以预测的冲突，因为线程间不可预计的交互会导致很高的缓存未命中率

在大型且高度关联的缓存中，这种线程间互动会比较小，而对于嵌入式架构来说，它们更容易把多线程和小且简单的缓存结合在一起

本文旨在介绍**通过编译器的软件方法使得嵌入式架构能够继续使用这些小型简单的缓存而无需更换为大型复杂能耗高的缓存**

### CCDP

缓存感知数据放置(*Cache-Conscious Data Placement*)，是一种为应用程序的数据对象找到智能布局的技术，以便在运行时以交错模式访问的对象不会映射到相同的缓存块

在单一上下文的处理器上，CCDP已被证明是能够减少缓存为命中率和提升性能表现的

然而在多线程环境下（如同步多线程），CCDP失去了很多优势

在同步多线程(*Simultaneous Multithreading, SMT*)处理器中，多个线程在不同的硬件上下文中并发运行，相比于其它传统的用来提高处理器性能的优化方法来说这种架构已经被证明是更节能的，因此被作为高性能嵌入式架构的候选者

在多线程共享缓存的同步多线程处理器中，来自不同线程的对象会竞争相同的缓存行，导致了潜在巨大的线程间冲突数据未命中，而这种冲突并不能使用先前的方法来检测，因为线程间的冲突是不确定的

![Figure 1](Papers/Reducing_Miss_Rate_by_Compiler_Techniques/Reducing_Miss_Rate_by_Compiler_Techniques.pdf#page=2&rect=121,547,473,702)

图一表明：

- 线程间冲突与线程内冲突导致的数据未命中一样普遍
- 相较于其它冲突类型，这种冲突的出现导致冲突整体的影响显著增加

扩展来说，多核处理器架构也会有这种问题，多个处理器可能共享一块L2缓存甚至是L1缓存

### New Techniques

本文只关注多线程架构，因为这是最基本的多线程交互和共享缓存的模式

研究者开发了新的可以扩展CCDP到多线程架构的技术

考虑以下的编译场景：

- 最普通的情况，即我们并不事先知道哪些应用会被一起调度，这种情况在嵌入式架构中也可能发生
- 更具体的嵌入式应用的情况，我们可以更精确了解应用是如何运行的，这样协同编译至少是可行的

在更专业的嵌入式应用程序中，我们将能够更精确地利用有关应用程序及其运行方式的特定知识来协同编译，或者至少协同编译这些并发运行的应用程序

### Contributions

- 证明传统的不基于多线程架构的缓存感知数据放置方法在多线程架构下不是有效的，甚至是有害的
- 提出两个CCDP的扩展，可以用于识别和清除大部分尚文所述编译情况下的线程间缓存冲突
- 证明即使对于具有许多对象和交错的应用程序，也可以在不牺牲性能和放置质量的情况下保持合理大小的时间关系图
- 使用多种机制来提升CCDP的性能表现
- 证明这些机制可以在不同的缓存配置中发挥作用
