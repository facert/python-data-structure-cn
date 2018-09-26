## 5.12.快速排序

快速排序使用分而治之来获得与归并排序相同的优点，而不使用额外的存储。然而，作为权衡，有可能列表不能被分成两半。当这种情况发生时，我们将看到性能降低。

快速排序首先选择一个值，该值称为 `枢轴值`。虽然有很多不同的方法来选择枢轴值，我们将使用列表中的第一项。枢轴值的作用是帮助拆分列表。枢轴值属于最终排序列表（通常称为拆分点）的实际位置，将用于将列表划分为快速排序的后续调用。

Figure 12 展示 54 将作为我们的第一个枢纽值。由于我们已经看过这个例子几次，我们知道 54 最终将会在当前持有 31 的位置。接下来将发生分区过程。它将找到拆分点，同时将其他项移动到列表的适当侧，小于或大于枢轴值。

![5.12.快速排序.figure12](assets/5.12.%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F.figure12.png)

*Figure 12*

分区从通过在列表中剩余项目的开始和结束处定位两个位置标记（我们称为左标记和右标记）开始（Figure 13中的位置 1 和 8 ）。分区的目标是移动相对于枢轴值位于错误侧的项，同时也收敛于分裂点。 Figure13展示了我们定位54的位置的过程。

![5.12.快速排序.figure13](assets/5.12.%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F.figure13.png)

*Figure 13*

我们首先增加左标记，直到我们找到一个大于枢轴值的值。 然后我们递减右标，直到我们找到小于枢轴值的值。我们发现了两个相对于最终分裂点位置不适当的项。 对于我们的例子，这发生在 93 和 20。现在我们可以交换这两个项目，然后重复该过程。

在右标变得小于左标记的点，我们停止。右标记的位置现在是分割点。枢轴值可以与拆分点的内容交换，枢轴值现在就位（Figure 14）。 此外，分割点左侧的所有项都小于枢轴值，分割点右侧的所有项都大于枢轴值。现在可以在分割点处划分列表，并且可以在两半上递归调用快速排序。

![5.12.快速排序.figure14](assets/5.12.%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F.figure14.png)

*Figure14*

ActiveCode 1中显示 `quickSort` 函数调用递归函数`quickSortHelper`。 `quickSortHelper` 以与合并排序相同的基本情况开始。如果列表的长度小于或等于一，它已经排序。 如果它更大，那么它可以被分区和递归排序。 分区函数实现前面描述的过程。

```python
def quickSort(alist):
   quickSortHelper(alist,0,len(alist)-1)

def quickSortHelper(alist,first,last):
   if first<last:

       splitpoint = partition(alist,first,last)

       quickSortHelper(alist,first,splitpoint-1)
       quickSortHelper(alist,splitpoint+1,last)


def partition(alist,first,last):
   pivotvalue = alist[first]

   leftmark = first+1
   rightmark = last

   done = False
   while not done:

       while leftmark <= rightmark and alist[leftmark] <= pivotvalue:
           leftmark = leftmark + 1

       while alist[rightmark] >= pivotvalue and rightmark >= leftmark:
           rightmark = rightmark -1

       if rightmark < leftmark:
           done = True
       else:
           temp = alist[leftmark]
           alist[leftmark] = alist[rightmark]
           alist[rightmark] = temp

   temp = alist[first]
   alist[first] = alist[rightmark]
   alist[rightmark] = temp


   return rightmark

alist = [54,26,93,17,77,31,44,55,20]
quickSort(alist)
print(alist)
```

要分析 `quickSort` 函数，请注意，对于长度为 n 的列表，如果分区总是出现在列表中间，则会再次出现 log⁡n 分区。为了找到分割点，需要针对枢轴值检查 n 个项中的每一个。结果是 nlog⁡n。此外，在归并排序过程中不需要额外的存储器。

不幸的是，在最坏的情况下，分裂点可能不在中间，并且可能非常偏向左边或右边，留下非常不均匀的分割。在这种情况下，对 n 个项的列表进行排序划分为对0 个项的列表和 n-1 个项目的列表进行排序。然后将 n-1 个划分的列表排序为大小为0的列表和大小为 n-2 的列表，以此类推。结果是具有递归所需的所有开销的 $$O(n^2)$$ 排序。

我们之前提到过，有不同的方法来选择枢纽值。特别地，我们可以尝试通过使用称为中值三的技术来减轻一些不均匀分割的可能性。要选择枢轴值，我们将考虑列表中的第一个，中间和最后一个元素。在我们的示例中，这些是54,77和20.现在选择中值，在我们的示例中为54，并将其用于枢轴值（当然，这是我们最初使用的枢轴值）。想法是，在列表中的第一项不属于列表的中间的情况下，中值三将选择更好的“中间”值。当原始列表部分有序时，这将特别有用。我们将此枢轴值选择的实现作为练习。
