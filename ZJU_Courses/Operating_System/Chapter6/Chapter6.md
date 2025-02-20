---
tags:
  - ZJU-Courses
icon: 6️⃣
---

# Chapter 6: Process Synchronization

---

## Background

多道程序环境下，进程是并发执行的，不同进程间存在着不同的相互制约关系，为了协调进程之间的相互制约关系，达到资源共享和进程写作，避免进程冲突，引入进程同步的概念

共享内存通常使用**有界缓冲区**，不同进程写有界缓冲区时对缓冲区计数器的操作必须遵循**原子性原则**，同理不同进程访问共享资源时也要执行原子操作

竞争(*Race Condition*)是指多个进程并发地访问和控制共享数据，最终的数据值取决于哪个进程最后完成，运行的结果由进程的访问数据顺序决定

为了避免竞争的发生，需要对并发的进程进行同步

进程之间的竞争面临的三个控制问题：

- 互斥(*Mutual Exclusion*)
- 死锁(*Deadlock*)
- 饥饿(*Starvation*)

**进程的最基本特性：并发性、独立性**

---

## The Critical-Section Problem

系统的共享资源区虽然可以提供给多个进程访问和使用，但是一个时间段内只允许一个进程访问该资源，称这样的资源为**临界资源**，这是一种互斥共享方式

> [!example] 临界资源
> - 硬件打印机
> - 消息缓冲队列
> - 公共变量
> - 文件
> - ...

与之相反的是同时共享方式，允许一个时间段内多个进程同时进行访问

访问临界资源的代码段称为**临界区**(*Critical Section*)，当一个进程在临界区内执行时，其它进程不允许在它们的临界区内执行，每个进程可以有一个或多个临界区，其设置方法由程序员决定

通常来说的临界区代码符合以下逻辑：

1. 检查是否可进入临界区
    1. 若可以进入，则上锁，防止其它进程进入
    2. 若不可进入，则等待
2. 访问临界区，执行代码
3. 解锁
4. 其它处理

需要注意，解决临界区问题必须要满足：

- 互斥
- 空闲让进(*Progress*)：如果临界区内没有正在执行的进程，且有进程希望进入临界区，则允许这些进程进入临界区访问
- 有限等待(*Bounded Waiting*)：等待访问临界区的进程不能无限等待，防止饥饿
- 让权等待：如果进程无法进入临界区，则立即释放处理机

实现同步的机制有

- 软件方法：代码逻辑实现
- 硬件方法：通过CPU中断实现
- 信号量机制

---

## Peterson's Solution

进程互斥的软件实现方法

> [!note] 前提
> 假设`LOAD`和`STORE`指令都是原子性的，即要么完全完成要么不完成

### Single Flag

每个进程进入临界区的权限由一个标志变量决定，一个进程退出临界区后，标志改变，另一个进程马上进入临界区，如此交替

强制进程进入临界区，没有考虑进程的实际需要，容易造成资源利用不均匀，此外当一个进程让出临界区之后，另一个进程使用临界区之前，这个让出临界区的进程不可能再次使用临界区，**违反了空闲让进原则**

### Double Flag

对单标志法进行改进，采用两个标志，只有当前进程想要进入临界区且另一个进程不在临界区时才进入

可能导致两个进程同时进入临界区，因为**进程进入临界区前先检查能否进入，然后再进入**，在这个间隙可能两个进程同时做这一步，就会导致同时进入，**违反了互斥原则**

改进方法：先设置标志，再进入，即**双标志后检查法**

可能导致两个进程都进入不了临界区，**违反了有限等待原则**

### Peterson

结合了单标志法和双标志法

每个进程先将各自的标志记录，表达想要进入临界区的想法，然后等待单标志轮到自己进程进入时，二者都满足才进入临界区

---

## Bakery Algorithm

Lamport算法称为面包房算法，解决了多个线程并发访问一个共享的单用户资源的互斥问题的算法

> [!tip] 类比面包房
> 1. 有n个顾客进入面包房买巧克力面条蒸蛋糕，安排他们按照次序在前台登记签到的号码。该签到号码依次加1
> 2. 根据签到号码的由小到大的顺序依次进店买好吃的蛋糕
> 3. 完成购物的顾客在前台把自己的签到号码归0
> 4. 如果完成购买的顾客要再次进店需要重新排队
> 
> > [!note] 注意
> > 同时进入面包店采购的两个或两个以上的顾客有可能得到相同的号码，多个顾客如果拿到相同的号码，则规定按照顾客名字的字典顺序进行排序

```c title:"Bakery Algorithm" hl:9,10,11,12
// Shared datra.
bool choosing[n]; // Whether a process is getting its queue number.
int number[n]; // A process's queue number, if 0, means the process is not enqueued.

// Process.
void Process(int pid)
{
    do {
        // Atomic operation of number[i].
        choosing[pid] = true;
        number[pid] = max(number[0], number[1], ..., number[n - 1]) + 1; // Enqueue.
        choosing[pid] = false;
        // Wait for other process to quit the area.
        for (int j = 0; j < n; j++) {
            while (choosing[j]);
            while ((number[j] != 0) && (number[j], j) < (number[pid], pid)); 
        }
        // Your turn.
        Critical Section;
        number[pid] = 0; // Dequeue.
        Remainder Section;
    } while (1);
    return ;
}
```

每个事件对应一个lamport时间戳，初始值为0

- 如果事件在节点内发生，时间戳加1
- 如果事件属于发送时间，时间戳加1，并在消息中带上该时间戳
- 如果事件属于接收事件，时间戳为本地时间戳和消息中的时间戳的最大值加1

---

## Synchronization Hardware

由硬件提供特殊的原子硬件指令完成硬件方法

### Test-And-Set

```c
bool lock = false; // Shared data.

// Test-And-Set.
bool TS(bool *lock)
{
    bool old = *lock;
    *lock = true;
    return old;
}

void Process(int pid)
{
    while (1) {
        while (TS(&lock)); // Test.
        Critical Section; // Access.
        lock = false; // Release lock.
        Remainder Section;
    }
    return ;
}
```

- 如果共享资源没有上锁，此时`{C}lock = false`，由`TS()`进行上锁
- 如果共享资源已经上锁，此时`{C}lock = true`，则当前进程保持循环以等待释放锁

### Swap

原子性交换两个变量

```c
bool lock = false; // Shared data.

// Atomically swap two variables.
void Swap (bool *a, bool *b)
{
    bool temp = *a;
    *a = *b;
    *b = temp;
}

void Process(int pid)
{
    while (1) {
        key = true; 
        while (key == true) Swap(&lock, &key);
        Critical Section;
        lock = false;
        Remainder Section;
    }
    return ;
}
```

### Pros && Cons

- 硬件方法的优点
    - 适用于任意数目的进程，在单处理器或多处理器上
    - 简单，容易验证其正确性
    - 可以支持进程内存在多个临界区，只需为每个临界区设立一个布尔变量
- 硬件方法的缺点
    - 等待要耗费CPU时间，**不能实现让权等待**
    - 可能“饥饿”：从等待进程中随机选择一个进入临界区，有的进程可能一直选不上
    - 可能死锁：单CPU情况下,P1执行特殊指令`swap`进入临界区，这时拥有更高优先级P2执行并中断P1,如果P2又要使用P1占用的资源，按照资源分配规则拒绝P2对资源的要求，陷入忙等循环，然后P1也得不到CPU，因为P1比P2优先级低

### Bounded-Waiting Mutual Exclusion With Test-And-Set

为了解决死锁，可以使用这个算法

```c
bool waiting[n]; // Waiting queue.
bool lock = false;

void Process(int pid)
{
    while(1) {
        waiting[pid] = true;
        key = true;
        while (waiting[pid] && key) key = TestAndSet(lock);
        waiting[pid] = false;
        Critical Section;
        int j = (pid + 1) % n;
        while ((j != pid) && !waiting[j]) j = (j + 1) % n;
        if (j == pid) lock = false;
        else waiting[j] = false;
        Remainder Section;
    }
    return ;
}
```

这个算法可以解决所有的临界区问题

### Spinlock

自旋锁是一个与共用数据结构有关的锁定机制，当一个进程想要访问已被其它进程锁定的资源时，进程循环检测该锁是否被释放

自旋锁存储在共用内存中，为了速度和适配于各种体系架构，获取和释放自旋锁的代码是由汇编写成

当线程试图获得自旋锁时，在处理器上所有其它工作终止，因此拥有自旋锁的线程永远不会被抢占，但这个线程可以继续执行以便尽可能快释放锁

内核对使用自旋锁十分小心，当它拥有自旋锁时，它执行的指令数将减至最少

---

## Semaphores

信号量(*Semaphore*)是一种数据类型，表示系统中资源或临界区

信号量不需要繁忙等待

信号量的两个操作，信号量只能通过这两个操作获取

- `{C}wait(S)`，又称`{C}P(S)`，表示申请一个资源
- `{C}signal(S)`，又称`{C}V(S)`，表示释放一个资源

```c
typedef int Semaphore; // Int type semaphore.
Semaphore s;

// Wait for resources.
void wait()
{
    while (s <= 0);
    s--;
}

// Release lock.
void signal()
{
    s++;
}
```

信号量可以分为计数信号量和二值信号量，二值信号量又称互斥锁(*Mutex*)

```c
Semaphore mutex = 1; // Shared data.

void Process(int pid)
{
    do {
        wait(mutex);
        Critical Section;
        signal(mutex);
        Remainder Section;
    } while (true);
    return ;
}
```

**整型信号量可能导致繁忙等待的情况**，因为可能有进程在临界区时间很长，导致其它进程必须等待，**违反了让权等待原则**

基于此，可以引入记录型信号量

```c
typedef struct {
    int value;
    struct process *list; // Pointer to next record in the waiting queue.
} Semaphore;
```

信号量的物理含义：

- `{C}S.value > 0`表示系统有`{C}S.value`个资源可用
- `{C}S.value = 0`表示系统没有资源可用
- `{C}S.value < 0`表示系统有`{C}0 - S.value`个进程在等待队列中等待进入临界区

为了防止繁忙等待的情况，引入两个操作：

- `{C}block()`：把这个进程放在合适的等待队列中
- `{C}wakeup()`：把等待队列中的一个进程移出并放入就绪队列中

```c
void wait(Semaphore *s)
{
    s->value--;
    if (s->value < 0) {
        Add This Process To S->list;
        block();
    }
    return ;
}

void signal(Semaphore *s)
{
    s->value++;
    if (s->value <= 0) {
        Remove A Process P From S->list;
        wakeup(P); 
    }
    return ;
}
```

> [!warning]- 注意
> `wait`和`signal`操作必须成对出现
> - 一般情况下，当为互斥操作时，它们同处于同一进程；当为同步操作时，则不在同一进程中出现
> - **如果两个`wait`操作相邻，那么它们的顺序至关重要，而两个相邻的`signal`操作的顺序无关紧要**
> - 一个同步`wait`操作与一个互斥`wait`操作在一起时， 同步`wait`操作在互斥`wait`操作前

> [!danger] 注意
> `wait`和`signal`使用不当会造成死锁

### Deadlock And Starvation

死锁：两个进程互相等待对方释放资源

饥饿：进程一直被阻塞，当可用资源释放时完成当前进程任务已经无效了

逆反优先级(*Priority Inversion*)：调度问题导致高优先级进程在等待低优先级进程的锁释放

---

## Process Synchronization Problems

### Bounded-Buffer Problem

生产者-消费者问题是最著名的同步问题，描述了一组生产者向一组消费者提供消息，它们共享一个有限缓冲池(*Bounded-Buffer Pool*)，生产者向其中投放消息，消费者从其中取得消息

设总共有`N`个缓冲区，记录几个信号量：

- 互斥信号量`mutex`，初始化为`1`
- 非空缓冲区数量信号量`full`，初始化为`0`
- 空闲缓冲区数量信号量`empty`，初始化为`N`

```c title:"Producer-Consumer"
const int N;
// Shared data.
Semaphore mutex = 1;
Semaphore full = 0;
Semaphore empty = N;

// Producer process.
void Producer()
{
    do {
        Produce an item in nextp;
        ...
        wait(&empty); // Wait for an empty buffer.
        wait(&mutex); // Lock up.
        ...
        Add nextp to buffer
        ... 
        signal(&mutex); // Release lock.
        signal(&full); // Take a buffer.
    } while(1);
    return ;
}

// Consumer process.
void Consumer()
{
    do {
        wait(&full);
        wait(&mutex);
        ...
        Remove an item from buffer to nextc;
        ...
        signal(&mutex);
        signal(&empty);
        ...
        Consumer the item in nextc;
        ...
    } while(1);
    return ;
}
```

### Readers-Writers Problem

读者-写者问题主要描述一组并发的进程

- 读者进程：只读数据
- 写者进程：可以读写

多个读者可以一起读取数据，而一个写者不能和其它进程同时访问数据，它们之间必须互斥

```c title:"Readers-Writers"
// Shared data.
Semaphore mutex = 1; // Mutex for concurrent access to readcount.
Semaphore wrt = 1; // Reader-Writer mutex && Writer-Writer mutex.
int readcount = 0;

// Writer process.
void Writer()
{
    do {
        wait(&wrt);
        Writing;
        signal(&wrt);
    } while(1);
    return ;
}

// Reader process.
void Reader()
{
    do {
        wait(&mutex); // Lock up to increase readcount.
        readcount++;
        // If you are the 1st reader, request to access shared data to prevent writer.
        if (readcount == 1)
            wait(&wrt); // 
        signal(&mutex); // Release lock.
        ...
        Reading;
        ...
        wait(&mutex); // Lock up to decrease readcount.
        readcount--;
        // If you are the last reader, release the write lock.
        if (readcount == 0)
            signal(&wrt);
        signal(&mutex); // Release lock.
    } while(1);
    return ;
}
```

> [!tip] 注意
> 可以看到，上述过程可能导致写者饥饿，因为如果有读者正在读那么写者必须等待最后一个读者释放`wrt`，但是一个读者正在读时可以有其它读者插入，使得读者优先于写者

### Dining-Philosophers Problem

哲学家就餐问题，即避免饥饿问题

> N Philosophers sitting at a round table, each philosopher shares a chopstick with neighbor.  Each philosopher must have both chopsticks to eat. Neighbors can’t eat simultaneously. Philosophers alternate between thinking and eating

```c title:"Dining-Philosophers"
// Shared data.
Semaphore chopstick[5] = {1, 1, 1, 1, 1};

// Philosopher process.
void Philosopher(int i)
{
    do {
        wait(&chopstick[i]);
        wait(&chopstick[(i + 1) % 5]);
        ...
        eat;
        ...
        signal(&chopstick[i]);
        signal(&chopstick[(i + 1) % 5]);
        ...
        think;
        ...
    } while(1);
    return ;
}
```

> [!bug] 问题
> 可能会导致死锁发生，试想一下如果每个哲学家都拿起右边的筷子，则每个人都没有办法拿到左筷子

> [!tip] 解决方法
> - 最多允许`{C}N-1`个哲学家饥饿
> - 只有左右筷子都可用时才拿起，否则一只筷子都不拿
> - 奇数位上的哲学家总是先拿起左筷子，偶数位上的哲学家总是先拿起右筷子
> - 每个哲学家拿起第一根筷子一段时间后，如果拿不到第二根筷子，则放下第一根筷子

---

## Monitors

> [!note] 引入
> 信号量实现复杂，且容易发生死锁现象，因此引入管程，进行信号量的操作封装，相比于信号量更好控制

管程(*Monitors*)是关于共享资源的数据结构及一组针对该资源的操作过程所构成的软件模块，是一种高级同步机制，是管理进程间同步的机制，保证进程互斥地访问共享变量，并方便地阻塞和唤醒进程

管程可以以函数库的形式实现

管程的基本思想是把信号量及其操作封装在一个对象内部，即共享变量及对共享变量能够进行的所有操作集中在一个模块中
