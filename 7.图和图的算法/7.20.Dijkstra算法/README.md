## 7.20.Dijkstra算法

我们将用于确定最短路径的算法称为“Dijkstra算法”。Dijkstra算法是一种迭代算法，它为我们提供从一个特定起始节点到图中所有其他节点的最短路径。这也类似于广度优先搜索的结果。

为了跟踪从开始节点到每个目的地的总成本，我们将使用顶点类中的 dist 实例变量。 dist实例变量将包含从开始到所讨论的顶点的最小权重路径的当前总权重。该算法对图中的每个顶点重复一次;然而，我们在顶点上迭代的顺序由优先级队列控制。用于确定优先级队列中对象顺序的值为dist。当首次创建顶点时，dist被设置为非常大的数。理论上，你将 dist 设置为无穷大，但在实践中，我们只是将它设置为一个数字，大于任何真正的距离，我们将在问题中试图解决。

Dijkstra算法的代码如 Listing 1 所示。当算法完成时，距离设置正确，如图中每个顶点的前导链接一样

```python
from pythonds.graphs import PriorityQueue, Graph, Vertex
def dijkstra(aGraph,start):
    pq = PriorityQueue()
    start.setDistance(0)
    pq.buildHeap([(v.getDistance(),v) for v in aGraph])
    while not pq.isEmpty():
        currentVert = pq.delMin()
        for nextVert in currentVert.getConnections():
            newDist = currentVert.getDistance() \
                    + currentVert.getWeight(nextVert)
            if newDist < nextVert.getDistance():
                nextVert.setDistance( newDist )
                nextVert.setPred(currentVert)
                pq.decreaseKey(nextVert,newDist)
```

*Listing 1*

Dijkstra的算法使用优先级队列。你可能还记得，优先级队列是基于我们在树章节中实现的堆。这个简单的实现和我们用于Dijkstra算法的实现之间有几个区别。首先，PriorityQueue 类存储键值对的元组。这对于Dijkstra的算法很重要，因为优先级队列中的键必须匹配图中顶点的键。其次，值用于确定优先级，并且用于确定键在优先级队列中的位置。在这个实现中，我们使用到顶点的距离作为优先级，因为我们看到当探索下一个顶点时，我们总是要探索具有最小距离的顶点。第二个区别是增加 `decreaseKey` 方法。正如你看到的，当一个已经在队列中的顶点的距离减小时，使用这个方法，将该顶点移动到队列的前面。

让我们使用下面的序列图像作为指导，一次应用 Dijkstra 算法的一个顶点。我们从顶点 u 开始。与 u 相邻的三个顶点是 v，w 和 x。由于到 v，w 和 x 的初始距离都被初始化为 `sys.maxint`，通过起始节点获得它们的新成本都是它们的直接成本。因此，我们将这三个节点中的每一个成本更新。我们还将每个节点的前导设置为 u，并将每个节点添加到优先级队列。我们使用距离作为优先级队列的键。算法的状态如 Figure 3所示。

在 while 循环的下一次迭代中，我们检查与 x 相邻的顶点。顶点 x 是下一个，因为它具有最低的总成本，因此冒泡到优先级队列的开始。在 x，我们看看它的邻居 u，v，w 和 y。对于每个相邻顶点，我们检查通过 x 到该顶点的距离是否小于先前已知的距离。显然，这是 y 的情况，因为它的距离是 sys.maxint。这不是 u 或 v 的情况，因为它们的距离分别为 0 和 2。然而，我们现在知道，如果我们经过 x 而不是从 u 直接到 w，到 w 的距离更小。既然是这样，我们用新的距离更新 w，并将 w 的前导从 u 更改为 x。有关所有顶点的状态，请参见 Figure 4。

下一步是查看邻近 v 的顶点（参见 Figure 5）。此步骤不会对图形进行任何更改，因此我们继续前进到节点 y。在节点 y（见Figure 6），我们发现到 w 和 z 都更小，因此我们相应地调整距离和前导链接。最后，我们检查节点 w 和 z（参见 Figure 6 和 Figure 8）。但是，没有发现额外的更改，因此优先级队列为空，Dijkstra的算法退出。

![7.20.Dijkstra算法.figure3](assets/7.20.Dijkstra%E7%AE%97%E6%B3%95.figure3.png)

![7.20.Dijkstra算法.figure4](assets/7.20.Dijkstra%E7%AE%97%E6%B3%95.figure4.png)

![7.20.Dijkstra算法.figure5](assets/7.20.Dijkstra%E7%AE%97%E6%B3%95.figure5.png)

重要的是要注意，Dijkstra的算法只有当权重都是正数时才起作用。 如果你在图的边引入一个负权重，算法永远不会退出。

我们将注意到，通过因特网路由发送消息，可以使用其他算法来找到最短路径。 在互联网上使用 Dijkstra 算法的一个问题是，为了使算法运行，你必须有一个完整的图表示。 这意味着每个路由器都有一个完整的互联网中所有路由器地图。 实际上不是这种情况，算法的其他变种允许每个路由器在它们发送时发现图。 你可能想要了解的一种这样的算法称为 “距离矢量” 路由算法。
