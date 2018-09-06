## 5.3.顺序查找

当数据项存储在诸如列表的集合中时，我们说它们具有线性或顺序关系。 每个数据项都存储在相对于其他数据项的位置。 在 Python 列表中，这些相对位置是单个项的索引值。由于这些索引值是有序的，我们可以按顺序访问它们。 这个过程产生我们的第一种搜索技术 `顺序查找`。

Figure 1 展示了这种搜索的工作原理。 从列表中的第一个项目开始，我们按照基本的顺序排序，简单地从一个项移动到另一个项，直到找到我们正在寻找的项或遍历完整个列表。如果我们遍历完整个列表，则说明正在搜索的项不存在。

![5.3.顺序查找.figure1](assets/5.3.%E9%A1%BA%E5%BA%8F%E6%9F%A5%E6%89%BE.figure1.png)

*Figure 1*

该算法的 Python 实现见 CodeLens 1。该函数需要一个列表和我们正在寻找的项作为参数，并返回一个是否存在的布尔值。`found` 布尔变量初始化为 False，如果我们发现列表中的项，则赋值为 True。

```python
def sequentialSearch(alist, item):
        pos = 0
        found = False

        while pos < len(alist) and not found:
            if alist[pos] == item:
                found = True
            else:
                pos = pos+1

        return found

testlist = [1, 2, 32, 8, 17, 19, 42, 13, 0]
print(sequentialSearch(testlist, 3))
print(sequentialSearch(testlist, 13))
```

*CodeLens 1*

### 5.3.1.顺序查找分析

为了分析搜索算法，我们需要定一个基本计算单位。回想一下，这通常是为了解决问题要重复的共同步骤。对于搜索，计算比较操作数是有意义的。每个比较都有可能找到我们正在寻找的项目。此外，我们在这里做另一个假设。项列表不以任何方式排序。项随机放置到列表中。换句话说，项在列表任何位置的概率是一样的。

如果项不在列表中，知道它的唯一方法是将其与存在的每个项进行比较。如果有n 个项，则顺序查找需要 n 个比较来发现项不存在。在项在列表中的情况下，分析不是那么简单。实际上有三种不同的情况可能发生。在最好的情况下，我们在列表的开头找到所需的项，只需要一个比较。在最坏的情况下，我们直到最后的比较才找到项，第 n 个比较。

平均情况怎么样？平均来说，我们会在列表的一半找到该项; 也就是说，我们将比较 n/2 项。然而，回想一下，当 n 变大时，系数，无论它们是什么，在我们的近似中变得不重要，因此顺序查找的复杂度是 $$O(n)$$。Table 1 总结了这些结果。

![5.3.顺序查找.table1](assets/5.3.%E9%A1%BA%E5%BA%8F%E6%9F%A5%E6%89%BE.table1.png)

*Table 1*

我们之前假设，我们列表中的项是随机放置的，因此在项之间没有相对顺序。如果项以某种方式排序，顺序查找会发生什么？我们能够在搜索技术中取得更好的效率吗？

假设项的列表按升序排列。如果我们正在寻找的项存在于列表中，它在 n 个位置中的概率依旧相同。我们仍然会有相同数量的比较来找到该项。然而，如果该项不存在，则有一些优点。Figure 2 展示了这个过程，寻找项 50。注意，项仍然按顺序进行比较直到 54。此时，我们知道一些额外的东西。不仅 54 不是我们正在寻找的项，也没有超过 54 的其他元素可以匹配到该项，因为列表是有序的。在这种情况下，算法不必继续查看所有项。它可以立即停止。 CodeLens 2 展示了顺序查找功能的这种变化。

![5.3.顺序查找.figure2](assets/5.3.%E9%A1%BA%E5%BA%8F%E6%9F%A5%E6%89%BE.figure2.png)


*Figure 2*

```python
def orderedSequentialSearch(alist, item):
        pos = 0
        found = False
        stop = False
        while pos < len(alist) and not found and not stop:
            if alist[pos] == item:
                found = True
            else:
                if alist[pos] > item:
                    stop = True
                else:
                    pos = pos+1

        return found

testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42,]
print(orderedSequentialSearch(testlist, 3))
print(orderedSequentialSearch(testlist, 13))
```

*CodeLens 2*

Table 2 总结了这些结果。 请注意，在最好的情况下，我们通过只查看一项会发现该项不在列表中。 平均来说，我们将只了解 n/2 项就知道。然而，这种复杂度仍然是 $$O(n)$$。 总之，只有在我们没有找到该项的情况下，才通过对列表排序来改进顺序查找。

![5.3.顺序查找.table2](assets/5.3.%E9%A1%BA%E5%BA%8F%E6%9F%A5%E6%89%BE.table2.png)

*Table 2*
