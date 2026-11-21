# C++ STL 攻略

C++ 标准模板库（Standard Template Library，STL）是一套功能强大的 C++ 模板类和函数的集合，它提供了一系列通用的、可复用的算法和数据结构。

STL 的设计基于泛型编程，这意味着使用模板可以编写出独立于任何特定数据类型的代码。

STL 分为多个组件，包括容器（Containers）、迭代器（Iterators）、算法（Algorithms）、仿函数（Functors）、适配器（Adapters）和分配器（Allocators）。

使用 STL 的好处:

* **代码复用** ：STL 提供了大量的通用数据结构和算法，可以减少重复编写代码的工作。
* **性能优化** ：STL 中的算法和数据结构都经过了优化，以提供最佳的性能。
* **泛型编程** ：使用模板，STL 支持泛型编程，使得算法和数据结构可以适用于任何数据类型。
* **易于维护** ：STL 的设计使得代码更加模块化，易于阅读和维护。

---
## 容器

容器是 STL 中最基本的组件之一，提供了各种数据结构，包括向量（vector）、链表（list）、队列（queue）、栈（stack）、集合（set）、映射（map）等。这些容器具有不同的特性和用途，可以根据实际需求选择合适的容器。

STL 中的容器可以分为三类：

1、序列容器：存储元素的序列，允许双向遍历。

* std::**vector** ：动态数组，支持快速随机访问，在尾部插入/删除元素非常快。
* std::**deque** ：双端队列，支持随机访问，在头部和尾部插入/删除都非常快。
* std::**list** ：双向链表，在任何位置插入和删除元素都非常快（只要拿到位置指针），不支持随机访问。
* std::**array** ：静态数组，固定大小的连续内存（C++11 引入）。

2、关联容器：元素在插入时，会根据特定的规则自动排序。底层通常由红黑树（一种自平衡二叉查找树）实现。它们不支持像数组那样的 v[i] 索引访问，而是通过 key（键）来查找。

* std::**set** / std::**multiset** ：集合，只存key，且自动去重并升序排列。multiset允许有重复元素。
* std::**map** / std::**multimap** ：映射/字典，存储键值对（ key - value ），key唯一且自动排序。multimap允许key重复。

3、无序容器（C++11 引入）：底层由哈希表实现，支持快速的查找、插入和删除。缺点：元素不会自动排序，遍历顺序是随机的（取决于哈希函数）。

* std::**unordered_set** ：无序集合。
* std::**unordered_multiset** ：无序多重集合。
* std::**unordered_map** ：无序映射。
* std::**unordered_multimap** ：无序多重映射。

---
### vector 动态数组

vector 是一个序列容器，用于存储动态大小的数组，它允许用户在容器的末尾快速地添加或删除元素。与数组相比，vector 提供了更多的功能，如自动调整大小、随机访问等。

#### 声明

`#include <vector>`

#### 基本语法

1.创建一个 vector ：`std::vector<元素类型> 名称(长度, 初值);`

    时间复杂度：O(n)

``` cpp linenums="1"
using namespace std;

vector<int> arr1;   // 创建初始为空的int数组
vector<int> arr2(67);   // 创建初始长度为67的int数组，所有元素初值为默认值0
vector<int> arr3(67, 1);   // 创建初始长度为67的int数组，所有元素初值为1

// 创建初始长度为67的二维数组，每行初始为空
vector<vector<int>> mat1(67, vector<int> ());

// 创建初始长度为67、宽度为325的二维数组，所有元素初值为1
vector<vector<int>> mat2(67, vector<int> (325, 1));
```

2.末尾添加元素：`名称.push_back(元素);`

    时间复杂度：均摊O(1)

``` cpp linenums="1"
using namespace std;

vector<int> arr;
arr.push_back(67);   // 向arr末尾添加元素67，若arr容量不足则自动扩容
```

3.访问元素：`名称[下标];`

    时间复杂度：O(1)

``` cpp linenums="1"
using namespace std;

vector<int> arr = {1, 2, 3, 4};   // 使用列表初始化（C++11引入）
int num = arr[0];   // 访问下标为0的元素并赋值给num
```

4.获取长度：`名称.size();`

    时间复杂度：O(1)

``` cpp linenums="1"
using namespace std;

vector<int> arr(6, 7);

// 声明一个size_t类型（表示长度的无符号整型）的变量vsize，并用arr.size()的返回值给它赋值
size_t vsize = arr.size();    // vsize = 6
```

5.清空 vector ：`名称.clear();`

    时间复杂度：O(n)

``` cpp linenums="1"
using namespace std;

vector<int> arr(6, 7);
arr.clear();    // 清空arr
```

#### 常用成员函数

| 函数 | 说明 | 算竞 |
| ---- | ---- | ---- |
| push_back(val)  | 在末尾添加元素 |
| pop_back() | 删除末尾元素 |
| vector变量名[pos] | 返回指定位置的元素（不带边界检查） |
| at(pos) | 返回指定位置的元素（带边界检查） | 基本不用 |
| front() | 返回第一个元素 |
| back() | 返回最后一个元素 |
| data() | 返回指向底层数组的指针 | 基本不用 |
| size() | 返回当前元素数量 |
| capacity() | 返回当前分配的容量 | 基本不用 |
| reserve(n) | 预留至少n个元素的存储空间 | 基本不用 |
| resize(n, val) | 将长度调整为n，若是扩大，则将新元素赋值为val |
| clear() | 清空所有元素 |
| insert(pos, val) | 在指定位置插入元素 | 少用 |
| erase(pos) | 删除指定位置的元素 | 少用 |
| begin() / end() | 返回起始/结束迭代器 |

vector 是 C++ STL 中一个非常有用的容器，它提供了动态数组的功能，使得元素的添加和删除变得更加灵活和方便。随着学习的深入，你将发现 vector 在实际编程中的强大功能和广泛应用。