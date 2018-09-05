## 5.10.希尔排序

希尔排序（有时称为“递减递增排序”）通过将原始列表分解为多个较小的子列表来改进插入排序，每个子列表使用插入排序进行排序。 选择这些子列表的方式是希尔排序的关键。不是将列表拆分为连续项的子列表，希尔排序使用增量i（有时称为 `gap`），通过选择 i 个项的所有项来创建子列表。

这可以在 Figure 6 中看到。该列表有九个项。如果我们使用三的增量，有三个子列表，每个子列表可以通过插入排序进行排序。完成这些排序后，我们得到如 Figure 7 所示的列表。虽然这个列表没有完全排序，但发生了很有趣的事情。 通过排序子列表，我们已将项目移动到更接近他们实际所属的位置。

![5.10.希尔排序.figure6](assets/5.10.shell%E6%8E%92%E5%BA%8F.figure6.png)

*Figure 6-7*

Figure 8 展示了使用增量为 1 的插入排序; 换句话说，标准插入排序。注意，通过执行之前的子列表排序，我们减少了将列表置于其最终顺序所需的移位操作的总数。对于这种情况，我们只需要四次移位完成该过程。

![5.10.希尔排序.figure8](assets/5.10.shell%E6%8E%92%E5%BA%8F.figure8.png)

*Figure 8*

![5.10.希尔排序.figure9](assets/5.10.shell%E6%8E%92%E5%BA%8F.figure9.png)

*Figure 9*

我们之前说过，增量的选择方式是希尔排序的独特特征。 ActiveCode 1中展示的函数使用不同的增量集。在这种情况下，我们从 n/2 子列表开始。下一次，n/4 子列表排序。 最后，单个列表按照基本插入排序进行排序。 Figure 9 展示了我们使用此增量的示例的第一个子列表。

```python
def shellSort(alist):
    sublistcount = len(alist)//2
    while sublistcount > 0:

      for startposition in range(sublistcount):
        gapInsertionSort(alist,startposition,sublistcount)

      print("After increments of size",sublistcount,
                                   "The list is",alist)

      sublistcount = sublistcount // 2

def gapInsertionSort(alist,start,gap):
    for i in range(start+gap,len(alist),gap):

        currentvalue = alist[i]
        position = i

        while position>=gap and alist[position-gap]>currentvalue:
            alist[position]=alist[position-gap]
            position = position-gap

        alist[position]=currentvalue

alist = [54,26,93,17,77,31,44,55,20]
shellSort(alist)
print(alist)
```

*Activecode 1*

乍一看，你可能认为希尔排序不会比插入排序更好，因为它最后一步执行了完整的插入排序。 然而，结果是，该最终插入排序不需要进行非常多的比较（或移位），因为如上所述，该列表已经被较早的增量插入排序预排序。 换句话说，每个遍历产生比前一个“更有序”的列表。 这使得最终遍历非常有效。

虽然对希尔排序的一般分析远远超出了本文的范围，我们可以说，它倾向于落在 $$O(n)$$ 和 $$O(n^2)$$ 之间的某处，基于以上所描述的行为。对于 Listing 5中显示的增量，性能为 $$O(n^2)$$ 。 通过改变增量，例如使用`2^k -1（1,3,7,15,31等等）`，希尔排序可以在 $$O(n^{\frac{3}{2}})$$处执行。
