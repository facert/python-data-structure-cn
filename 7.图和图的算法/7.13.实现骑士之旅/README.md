## 7.13.实现骑士之旅

我们将用来解决骑士旅游问题的搜索算法称为 深度优先搜索（DFS）。尽管在前面部分中讨论的广度优先搜索算法一次建立一个搜索树，但是深度优先搜索通过尽可能深地探索树的一个分支来创建搜索树。在本节中，我们将介绍实现深度优先搜索的两种算法。我们将看到的第一个算法通过明确地禁止一个节点被访问多次来直接解决骑士的旅行问题。第二种实现是更通用的，但是允许在构建树时多次访问节点。第二个版本在后续部分中用于开发其他图形算法。

图的深度优先搜索正是我们需要的，来找到有 63 个边的路径。我们将看到，当深度优先搜索算法找到死角（图中没有可移动的地方）时，它将回到下一个最深的顶点，允许它进行移动。

`knightTour` 函数有四个参数：`n` ，搜索树中的当前深度; `path`，到此为止访问的顶点的列表; `u`，图中我们希望探索的顶点; `limit` 路径中的节点数。 `knightTour` 函数是递归的。当调用 `knightTour` 函数时，它首先检查基本情况。如果我们有一个包含 `64` 个顶点的路径，我们状态为 `True` 的 `knightTour` 返回，表示我们找到了一个成功的线路。如果路径不够长，我们继续通过选择一个新的顶点来探索一层，并对这个顶点递归调用knightTour。

DFS 还使用颜色来跟踪图中的哪些顶点已被访问。未访问的顶点是白色的，访问的顶点是灰色的。如果已经探索了特定顶点的所有邻居，并且我们尚未达到64个顶点的目标长度，我们已经到达死胡同。当我们到达死胡同时，我们必须回溯。当我们从状态为 False 的 knightTour 返回时，发生回溯。在广度优先搜索中，我们使用一个队列来跟踪下一个要访问的顶点。由于深度优先搜索是递归的，我们隐式使用一个栈来帮助我们回溯。当我们从第 11 行的状态为 False 的`knightTour` 调用返回时，我们保持在 while 循环中，并查看 `nbrList` 中的下一个顶点。

```python
from pythonds.graphs import Graph, Vertex
def knightTour(n,path,u,limit):
        u.setColor('gray')
        path.append(u)
        if n < limit:
            nbrList = list(u.getConnections())
            i = 0
            done = False
            while i < len(nbrList) and not done:
                if nbrList[i].getColor() == 'white':
                    done = knightTour(n+1, path, nbrList[i], limit)
                i = i + 1
            if not done:  # prepare to backtrack
                path.pop()
                u.setColor('white')
        else:
            done = True
        return done
```

*Listing 3*

让我们看看一个简单的例子 `knightTour` 。你可以按照搜索的步骤参考下面的图。对于这个例子，我们假设对第 6 行的 `getConnections` 方法的调用按字母顺序对节点排序。我们首先调用 `knightTour(0，path，A，6)`

Figure 中 `knightTour` 从节点 A 开始.与 A 相邻的节点是 B 和 D。由于 B 在字母 D 之前，DFS选择 B 展开下一个，如 Figure 4 所示。当`knightTour` 被递归调用时，开始从 B 开始探寻。 B 与 C 和 D 相邻，所以 knightTour 选择接下来探索 C。然而，如 Figure 5 所示，节点 C 是没有相邻节点的死胡同。此时，我们将节点 C 的颜色更改为白色。对 knightTour 的调用返回值 False。从递归调用的返回有效地将搜索回溯到顶点B（参见Figure 6）。列表中要探索的下一个顶点是顶点 D，因此 `knightTour` 使递归调用移动到节点 D（参见 Figure 7）。从顶点 D 开始，`knightTour` 可以继续进行递归调用，直到我们再次到达节点 C（参见Figure 8，Figure 9和 Figure 10）。然而，当我们到达节点C时，测试 `n <limit` 失败，所以我们知道已经耗尽了图中的所有节点。在这一点上，我们可以返回 True，表示我们已经成功地浏览了图。当我们返回列表时，路径具有值 `[A，B，D，E，F，C]`，这是我们需要遍历图以访问每个节点的顺序。

![7.13.实现骑士之旅.figure3-1](assets/7.13.%E5%AE%9E%E7%8E%B0%E9%AA%91%E5%A3%AB%E4%B9%8B%E6%97%85.figure3-1.png)

![7.13.实现骑士之旅.figure3-2](assets/7.13.%E5%AE%9E%E7%8E%B0%E9%AA%91%E5%A3%AB%E4%B9%8B%E6%97%85.figure3-2.png)

*Figure 3-10*

Figure 11 展示了一个 $$8 \times 8$$ 板的完整遍历。有许多可能的路径; 一些是对称的。 通过一些修改，你可以使遍历开始和结束在同一个正方形。

![7.13.实现骑士之旅.figure3](assets/7.13.%E5%AE%9E%E7%8E%B0%E9%AA%91%E5%A3%AB%E4%B9%8B%E6%97%85.figure3.png)

*Figure 10*
