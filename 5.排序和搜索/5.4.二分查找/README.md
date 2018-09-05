## 5.4.二分查找

有序列表对于我们的比较是很有用的。在顺序查找中，当我们与第一个项进行比较时，如果第一个项不是我们要查找的，则最多还有 `n-1` 个项目。 二分查找从中间项开始，而不是按顺序查找列表。 如果该项是我们正在寻找的项，我们就完成了查找。 如果它不是，我们可以使用列表的有序性质来消除剩余项的一半。如果我们正在查找的项大于中间项，就可以消除中间项以及比中间项小的一半元素。如果该项在列表中，肯定在大的那半部分。

然后我们可以用大的半部分重复这个过程。从中间项开始，将其与我们正在寻找的内容进行比较。再次，我们找到元素或将列表分成两半，消除可能的搜索空间的另一部分。Figure 3 展示了该算法如何快速找到值 54 。完整的函数见CodeLens 3中。

![5.4.二分查找.figure3](assets/5.4.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.figure3.png)

*Figure 3*

```python
def binarySearch(alist, item):
        first = 0
        last = len(alist)-1
        found = False

        while first<=last and not found:
            midpoint = (first + last)//2
            if alist[midpoint] == item:
                found = True
            else:
                if item < alist[midpoint]:
                    last = midpoint-1
                else:
                    first = midpoint+1

        return found

testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42,]
print(binarySearch(testlist, 3))
print(binarySearch(testlist, 13))
```

*CodeLens 3*

在我们继续分析之前，我们应该注意到，这个算法是分而治之策略的一个很好的例子。分和治意味着我们将问题分成更小的部分，以某种方式解决更小的部分，然后重新组合整个问题以获得结果。 当我们执行列表的二分查找时，我们首先检查中间项。如果我们正在搜索的项小于中间项，我们可以简单地对原始列表的左半部分执行二分查找。同样，如果项大，我们可以执行右半部分的二分查找。 无论哪种方式，都是递归调用二分查找函数。 CodeLens 4 展示了这个递归版本。

```python
def binarySearch(alist, item):
        if len(alist) == 0:
            return False
        else:
            midpoint = len(alist)//2
            if alist[midpoint]==item:
              return True
            else:
              if item<alist[midpoint]:
                return binarySearch(alist[:midpoint],item)
              else:
                return binarySearch(alist[midpoint+1:],item)

testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42,]
print(binarySearch(testlist, 3))
print(binarySearch(testlist, 13))
```

*CodeLens 4*

### 5.4.1.二分查找分析

为了分析二分查找算法，我们需要记住，每个比较消除了大约一半的剩余项。该算法检查整个列表的最大比较数是多少？如果我们从 n 项开始，大约 n/2 项将在第一次比较后留下。第二次比较后，会有约 n/4。 然后 n/8，n/16，等等。 我们可以拆分列表多少次？ Table 3 帮助我们找到答案。

![5.4.二分查找.table3](assets/5.4.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.table3.png)

*Table 3*

当我们切分列表足够多次时，我们最终得到只有一个项的列表。 要么是我们正在寻找的项，要么不是。达到这一点所需的比较数是 i，当 $$\frac{n}{2^i} = 1$$ 时。 求解 i 得出 $$i = logn$$ 。 最大比较数相对于列表中的项是对数的。 因此，二分查找是 $$O(log n)$$。

还需要解决一个额外的分析问题。在上面所示的递归解中，递归调用，

```python
binarySearch(alist[:midpoint],item)
```

使用切片运算符创建列表的左半部分，然后传递到下一个调用（同样对于右半部分）。我们上面做的分析假设切片操作符是恒定时间的。然而，我们知道 Python中的 slice 运算符实际上是 $$O(k)$$。这意味着使用 slice 的二分查找将不会在严格的对数时间执行。幸运的是，这可以通过传递列表连同开始和结束索引来纠正。可以像 CodeLens 3 中所做的那样计算索引。我们将此实现作为练习。

即使二分查找通常比顺序查找更好，但重要的是要注意，对于小的 n 值，排序的额外成本可能不值得。事实上，我们应该经常考虑采取额外的分类工作是否使搜索获得好处。如果我们可以排序一次，然后查找多次，排序的成本就不那么重要。然而，对于大型列表，一次排序可能是非常昂贵，从一开始就执行顺序查找可能是最好的选择。
