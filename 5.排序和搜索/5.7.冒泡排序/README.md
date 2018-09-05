## 5.7.冒泡排序

冒泡排序需要多次遍历列表。它比较相邻的项并交换那些无序的项。每次遍历列表将下一个最大的值放在其正确的位置。实质上，每个项“冒泡”到它所属的位置。

Figure 1 展示了冒泡排序的第一次遍历。阴影项正在比较它们是否乱序。如果在列表中有 n 个项目，则第一遍有 n-1 个项需要比较。重要的是要注意，一旦列表中的最大值是一个对的一部分，它将不断地被移动，直到遍历完成。

![5.7.冒泡排序.figure1](assets/5.7.%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F.figure1.png)

*Figure 1*

在第二次遍历的开始，现在最大的值已经在正确的位置。有 n-1 个项留下排序，意味着将有 n-2 对。由于每次通过将下一个最大值放置在适当位置，所需的遍历的总数将是 n-1。 在完成 n-1 遍之后，最小的项肯定在正确的位置，不需要进一步处理。 ActiveCode 1 显示完整的 `bubbleSort` 函数。它将列表作为参数，并根据需要交换项来修改它。

交换操作，有时称为 `swap`，在 Python 中与在大多数其他编程语言略有不同。通常，交换列表中的两个元素需要临时存储位置（额外的内存位置）。 代码片段如下

```python
temp = alist[i]
alist[i] = alist[j]
alist[j] = temp
```

将交换列表中的第 i 项和第 j 项。没有临时存储，其中一个值将被覆盖。

在Python中，可以执行同时赋值。 语句 `a，b = b，a` 两个赋值语句同时完成（参见 Figure 2）。使用同时分配，交换操作可以在一个语句中完成。

ActiveCode 1 中的行 5-7 使用先前描述的三步过程执行 i 和第 i + 1 个项的交换。 注意，我们也可以使用同时分配来交换项目。

![5.7.冒泡排序.figure2](assets/5.7.%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F.figure2.png)

*Figure 2*

```python
def bubbleSort(alist):
    for passnum in range(len(alist)-1,0,-1):
        for i in range(passnum):
            if alist[i]>alist[i+1]:
                temp = alist[i]
                alist[i] = alist[i+1]
                alist[i+1] = temp

alist = [54,26,93,17,77,31,44,55,20]
bubbleSort(alist)
print(alist)
```

*ActiveCode 1*

为了分析气泡排序，我们应该注意，不管项如何在初始列表中排列，将进行 n-1 次遍历以排序大小为 n 的列表。 Figure 1 展示了每次通过的比较次数。比较的总数是第 n-1 个整数的和。回想起来，前 n 个整数的和是 1/2n^2 + 1/2n。 第 n-1 个整数的和为 1/2n^2 + 1/2n -n，其为 1/2n^2 - 1/2n。 这仍然是 $$O(n^2)$$ 比较。在最好的情况下，如果列表已经排序，则不会进行交换。 但是，在最坏的情况下，每次比较都会导致交换元素。 平均来说，我们交换了一半时间。

![5.7.冒泡排序.table1](assets/5.7.%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F.table1.png)

*Table1*

冒泡排序通常被认为是最低效的排序方法，因为它必须在最终位置被知道之前交换项。 这些“浪费”的交换操作是非常昂贵的。 然而，因为冒泡排序遍历列表的整个未排序部分，它有能力做大多数排序算法不能做的事情。特别地，如果在遍历期间没有交换，则我们知道该列表已排序。 如果发现列表已排序，可以修改冒泡排序提前停止。这意味着对于只需要遍历几次列表，冒泡排序具有识别排序列表和停止的优点。 ActiveCode 2 展示了这种修改，通常称为 `短冒泡排序`。

```python
def shortBubbleSort(alist):
    exchanges = True
    passnum = len(alist)-1
    while passnum > 0 and exchanges:
       exchanges = False
       for i in range(passnum):
           if alist[i]>alist[i+1]:
               exchanges = True
               temp = alist[i]
               alist[i] = alist[i+1]
               alist[i+1] = temp
       passnum = passnum-1

alist=[20,30,40,90,50,60,70,80,100,110]
shortBubbleSort(alist)
print(alist)
```

*Activecode 2*
