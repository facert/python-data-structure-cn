## 6.9.二叉堆操作

我们的二叉堆实现的基本操作如下：

* BinaryHeap() 创建一个新的，空的二叉堆。
* insert(k) 向堆添加一个新项。
* findMin() 返回具有最小键值的项，并将项留在堆中。
* delMin() 返回具有最小键值的项，从堆中删除该项。
* 如果堆是空的，isEmpty() 返回 true，否则返回 false。
* size() 返回堆中的项数。
* buildHeap(list) 从键列表构建一个新的堆。

ActiveCode 1 展示了使用一些二叉堆方法。注意，无论我们向堆中添加项的顺序是什么，每次都删除最小的。我们现在将把注意力转向如何实现这个想法。

```python
from pythonds.trees.binheap import BinHeap

bh = BinHeap()
bh.insert(5)
bh.insert(7)
bh.insert(3)
bh.insert(11)

print(bh.delMin())

print(bh.delMin())

print(bh.delMin())

print(bh.delMin())
```
