## 3.16.Deque抽象数据类型

deque 抽象数据类型由以下结构和操作定义。如上所述，deque 被构造为项的有序集合，其中项从首部或尾部的任一端添加和移除。下面给出了 deque 操作。

* Deque() 创建一个空的新 deque。它不需要参数，并返回空的 deque。
* addFront(item) 将一个新项添加到 deque 的首部。它需要 item 参数 并不返回任何内容。
* addRear(item) 将一个新项添加到 deque 的尾部。它需要 item 参数并不返回任何内容。
* removeFront() 从 deque 中删除首项。它不需要参数并返回 item。deque 被修改。
* removeRear() 从 deque 中删除尾项。它不需要参数并返回 item。deque 被修改。
* isEmpty() 测试 deque 是否为空。它不需要参数，并返回布尔值。
* size() 返回 deque 中的项数。它不需要参数，并返回一个整数。

例如，我们假设 d 是已经创建并且当前为空的 deque，则 Table 1 展示了一系列 deque 操作的结果。注意，首部的内容列在右边。在将 item 移入和移出时，跟踪前面和后面是非常重要的，因为可能会有点混乱。

![3.16.Deque抽象数据类型.table1](assets/3.16.Deque%E6%8A%BD%E8%B1%A1%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.table1.png)

*Table 1*
