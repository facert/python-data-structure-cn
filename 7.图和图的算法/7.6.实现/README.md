## 7.6.实现

使用字典，很容易在 Python 中实现邻接表。在我们的 Graph 抽象数据类型的实现中，我们将创建两个类（见 Listing 1和 Listing 2），Graph（保存顶点的主列表）和 Vertex（将表示图中的每个顶点）。

每个顶点使用字典来跟踪它连接的顶点和每个边的权重。这个字典称为`connectedTo` 。 下面的列表展示了 `Vertex` 类的代码。构造函数只是初始化 `id` ，通常是一个字符串和 `connectedTo` 字典。 `addNeighbor`方法用于从这个顶点添加一个连接到另一个。`getConnections` 方法返回邻接表中的所有顶点，如 `connectedTo` 实例变量所示。 `getWeight` 方法返回从这个顶点到作为参数传递的顶点的边的权重。

```python
class Vertex:
    def __init__(self,key):
        self.id = key
        self.connectedTo = {}

    def addNeighbor(self,nbr,weight=0):
        self.connectedTo[nbr] = weight

    def __str__(self):
        return str(self.id) + ' connectedTo: ' + str([x.id for x in self.connectedTo])

    def getConnections(self):
        return self.connectedTo.keys()

    def getId(self):
        return self.id

    def getWeight(self,nbr):
        return self.connectedTo[nbr]
```

*Listing 1*

下一个列表中显示的 Graph 类包含将顶点名称映射到顶点对象的字典。在 Figure 4中，该字典对象由阴影灰色框表示。 `Graph` 还提供了将顶点添加到图并将一个顶点连接到另一个顶点的方法。`getVertices` 方法返回图中所有顶点的名称。此外，我们实现了 `__iter__` 方法，以便轻松地遍历特定图中的所有顶点对象。 这两种方法允许通过名称或对象本身在图形中的顶点上进行迭代。

```python
class Graph:
    def __init__(self):
        self.vertList = {}
        self.numVertices = 0

    def addVertex(self,key):
        self.numVertices = self.numVertices + 1
        newVertex = Vertex(key)
        self.vertList[key] = newVertex
        return newVertex

    def getVertex(self,n):
        if n in self.vertList:
            return self.vertList[n]
        else:
            return None

    def __contains__(self,n):
        return n in self.vertList

    def addEdge(self,f,t,cost=0):
        if f not in self.vertList:
            nv = self.addVertex(f)
        if t not in self.vertList:
            nv = self.addVertex(t)
        self.vertList[f].addNeighbor(self.vertList[t], cost)

    def getVertices(self):
        return self.vertList.keys()

    def __iter__(self):
        return iter(self.vertList.values())
```

*Listing 2*

使用刚才定义的 `Graph` 和 `Vertex` 类，下面的 Python 会话创建 Figure 2中的图。首先我们创建 6 个编号为 0 到 5 的顶点。然后我们展示顶点字典。 注意，对于每个键 0 到 5，我们创建了一个顶点的实例。接下来，我们添加将顶点连接在一起的边。 最后，嵌套循环验证图中的每个边缘是否正确存储。 你应该在本会话结束时根据 Figure 2检查边列表的输出是否正确。

```bash
>>> g = Graph()
>>> for i in range(6):
...    g.addVertex(i)
>>> g.vertList
{0: <adjGraph.Vertex instance at 0x41e18>,
 1: <adjGraph.Vertex instance at 0x7f2b0>,
 2: <adjGraph.Vertex instance at 0x7f288>,
 3: <adjGraph.Vertex instance at 0x7f350>,
 4: <adjGraph.Vertex instance at 0x7f328>,
 5: <adjGraph.Vertex instance at 0x7f300>}
>>> g.addEdge(0,1,5)
>>> g.addEdge(0,5,2)
>>> g.addEdge(1,2,4)
>>> g.addEdge(2,3,9)
>>> g.addEdge(3,4,7)
>>> g.addEdge(3,5,3)
>>> g.addEdge(4,0,1)
>>> g.addEdge(5,4,8)
>>> g.addEdge(5,2,1)
>>> for v in g:
...    for w in v.getConnections():
...        print("( %s , %s )" % (v.getId(), w.getId()))
...
( 0 , 5 )
( 0 , 1 )
( 1 , 2 )
( 2 , 3 )
( 3 , 4 )
( 3 , 5 )
( 4 , 0 )
( 5 , 4 )
( 5 , 2 )
```

*Figure 2*
