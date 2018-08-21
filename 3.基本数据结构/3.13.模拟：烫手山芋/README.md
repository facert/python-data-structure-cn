## 3.13.模拟：烫手山芋

队列的典型应用之一是模拟需要以 FIFO 方式管理数据的真实场景。首先，让我们看看孩子们的游戏烫手山芋，在这个游戏中（见 Figure 2），孩子们围成一个圈，并尽可能快的将一个山芋递给旁边的孩子。在某一个时间，动作结束，有山芋的孩子从圈中移除。游戏继续开始直到剩下最后一个孩子。

![3.13.模拟：烫手山芋.figure2](assets/3.13.%E6%A8%A1%E6%8B%9F%EF%BC%9A%E7%83%AB%E6%89%8B%E5%B1%B1%E8%8A%8B.figure2.png)
*Figure 2*

这个游戏相当于著名的约瑟夫问题，一个一世纪著名历史学家弗拉维奥·约瑟夫斯的传奇故事。故事讲的是，他和他的 39 个战友被罗马军队包围在洞中。他们决定宁愿死，也不成为罗马人的奴隶。他们围成一个圈，其中一人被指定为第一个人，顺时针报数到第七人，就将他杀死。约瑟夫斯是一个成功的数学家，他立即想出了应该坐到哪才能成为最后一人。最后，他加入了罗马的一方，而不是杀了自己。你可以找到这个故事的不同版本，有些说是每次报数 3 个人，有人说允许最后一个人逃跑。无论如何，思想是一样的。

我们将模拟这个烫山芋的过程。我们的程序将输入名称列表和一个称为 num 常量用于报数。它将返回以 `num` 为单位重复报数后剩余的最后一个人的姓名。

为了模拟这个圈，我们使用队列（见 Figure3）。假设拿着山芋的孩子在队列的前面。当拿到山芋的时候，这个孩子将先出列再入队列，把他放在队列的最后。经过 num 次的出队入队后，前面的孩子将被永久移除队列。并且另一个周期开始，继续此过程，直到只剩下一个名字（队列的大小为 1）。

![3.13.模拟：烫手山芋.figure3](assets/3.13.%E6%A8%A1%E6%8B%9F%EF%BC%9A%E7%83%AB%E6%89%8B%E5%B1%B1%E8%8A%8B.figure3.png)

*Figure 3*

```python
from pythonds.basic.queue import Queue

def hotPotato(namelist, num):
    simqueue = Queue()
    for name in namelist:
        simqueue.enqueue(name)

    while simqueue.size() > 1:
        for i in range(num):
            simqueue.enqueue(simqueue.dequeue())

        simqueue.dequeue()

    return simqueue.dequeue()

print(hotPotato(["Bill","David","Susan","Jane","Kent","Brad"],7))

```

*Active code 1*

请注意，在此示例中，计数常数的值大于列表中的名称数。这不是一个问题，因为队列像一个圈，计数会重新回到开始，直到达到计数值。另外，请注意，列表加载到队列中以使列表上的名字位于队列的前面。在这种情况下，`Bill` 是列表中的第一个项，因此他在队列的前面。
