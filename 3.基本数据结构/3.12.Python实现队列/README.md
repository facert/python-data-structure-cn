## 3.12.Python实现队列
我们为了实现队列抽象数据类型创建一个新类。和前面一样，我们将使用列表集合来作为构建队列的内部表示。

我们需要确定列表的哪一端作为队首，哪一端作为队尾。Listing 1 所示的实现假定队尾在列表中的位置为 0。这允许我们使用列表上的插入函数向队尾添加新元素。弹出操作可用于删除队首的元素（列表的最后一个元素）。回想一下，这也意味着入队为 $$O(n)$$，出队为 $$O(1)$$。

```python
class Queue:
    def __init__(self):
        self.items = []

    def isEmpty(self):
        return self.items == []

    def enqueue(self, item):
        self.items.insert(0,item)

    def dequeue(self):
        return self.items.pop()

    def size(self):
        return len(self.items)
```

*Listing 1*

进一步的操作这个队列产生如下结果：

```python
>>> q.size()
3
>>> q.isEmpty()
False
>>> q.enqueue(8.4)
>>> q.dequeue()
4
>>> q.dequeue()
'dog'
>>> q.size()
2
```
