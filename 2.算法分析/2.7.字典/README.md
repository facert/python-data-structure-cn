## 2.7.字典

python 中第二个主要的数据结构是字典。你可能记得，字典和列表不同，你可以通过键而不是位置来访问字典中的项目。在本书的后面，你会看到有很多方法来实现字典。字典的 get 和 set 操作都是 O(1)。另一个重要的操作是 contains，检查一个键是否在字典中也是 O(1)。所有字典操作的效率总结在 Table3 中。关于字典性能的一个重要方面是，我们在表中提供的效率是针对平均性能。 在一些罕见的情况下，contains，get item 和 set item 操作可以退化为 O(n)。我们将在后面的章节介绍。

![2.7.字典.table3](assets/2.7.%E5%AD%97%E5%85%B8.table3.png)
*Table 3*

我们会在最后的实验中，将比较列表和字典之间的 contains 操作的性能。在此过程中，我们将确认列表的 contains 操作符是 $$O(n)$$，字典的 contains 操作符是 $$O(1)$$。我们将在实验中列出一系列数字。然后随机选择数字，并检查数字是否在列表中。如果我们的性能表是正确的，列表越大，确定列表中是否包含任意一个数字应该花费的时间越长。

Listing 6 实现了这个比较。注意，我们对容器中的数字执行完全相同的操作。区别在于在第 7 行上 x 是一个列表，第9行上的 x 是一个字典。

```python
import timeit
import random

for i in range(10000,1000001,20000):
    t = timeit.Timer("random.randrange(%d) in x"%i,
                     "from __main__ import random,x")
    x = list(range(i))
    lst_time = t.timeit(number=1000)
    x = {j:None for j in range(i)}
    d_time = t.timeit(number=1000)
    print("%d,%10.3f,%10.3f" % (i, lst_time, d_time))
```

*Listing 6*

Figure 4 展示了 Listing6 的结果。你可以看到字典一直更快。 对于最小的列表大小为10,000个元素，字典是列表的89.4倍。对于最大的列表大小为990,000 个元素。字典是列表的11,603倍！你还可以看到列表上的contains运算符所花费的时间与列表的大小成线性增长。这验证了列表上的contains运算符是 $$O(n)$$ 的断言。还可以看出，字典中的 contains 运算符的时间是恒定的，即使字典大小不断增长。事实上，对于字典大小为10,000个元素，contains操作占用0.004毫秒，对于字典大小为990,000个元素，它也占用0.004毫秒。

![2.7.字典.figure4](assets/2.7.%E5%AD%97%E5%85%B8.figure4.png)

*Figure 4*

由于 Python 是一种不断发展的语言，底层总是有变化的。 有关 Python 数据结构性能的最新信息可以在 Python 网站上找到。 在撰写本文时，Python wiki有一个很好的时间复杂性页面，可以在 [Time Complexity Wiki](https://wiki.python.org/moin/TimeComplexity) 中找到。
