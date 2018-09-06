## 7.12.构建骑士之旅图

为了将骑士的旅游问题表示为图，我们将使用以下两个点：棋盘上的每个正方形可以表示为图形中的一个节点。 骑士的每个合法移动可以表示为图形中的边。 Figure 1 展示了骑士的移动以及图中的对应边。

![7.12.构建骑士之旅图.figure1](assets/7.12.%E6%9E%84%E5%BB%BA%E9%AA%91%E5%A3%AB%E4%B9%8B%E6%97%85%E5%9B%BE.figure1.png)

*Figure 1*

要构建一个 `n*n` 的完整图，我们可以使用 Listing 1 中所示的 Python 函数。`knightGraph` 函数在整个板上进行一次遍历。 在板上的每个方块上，`knightGraph` 函数调用 `genLegalMoves` ，为板上的位置创建一个移动列表。 所有移动在图形中转换为边。 另一个帮助函数 `posToNodeId` 按照行和列将板上的位置转换为类似于 Figure 1 所示的顶点数的线性顶点数。

```python
from pythonds.graphs import Graph

def knightGraph(bdSize):
    ktGraph = Graph()
    for row in range(bdSize):
       for col in range(bdSize):
           nodeId = posToNodeId(row,col,bdSize)
           newPositions = genLegalMoves(row,col,bdSize)
           for e in newPositions:
               nid = posToNodeId(e[0],e[1],bdSize)
               ktGraph.addEdge(nodeId,nid)
    return ktGraph

def posToNodeId(row, column, board_size):
    return (row * board_size) + column
```

*Listing 1*

`genLegalMoves` 函数（Listing 2）使用板上骑士的位置，并生成八个可能移动中的一个。 `legalCoord` 辅助函数（Listing 2）确保生成的特定移动仍在板上。

```python
def genLegalMoves(x,y,bdSize):
    newMoves = []
    moveOffsets = [(-1,-2),(-1,2),(-2,-1),(-2,1),
                   ( 1,-2),( 1,2),( 2,-1),( 2,1)]
    for i in moveOffsets:
        newX = x + i[0]
        newY = y + i[1]
        if legalCoord(newX,bdSize) and \
                        legalCoord(newY,bdSize):
            newMoves.append((newX,newY))
    return newMoves

def legalCoord(x,bdSize):
    if x >= 0 and x < bdSize:
        return True
    else:
        return False
```

*Listing 2*

Figure 2 展示了一个 $$8 \times 8$$ 板的可能移动的完整图。图中有正好 336 个边。 注意，与板的边相对应的顶点具有比板中间的顶点更少的连接（移动数）。 再次我们可以看到图的稀疏。 如果图形完全连接，则会有 4,096 个边。 由于只有336 个边，邻接矩阵只有 8.2％ 填充率。

![7.12.构建骑士之旅图.figure2](assets/7.12.%E6%9E%84%E5%BB%BA%E9%AA%91%E5%A3%AB%E4%B9%8B%E6%97%85%E5%9B%BE.figure2.png)

*Figure 2*
