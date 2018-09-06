## 5.8.选择排序

选择排序改进了冒泡排序，每次遍历列表只做一次交换。为了做到这一点，一个选择排序在他遍历时寻找最大的值，并在完成遍历后，将其放置在正确的位置。与冒泡排序一样，在第一次遍历后，最大的项在正确的地方。 第二遍后，下一个最大的就位。遍历 n-1 次排序 n 个项，因为最终项必须在第（n-1）次遍历之后。

Figure 3 展示了整个排序过程。在每次遍历时，选择最大的剩余项，然后放置在其适当位置。第一遍放置 93，第二遍放置 77，第三遍放置 55 等。 该函数展示在 ActiveCode 1 中。

![5.8.选择排序.activecode1](assets/5.8.%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F.activecode1.png)

*Activecode 1*

```python
def selectionSort(alist):
   for fillslot in range(len(alist)-1,0,-1):
       positionOfMax=0
       for location in range(1,fillslot+1):
           if alist[location]>alist[positionOfMax]:
               positionOfMax = location

       temp = alist[fillslot]
       alist[fillslot] = alist[positionOfMax]
       alist[positionOfMax] = temp

alist = [54,26,93,17,77,31,44,55,20]
selectionSort(alist)
print(alist)

```

你可能会看到选择排序与冒泡排序有相同数量的比较，因此也是 $$O(n^2)$$。 然而，由于交换数量的减少，选择排序通常在基准研究中执行得更快。 事实上，对于我们的列表，冒泡排序有 20 次交换，而选择排序只有 8 次。
