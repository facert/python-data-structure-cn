## 3.17.Python实现Deque

正如我们在前面的部分中所做的，我们将为抽象数据类型 deque 的实现创建一个新类。同样，Python 列表将提供一组非常好的方法来构建 deque 的细节。我们的实现（Listing 1）将假定 deque 的尾部在列表中的位置为 0。

```python
class Deque:
    def __init__(self):
        self.items = []

    def isEmpty(self):
        return self.items == []

    def addFront(self, item):
        self.items.append(item)

    def addRear(self, item):
        self.items.insert(0,item)

    def removeFront(self):
        return self.items.pop()

    def removeRear(self):
        return self.items.pop(0)

    def size(self):
        return len(self.items)
```

*Listing 1*

在 removeFront 中，我们使用 pop 方法从列表中删除最后一个元素。 但是，在removeRear中，pop(0)方法必须删除列表的第一个元素。同样，我们需要在 addRear 中使用insert方法（第12行），因为 append 方法在列表的末尾添加一个新元素。

你可以看到许多与栈和队列中描述的 Python 代码相似之处。你也可能观察到，在这个实现中，从前面添加和删除项是 $$O(1)$$，而从后面添加和删除是 $$O(n)$$。 考虑到添加和删除项是出现的常见操作，这是可预期的。 同样，重要的是要确定我们知道在实现中前后都分配在哪里。
