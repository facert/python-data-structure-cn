## 6.10.二叉堆实现

### 6.10.1.结构属性

为了使我们的堆有效地工作，我们将利用二叉树的对数性质来表示我们的堆。 为了保证对数性能，我们必须保持树平衡。平衡二叉树在根的左和右子树中具有大致相同数量的节点。 在我们的堆实现中，我们通过创建一个 `完整二叉树` 来保持树平衡。 一个完整的二叉树是一个树，其中每个层都有其所有的节点，除了树的最底层，从左到右填充。 Figure 1 展示了完整二叉树的示例。

![6.10.二叉堆实现.figure1](assets/6.10.%E4%BA%8C%E5%8F%89%E5%A0%86%E5%AE%9E%E7%8E%B0.figure1.png)

*Figure 1*

完整二叉树的另一个有趣的属性是，我们可以使用单个列表来表示它。 我们不需要使用节点和引用，甚至列表的列表。因为树是完整的，父节点的左子节点（在位置 p 处）是在列表中位置 2p 中找到的节点。 类似地，父节点的右子节点在列表中的位置 2p + 1。为了找到树中任意节点的父节点，我们可以简单地使用Python 的整数除法。 假定节点在列表中的位置 n，则父节点在位置 n/2。 Figure 2 展示了一个完整的二叉树，并给出了树的列表表示。 请注意父级和子级之间是 2p 和 2p+1 关系。 树的列表表示以及完整的结构属性允许我们仅使用几个简单的数学运算来高效地遍历一个完整的二叉树。 我们将看到，这也是我们的二叉堆的有效实现。

### 6.10.2.堆的排序属性

我们用于堆中存储项的方法依赖于维护堆的排序属性。 堆的排序属性如下：在堆中，对于具有父 p 的每个节点 x，p 中的键小于或等于 x 中的键。 Figure 2 展示了具有堆顺序属性的完整二叉树。

![6.10.二叉堆实现.figure2](assets/6.10.%E4%BA%8C%E5%8F%89%E5%A0%86%E5%AE%9E%E7%8E%B0.figure2.png)

*Figure 2*

### 6.10.3.堆操作

我们将开始实现一个二叉堆的构造函数。由于整个二叉堆可以由单个列表表示，所以构造函数将初始化列表和一个 `currentSize` 属性来跟踪堆的当前大小。 Listing 1 展示了构造函数的 Python 代码。 你会注意到，一个空的二叉堆有一个单一的零作为 `heapList` 的第一个元素，这个零只是放那里，用于以后简单的整数除法。

```python
class BinHeap:
    def __init__(self):
        self.heapList = [0]
        self.currentSize = 0
```

*Listing 1*

我们将实现的下一个方法是 `insert` 。 将项添加到列表中最简单，最有效的方法是将项附加到列表的末尾。 它维护完整的树属性。但可能违反堆结构属性。可以编写一个方法，通过比较新添加的项与其父项，我们可以重新获得堆结构属性。 如果新添加的项小于其父项，则我们可以将项与其父项交换。 Figure 2 展示了将新添加的项替换到其在树中的适当位置所需的操作。

![6.10.二叉堆实现.figure2-1](assets/6.10.%E4%BA%8C%E5%8F%89%E5%A0%86%E5%AE%9E%E7%8E%B0.figure2-1.png)

*Figure 2*

注意，当我们完成一个项时，我们需要恢复新添加的项和父项之间的堆属性。 我们还需保留任何兄弟节点的堆属性。当然，如果新添加的项非常小，我们可能仍需要将其交换另一上层。事实上，我们可能需要交换到树的顶部。 Listing 2 展示了 `percUp` 方法，它在树中向上遍历一个新项，因为它需要去维护堆属性。  注意，我们可以通过使用简单的整数除法来计算任意节点的父节点。 当前节点的父节点可以通过将当前节点的索引除以 2 来计算。

我们现在可以编写 `insert` 方法了（见 Listing 3）。 插入方法中的大部分工作都是由 `percUp` 完成的。 一旦一个新项被追加到树上，`percUp` 接管并正确定位新项。

```python
def percUp(self,i):
    while i // 2 > 0:
      if self.heapList[i] < self.heapList[i // 2]:
         tmp = self.heapList[i // 2]
         self.heapList[i // 2] = self.heapList[i]
         self.heapList[i] = tmp
      i = i // 2
```

*Listing 2*

```python
def insert(self,k):
    self.heapList.append(k)
    self.currentSize = self.currentSize + 1
    self.percUp(self.currentSize)
```

*Listing 3*

使用正确定义的 `insert` 方法，我们现在可以看 `delMin` 方法。 因为堆属性要求树的根是树中的最小项，所以找到最小项很容易。`delMin` 的难点在根被删除后恢复堆结构和堆顺序属性。 我们可以分两步恢复我们的堆。首先，我们将通过获取列表中的最后一个项并将其移动到根位置来恢复根项，保持我们的堆结构属性。 但是，我们可能已经破坏了我们的二叉堆的堆顺序属性。 第二，我们通过将新的根节点沿着树向下推到其正确位置来恢复堆顺序属性。 Figure 3展示了将新的根节点移动到堆中的正确位置所需的交换序列。

![6.10.二叉堆实现.figure3](assets/6.10.%E4%BA%8C%E5%8F%89%E5%A0%86%E5%AE%9E%E7%8E%B0.figure3.png)

*Figure 3*

为了维护堆顺序属性，我们所需要做的是将根节点和最小的子节点交换。在初始交换之后，我们可以将节点和其子节点重复交换，直到节点被交换到正确的位置，使它小于两个子节点。树交换节点的代码可以在 Listing 4中的 `percDown`和`minChild` 方法中找到。

```python
def percDown(self,i):
    while (i * 2) <= self.currentSize:
        mc = self.minChild(i)
        if self.heapList[i] > self.heapList[mc]:
            tmp = self.heapList[i]
            self.heapList[i] = self.heapList[mc]
            self.heapList[mc] = tmp
        i = mc

def minChild(self,i):
    if i * 2 + 1 > self.currentSize:
        return i * 2
    else:
        if self.heapList[i*2] < self.heapList[i*2+1]:
            return i * 2
        else:
            return i * 2 + 1
```

*Listing 4*

`delmin` 操作的代码在 Listing 5 中。注意，有难度的工作由辅助函数处理，在这种情况下是 `percDown` 。

```python
def delMin(self):
    retval = self.heapList[1]
    self.heapList[1] = self.heapList[self.currentSize]
    self.currentSize = self.currentSize - 1
    self.heapList.pop()
    self.percDown(1)
    return retval
```

*Listing 5*

为了完成我们对二叉堆的讨论，我们将看从一个列表构建整个堆的方法。你可能想到的第一种方法如下所示。给定一个列表，通过一次插入一个键轻松地构建一个堆。由于你从一个项的列表开始，该列表是有序的，可以使用二分查找找到正确的位置，以大约 $$O(logn)$$ 操作的成本插入下一个键。 但是，请记住，在列表中间插入项可能需要 $$O(n)$$ 操作来移动列表的其余部分，为新项腾出空间。 因此，要在堆中插入 n 个键，将需要总共 $$O(nlogn)$$ 操作。 然而，如果我们从整个列表开始，那么我们可以在 $$O(n)$$ 操作中构建整个堆。Listing 6 展示了构建整个堆的代码。

```python
def buildHeap(self,alist):
    i = len(alist) // 2
    self.currentSize = len(alist)
    self.heapList = [0] + alist[:]
    while (i > 0):
        self.percDown(i)
        i = i - 1
```

*Listing 6*

![6.10.二叉堆实现.figure4](assets/6.10.%E4%BA%8C%E5%8F%89%E5%A0%86%E5%AE%9E%E7%8E%B0.figure4.png)

*Figure 4*

Figure 4 展示了 `buildHeap` 方法在 `[9,6,5,2,3]` 的初始树中的节点移动到其正确位置时所做的交换。虽然我们从树的中间开始，并以我们的方式回到根节点，`percDown` 方法确保最大的子节点总是沿着树向下移动。因为堆是一个完整的二叉树，超过中途点的任何节点都将是树叶，因此没有子节点。注意，当`i = 1` 时，我们从树的根节点向下交换，因此可能需要多次交换。正如你在 Figure 4 最右边的两个树中可以看到的，首先 9 从根位置移出，但是 9 在树中向下移动一级之后，`percDown` 检查下一组子树，以确保它被推到下一层。在这种情况下，它与 3 进行第二次交换。现在 9 已经移动到树的最低层，不能进行进一步交换。将 Figure 4 所示的这一系列交换的列表与树进行比较是有用的。

```bash
i = 2  [0, 9, 5, 6, 2, 3]
i = 1  [0, 9, 2, 6, 5, 3]
i = 0  [0, 2, 3, 6, 5, 9]
```

完整二叉堆代码实现见 activecode 1

```python
class BinHeap:
    def __init__(self):
        self.heapList = [0]
        self.currentSize = 0


    def percUp(self,i):
        while i // 2 > 0:
          if self.heapList[i] < self.heapList[i // 2]:
             tmp = self.heapList[i // 2]
             self.heapList[i // 2] = self.heapList[i]
             self.heapList[i] = tmp
          i = i // 2

    def insert(self,k):
      self.heapList.append(k)
      self.currentSize = self.currentSize + 1
      self.percUp(self.currentSize)

    def percDown(self,i):
      while (i * 2) <= self.currentSize:
          mc = self.minChild(i)
          if self.heapList[i] > self.heapList[mc]:
              tmp = self.heapList[i]
              self.heapList[i] = self.heapList[mc]
              self.heapList[mc] = tmp
          i = mc

    def minChild(self,i):
      if i * 2 + 1 > self.currentSize:
          return i * 2
      else:
          if self.heapList[i*2] < self.heapList[i*2+1]:
              return i * 2
          else:
              return i * 2 + 1

    def delMin(self):
      retval = self.heapList[1]
      self.heapList[1] = self.heapList[self.currentSize]
      self.currentSize = self.currentSize - 1
      self.heapList.pop()
      self.percDown(1)
      return retval

    def buildHeap(self,alist):
      i = len(alist) // 2
      self.currentSize = len(alist)
      self.heapList = [0] + alist[:]
      while (i > 0):
          self.percDown(i)
          i = i - 1

bh = BinHeap()
bh.buildHeap([9,5,6,2,3])

print(bh.delMin())
print(bh.delMin())
print(bh.delMin())
print(bh.delMin())
print(bh.delMin())

```

*ActiveCode 1*

我们可以在 $$O(n)$$ 中构建堆的断言可能看起来有点神秘，证明超出了本书的范围。 然而，理解的关键是记住 $$logn$$ 因子是从树的高度派生的。 对于`buildHeap` 中的大部分工作，树比 $$logn$$ 短。

基于可以从 $$O(n)$$ 时间构建堆的事实，你可以使用堆对列表在 $$O(nlogn)$$ 时间内排序，作为本章结尾的练习。
