# Containers In STL

> [!abstract] 本节摘要
> C++标准模板库中一些实用的容器模板

## Introduction[^1]

> [!quote] 什么是容器
> The Containers library is a generic collection of class templates and algorithms that allow programmers to easily implement common data structures like queues, lists and stacks.

容器是用于存储和管理数据的类模板，STL提供的容器大致分为三类：

- 序列容器
- 关联容器
- 无序容器

## Sequence Container

序列容器是元素按照线性顺序存储的容器

### Vector

```cpp title:"vector"
template<
    class T,
    class Allocator = std::allocator<T>
> class vector;
```

`{cpp}std::vector` 是封装动态数组的序列容器，元素被连续存储，可以通过迭代器和指向元素的常规指针进行访问

`{cpp}std::vector` 的存储是自动管理的，按需扩样收缩，通常占用多于静态数组的空间，因为要分配更多内存用于管理将来的增长，而内存只在额外空间耗尽时进行重分配，使用 `{cpp}capacity()` 查询分配的内存总量，可以使用 `{cpp}shrink_to_fit()` 把多于的内存返回给系统

如果元素数量已知，可以使用 `{cpp}reserve()` 消除内存重分配

容器复杂度特性：

- 随机访问： $O(1)$
- 在末尾插入或移除元素： $O(1)$
- 插入或移除元素： $O(n)$

|          成员函数           |      含义说明      |
| :---------------------: | :------------: |
|        **元素访问**         |      ...       |
|        `{cpp}at`        |  带越界检查访问指定元素   |
|    `{cpp}operator[]`    |     访问指定元素     |
|      `{cpp}front`       |     第一个元素      |
|       `{cpp}back`       |     最后一个元素     |
|         **迭代器**         |      ...       |
|      `{cpp}begin`       |    指向起始的迭代器    |
|       `{cpp}end`        |    指向末尾的迭代器    |
|      `{cpp}rbegin`      |   指向起始的逆向迭代器   |
|       `{cpp}rend`       |   指向末尾的逆向迭代器   |
|         **容量**          |      ...       |
|      `{cpp}empty`       |      是否为空      |
|       `{cpp}size`       |      元素数量      |
|     `{cpp}max_size`     |   可容纳的最大元素数    |
|     `{cpp}reserve`      |     预留存储空间     |
|     `{cpp}capacity`     | 当前存储空间能够容纳的元素数 |
|         **修改器**         |      ...       |
|      `{cpp}clear`       |      清除内容      |
|      `{cpp}insert`      |      插入元素      |
|   `{cpp}emplace`[^1]    |     原位构造元素     |
|      `{cpp}erase`       |      擦除元素      |
|    `{cpp}push_back`     |     末尾添加元素     |
| `{cpp}emplace_back`[^1] |    末尾原位构造元素    |
|     `{cpp}pop_back`     |     移除末尾元素     |
|      `{cpp}resize`      |    改变存储元素个数    |

### Array[^1]

固定大小的原位连续数组

TODO

### Deque

双端队列

TODO

### List

双向链表

TODO

### Forward_list[^1]

单向链表

TODO

## Associative Container

关联容器是元素按照键值对存储的有序容器，支持 $O(\log n)$ 级别复杂度的快速查找

### Map

```cpp title:"map"
template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class map;
```

`{cpp}std::map` 是一种有序关联容器，包含**唯一的键值对**，键之间使用比较函数 `{cpp}Compare` 排序

`{cpp}std::map` 的底层实现通常为[红黑树](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)

`{cpp}std::map` 的迭代器以**升序**迭代各键

容器复杂度特性：搜索、移除、插入操作的复杂度均为 $O(\log n)$

|          成员函数          |        含义说明        |
| :--------------------: | :----------------: |
|        **元素访问**        |        ...         |
|       `{cpp}at`        |     带越界检查访问元素      |
|   `{cpp}operator[]`    |      访问或插入元素       |
|        **迭代器**         |        ...         |
|      `{cpp}begin`      |      指向起始的迭代器      |
|       `{cpp}end`       |      指向末尾的迭代器      |
|     `{cpp}rbegin`      |     指向起始的逆向迭代器     |
|      `{cpp}rend`       |     指向末尾的逆向迭代器     |
|         **容量**         |        ...         |
|      `{cpp}empty`      |      检查元素是否为空      |
|      `{cpp}size`       |        元素数         |
|    `{cpp}max_size`     |     可容纳的最大元素数      |
|        **修改器**         |        ...         |
|      `{cpp}clear`      |        清除内容        |
|     `{cpp}insert`      |        插入元素        |
|   `{cpp}emplace`[^1]   |       原位构造元素       |
| `{cpp}try_emplace`[^2] | 若键不存在则原位插入，否则不做任何事 |
|      `{cpp}erase`      |        擦除元素        |
|    `{cpp}merge`[^2]    |     从另一容器合并节点      |
|         **查找**         |        ...         |
|      `{cpp}count`      |     匹配特定键的元素数量     |
|      `{cpp}find`       |     寻找带有特定键的元素     |
|   `{cpp}lower_bound`   |  返回首个不小于给定键元素的迭代器  |
|   `{cpp}upper_bound`   |  返回首个大于给定键元素的迭代器   |

### Set

```cpp title:"set"
template<
    class Key,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<Key>
> class set;
```

`{cpp}std::set` 是一种关联容器，含有 `{cpp}Key` 类型对象的已排序集，使用比较函数 `{cpp}Compare` 进行

`{cpp}std::set` 的底层实现通常为[红黑树](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)

容器复杂度特性：搜索、移除、插入操作的复杂度均为 $O(\log n)$

|        成员函数        |       含义说明       |
| :----------------: | :--------------: |
|      **迭代器**       |       ...        |
|    `{cpp}begin`    |     指向起始的迭代器     |
|     `{cpp}end`     |     指向末尾的迭代器     |
|   `{cpp}rbegin`    |    指向起始的逆向迭代器    |
|    `{cpp}rend`     |    指向末尾的逆向迭代器    |
|       **容量**       |       ...        |
|    `{cpp}empty`    |     检查元素是否为空     |
|    `{cpp}size`     |       元素数        |
|  `{cpp}max_size`   |    可容纳的最大元素数     |
|      **修改器**       |       ...        |
|    `{cpp}clear`    |       清除内容       |
|   `{cpp}insert`    |       插入元素       |
| `{cpp}emplace`[^1] |      原位构造元素      |
|    `{cpp}erase`    |       擦除元素       |
|  `{cpp}merge`[^2]  |    从另一容器合并节点     |
|       **查找**       |       ...        |
|    `{cpp}count`    |    匹配特定键的元素数量    |
|    `{cpp}find`     |    寻找带有特定键的元素    |
| `{cpp}lower_bound` | 返回首个不小于给定键元素的迭代器 |
| `{cpp}upper_bound` | 返回首个大于给定键元素的迭代器  |

### Multimap

```cpp title:"map"
template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class multimap;
```

`{cpp}std::multimap` 是一种有序关联容器，含有键值对的已排序列表同时容许多个元素拥有同一键，键之间使用比较函数 `{cpp}Compare` 排序

`{cpp}std::multimap` 的迭代器以**键的非降序**进行迭代

拥有等价键的键值对的顺序就是插入顺序，且不会更改[^1]

容器复杂度特性：搜索、移除、插入操作的复杂度均为 $O(\log n)$

|        成员函数        |       含义说明       |
| :----------------: | :--------------: |
|      **迭代器**       |       ...        |
|    `{cpp}begin`    |     指向起始的迭代器     |
|     `{cpp}end`     |     指向末尾的迭代器     |
|   `{cpp}rbegin`    |    指向起始的逆向迭代器    |
|    `{cpp}rend`     |    指向末尾的逆向迭代器    |
|       **容量**       |       ...        |
|    `{cpp}empty`    |     检查元素是否为空     |
|    `{cpp}size`     |       元素数        |
|  `{cpp}max_size`   |    可容纳的最大元素数     |
|      **修改器**       |       ...        |
|    `{cpp}clear`    |       清除内容       |
|   `{cpp}insert`    |       插入元素       |
| `{cpp}emplace`[^1] |      原位构造元素      |
|    `{cpp}erase`    |       擦除元素       |
|  `{cpp}merge`[^2]  |    从另一容器合并节点     |
|       **查找**       |       ...        |
|    `{cpp}count`    |    匹配特定键的元素数量    |
|    `{cpp}find`     |    寻找带有特定键的元素    |
| `{cpp}lower_bound` | 返回首个不小于给定键元素的迭代器 |
| `{cpp}upper_bound` | 返回首个大于给定键元素的迭代器  |

### Multiset

```cpp title:"set"
template<
    class Key,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<Key>
> class multiset;
```

`{cpp}std::multiset` 是一种关联容器，含有 `{cpp}Key` 类型对象的有序集，允许多个 `{cpp}Key` 拥有等价的值，使用比较函数 `{cpp}Compare` 进行

比较等价元素间的顺序是插入顺序，而且不会更改[^1]

容器复杂度特性：搜索、移除、插入操作的复杂度均为 $O(\log n)$

|        成员函数        |       含义说明       |
| :----------------: | :--------------: |
|      **迭代器**       |       ...        |
|    `{cpp}begin`    |     指向起始的迭代器     |
|     `{cpp}end`     |     指向末尾的迭代器     |
|   `{cpp}rbegin`    |    指向起始的逆向迭代器    |
|    `{cpp}rend`     |    指向末尾的逆向迭代器    |
|       **容量**       |       ...        |
|    `{cpp}empty`    |     检查元素是否为空     |
|    `{cpp}size`     |       元素数        |
|  `{cpp}max_size`   |    可容纳的最大元素数     |
|      **修改器**       |       ...        |
|    `{cpp}clear`    |       清除内容       |
|   `{cpp}insert`    |       插入元素       |
| `{cpp}emplace`[^1] |      原位构造元素      |
|    `{cpp}erase`    |       擦除元素       |
|  `{cpp}merge`[^2]  |    从另一容器合并节点     |
|       **查找**       |       ...        |
|    `{cpp}count`    |    匹配特定键的元素数量    |
|    `{cpp}find`     |    寻找带有特定键的元素    |
| `{cpp}lower_bound` | 返回首个不小于给定键元素的迭代器 |
| `{cpp}upper_bound` | 返回首个大于给定键元素的迭代器  |

## Unordered Associative Container

无序关联容器，支持平均 $O(1)$，最坏情况 $O(n)$ 级别复杂度的无序数据结构

无序关联容器都是基于哈希表实现

### Unordered_map[^1]

```cpp title:"unordered_map"
template<
    class Key,
    class T,
    class Hash = std::hash<Key>,
    class KeyEqual = std::qeual_to<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class unordered_map;
```

`{cpp}std::unordered_map` 是一种关联容器，包含**唯一的键值对**

元素在容器内部不以任何特定顺序排序，而是组织进桶中，想通散列码的键出现在同一个桶

容器复杂度特性：搜索、移除、插入操作的平均复杂度均为 $O(1)$

|          成员函数          |        含义说明        |
| :--------------------: | :----------------: |
|        **元素访问**        |        ...         |
|       `{cpp}at`        |     带越界检查访问元素      |
|   `{cpp}operator[]`    |      访问或插入元素       |
|        **迭代器**         |        ...         |
|      `{cpp}begin`      |      指向起始的迭代器      |
|       `{cpp}end`       |      指向末尾的迭代器      |
|         **容量**         |        ...         |
|      `{cpp}empty`      |      检查元素是否为空      |
|      `{cpp}size`       |        元素数         |
|    `{cpp}max_size`     |     可容纳的最大元素数      |
|        **修改器**         |        ...         |
|      `{cpp}clear`      |        清除内容        |
|     `{cpp}insert`      |        插入元素        |
|   `{cpp}emplace`[^1]   |       原位构造元素       |
| `{cpp}try_emplace`[^2] | 若键不存在则原位插入，否则不做任何事 |
|      `{cpp}erase`      |        擦除元素        |
|    `{cpp}merge`[^2]    |     从另一容器合并节点      |
|         **查找**         |        ...         |
|      `{cpp}count`      |     匹配特定键的元素数量     |
|      `{cpp}find`       |     寻找带有特定键的元素     |

### Unordered_set[^1]

```cpp title:"unordered_set"
template<
    class Key,
    class Hash = std::hash<Key>,
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator<Key>
> class unordered_set;
```

`{cpp}std::unordered_set` 是一种关联容器，包含 `{cpp}Key` 类型的唯一对象集合

元素在容器内部不以任何特定顺序排序，而是组织进桶中，想通散列码的键出现在同一个桶

> [!danger] 注意
> 不可修改容器元素，因为修改可能改变元素散列并破坏容器

容器复杂度特性：搜索、移除、插入操作的平均复杂度均为 $O(1)$

|        成员函数        |       含义说明       |
| :----------------: | :--------------: |
|      **迭代器**       |       ...        |
|    `{cpp}begin`    |     指向起始的迭代器     |
|     `{cpp}end`     |     指向末尾的迭代器     |
|       **容量**       |       ...        |
|    `{cpp}empty`    |     检查元素是否为空     |
|    `{cpp}size`     |       元素数        |
|  `{cpp}max_size`   |    可容纳的最大元素数     |
|      **修改器**       |       ...        |
|    `{cpp}clear`    |       清除内容       |
|   `{cpp}insert`    |       插入元素       |
| `{cpp}emplace`[^1] |      原位构造元素      |
|    `{cpp}erase`    |       擦除元素       |
|  `{cpp}merge`[^2]  |    从另一容器合并节点     |
|       **查找**       |       ...        |
|    `{cpp}count`    |    匹配特定键的元素数量    |
|    `{cpp}find`     |    寻找带有特定键的元素    |

### Unordered_multimap[^1]

```cpp title:"unordered_map"
template<
    class Key,
    class T,
    class Hash = std::hash<Key>,
    class KeyEqual = std::qeual_to<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class unordered_multimap;
```

`{cpp}std::unordered_multimap` 是一种无序关联容器，支持等价键并将键与另一类型值关联，每个键值可能有多个副本

元素在容器内部不以任何特定顺序排序，而是组织进桶中，想通散列码的键出现在同一个桶

容器复杂度特性：搜索、移除、插入操作的平均复杂度均为 $O(1)$

|          成员函数          |        含义说明        |
| :--------------------: | :----------------: |
|        **迭代器**         |        ...         |
|      `{cpp}begin`      |      指向起始的迭代器      |
|       `{cpp}end`       |      指向末尾的迭代器      |
|         **容量**         |        ...         |
|      `{cpp}empty`      |      检查元素是否为空      |
|      `{cpp}size`       |        元素数         |
|    `{cpp}max_size`     |     可容纳的最大元素数      |
|        **修改器**         |        ...         |
|      `{cpp}clear`      |        清除内容        |
|     `{cpp}insert`      |        插入元素        |
|   `{cpp}emplace`[^1]   |       原位构造元素       |
|      `{cpp}erase`      |        擦除元素        |
|    `{cpp}merge`[^2]    |     从另一容器合并节点      |
|         **查找**         |        ...         |
|      `{cpp}count`      |     匹配特定键的元素数量     |
|      `{cpp}find`       |     寻找带有特定键的元素     |

### Unordered_set[^1]

```cpp title:"unordered_set"
template<
    class Key,
    class Hash = std::hash<Key>,
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator<Key>
> class unordered_multiset;
```

`{cpp}std::unordered_multiset` 是一种关联容器，包含可能非唯一的 `{cpp}Key` 类型的对象集合

元素在容器内部不以任何特定顺序排序，而是组织进桶中，想通散列码的键出现在同一个桶

容器复杂度特性：搜索、移除、插入操作的平均复杂度均为 $O(1)$

|        成员函数        |    含义说明    |
| :----------------: | :--------: |
|      **迭代器**       |    ...     |
|    `{cpp}begin`    |  指向起始的迭代器  |
|     `{cpp}end`     |  指向末尾的迭代器  |
|       **容量**       |    ...     |
|    `{cpp}empty`    |  检查元素是否为空  |
|    `{cpp}size`     |    元素数     |
|  `{cpp}max_size`   | 可容纳的最大元素数  |
|      **修改器**       |    ...     |
|    `{cpp}clear`    |    清除内容    |
|   `{cpp}insert`    |    插入元素    |
| `{cpp}emplace`[^1] |   原位构造元素   |
|    `{cpp}erase`    |    擦除元素    |
|  `{cpp}merge`[^2]  | 从另一容器合并节点  |
|       **查找**       |    ...     |
|    `{cpp}count`    | 匹配特定键的元素数量 |
|    `{cpp}find`     | 寻找带有特定键的元素 |

## Container Adapters

容器适配器为顺序容器提供了不同接口

### Stack

```cpp title:"stack"
template<
    class T,
    class Container = std::deque<T>
> class stack;
```

`{cpp}std::stack` 是一种容器适配器，提供栈的功能

|        成员函数        |   含义说明   |
| :----------------: | :------: |
|      **元素访问**      |   ...    |
|     `{cpp}top`     |  访问栈顶元素  |
|       **容量**       |   ...    |
|    `{cpp}empty`    |  容器是否为空  |
|    `{cpp}size`     |   元素数量   |
|      **修改器**       |   ...    |
|    `{cpp}push`     |  栈顶插入元素  |
| `{cpp}emplace`[^1] | 栈顶原位构造元素 |
|     `{cpp}pop`     |  移除栈顶元素  |

### Queue

```cpp title:"queue"
template<
    class T,
    class Container = std::deque<T>
> class queue;
```

`{cpp}std::queue` 是一种容器适配器，提供队列的功能

|        成员函数        |   含义说明   |
| :----------------: | :------: |
|      **元素访问**      |   ...    |
|     `{cpp}front`     |  访问第一个元素  |
|     `{cpp}back`     |  访问最后一个元素  |
|       **容量**       |   ...    |
|    `{cpp}empty`    |  容器是否为空  |
|    `{cpp}size`     |   元素数量   |
|      **修改器**       |   ...    |
|    `{cpp}push`     |  栈顶插入元素  |
| `{cpp}emplace`[^1] | 栈顶原位构造元素 |
|     `{cpp}pop`     |  移除栈顶元素  |

### Priority_queue

```cpp title:"queue"
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```

`{cpp}std::priority_queue` 是一种容器适配器，提供优先队列的功能

适配器特性：

- 查找：默认查找最大元素，复杂度为 $O(1)$
- 插入：复杂度为 $O(\log n)$

> [!tip] 最大堆与最小堆
> 可以使用 `{cpp}std::greater<T>` 使得优先队列变为最小堆

|        成员函数        |   含义说明   |
| :----------------: | :------: |
|      **元素访问**      |   ...    |
|     `{cpp}top`     |  访问栈顶元素  |
|       **容量**       |   ...    |
|    `{cpp}empty`    |  容器是否为空  |
|    `{cpp}size`     |   元素数量   |
|      **修改器**       |   ...    |
|    `{cpp}push`     |  栈顶插入元素  |
| `{cpp}emplace`[^1] | 栈顶原位构造元素 |
|     `{cpp}pop`     |  移除栈顶元素  |

## References

- [Containers Library - cppreference.com](https://en.cppreference.com/w/cpp/container)

[^1]: C++11

[^2]: C++17
