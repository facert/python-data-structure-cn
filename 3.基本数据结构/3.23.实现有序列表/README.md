## 3.23.实现有序列表

为了实现有序列表，我们必须记住项的相对位置是基于一些潜在的特性。上面给出的整数的有序列表`17,26,31,54,77` 和 `93` 可以由 Figure 15 所示的链接结构表示。节点和链接结构表示项的相对位置。

![3.23.实现有序列表.figure15](assets/3.23.%E5%AE%9E%E7%8E%B0%E6%9C%89%E5%BA%8F%E5%88%97%E8%A1%A8.figure15.png)

*Figure 15*

为了实现 OrderedList 类，我们将使用与前面看到的无序列表相同的技术。再次，`head` 的引用为 `None` 表示为空链表（参见 Listing 8）。

```python
class OrderedList:
    def __init__(self):
        self.head = None
```

*Listing 8*

当我们考虑有序列表的操作时，我们应该注意，`isEmpty` 和`size` 方法可以与无序列表一样实现，因为它们只处理链表中的节点数量，而不考虑实际项值。同样，`remove` 方法将正常工作，因为我们仍然需要找到该项，然后删除它。剩下的两个方法，`search` 和 `add`，将需要一些修改。

搜索无序列表需要我们一次遍历一个节点，直到找到我们正在寻找的节点或者没找到节点（None）。事实证明，相同的方法在有序列表也有效。然而，在项不在链表中的情况下，我们可以利用该顺序来尽快停止搜索。

例如，Figure 16 展示了有序链表搜索值 45 。从链表的头部开始遍历，首先与 `17` 进行比较。由于 `17` 不是我们正在寻找的项，移动到下一个节点 `26` 。再次，这不是我们想要的，继续到 `31`，然后再到 `54` 。在这一点上，有一些不同。由于 `54` 不是我们正在寻找的项，我们以前的方法是继续向前迭代。然而，由于这是有序列表，一旦节点中的值变得大于我们正在搜索的项，搜索就可以停止并返回 False 。该项不可能存在于后面的链表中。

![3.23.实现有序列表.figure16](assets/3.23.%E5%AE%9E%E7%8E%B0%E6%9C%89%E5%BA%8F%E5%88%97%E8%A1%A8.figure16.png)

*Figure 16*

Listing 9 展示了完整的搜索方法。通过添加另一个布尔变量 `stop` 并将其初始化为 `False`（第4行），很容易合并上述新条件。 当 `stop` 是`False`（不停止）时，我们可以继续在列表中前进（第5行）。如果发现任何节点包含大于我们正在寻找的项的数据，我们将 `stop` 设置为 `True`（第9-10行）。其余行与无序列表搜索相同。

```python
def search(self,item):
    current = self.head
    found = False
    stop = False
    while current != None and not found and not stop:
        if current.getData() == item:
            found = True
        else:
            if current.getData() > item:
                stop = True
            else:
                current = current.getNext()

    return found
```

*Listing 9*

最重要的需要修改的方法是 `add`。 回想一下，对于无序列表，`add` 方法可以简单地将新节点放置在链表的头部。 这是最简单的访问点。 不幸的是，这将不再适用于有序列表。需要在现有的有序列表中查找新项所属的特定位置。

假设我们有由 `17,26,54,77` 和 `93` 组成的有序列表，并且我们要添加值`31` 。 `add` 方法必须确定新项属于 `26` 到 `54` 之间。Figure 17 展示了我们需要的设置。正如我们前面解释的，我们需要遍历链表，寻找添加新节点的地方。我们知道，当我们迭代完节点（ `current` 变为 None）或 `current` 节点的值变得大于我们希望添加的项时，我们就找到了该位置。在我们的例子中，看到值 `54` 我们停止迭代。

![3.23.实现有序列表.figure17](assets/3.23.%E5%AE%9E%E7%8E%B0%E6%9C%89%E5%BA%8F%E5%88%97%E8%A1%A8.figure17.png)

*Figure 17*

正如我们看到的无序列表，有必要有一个额外的引用，再次称为 `previous`，因为 `current` 不会提供对修改的节点的访问。 Listing 10 展示了完整的`add` 方法。 行 2-3 设置两个外部引用，行 9-10 允许 `previous` 每次通过迭代跟随 `current` 节点后面 。 条件（行5）允许迭代继续，只要有更多的节点，并且当前节点中的值不大于该项。 在任一种情况下，当迭代失败时，我们找到了新节点的位置。

该方法的其余部分完成 Figure17 所示的两步过程。一旦为该项创建了新节点，剩下的唯一问题是新节点是否将被添加在链表的开始处或某个中间位置。再次，`previous == None`（第13行）可以用来提供答案。

```python
def add(self,item):
    current = self.head
    previous = None
    stop = False
    while current != None and not stop:
        if current.getData() > item:
            stop = True
        else:
            previous = current
            current = current.getNext()

    temp = Node(item)
    if previous == None:
        temp.setNext(self.head)
        self.head = temp
    else:
        temp.setNext(current)
        previous.setNext(temp)
```

*Listing 10*

### 3.23.1.链表分析

为了分析链表操作的复杂性，我们需要考虑它们是否需要遍历。考虑具有 n 个节点的链表。 `isEmpty` 方法是 $$O(1)$$，因为它需要一个步骤来检查头的引用为 `None`。另一方面，`size` 将总是需要 n 个步骤，因为不从头到尾地移动没法知道有多少节点在链表中。因此，长度为 $$O(n)$$。将项添加到无序列表始终是 $$O(1)$$，因为我们只是将新节点放置在链表的头部。但是，搜索和删除，以及添加有序列表，都需要遍历过程。虽然平均他们可能只需要遍历节点的一半，这些方法都是 $$O(n)$$，因为在最坏的情况下，都将处理列表中的每个节点。

你可能还注意到此实现的性能与早前针对 Python 列表给出的实际性能不同。这表明链表不是 Python 列表的实现方式。 Python 列表的实际实现基于数组的概念。我们在第 8 章中更详细地讨论这个问题。
