## 7.3.图抽象数据类型

图抽象数据类型（ADT）定义如下：

* Graph() 创建一个新的空图。
* addVertex(vert) 向图中添加一个顶点实例。
* addEdge(fromVert, toVert) 向连接两个顶点的图添加一个新的有向边。
* addEdge(fromVert, toVert, weight) 向连接两个顶点的图添加一个新的加权的有向边。
* getVertex(vertKey) 在图中找到名为 `vertKey` 的顶点。
* getVertices() 返回图中所有顶点的列表。
* in 返回 True 如果 `vertex in graph` 里给定的顶点在图中，否则返回False。

从图的正式定义开始，我们有几种方法可以在 Python 中实现图 ADT。 我们将看到在使用不同的表示来实现上述 ADT 时存在权衡。 有两个众所周知的图形、实现，`邻接矩阵` 和 `邻接表` 。 我们将解释这两个选项，然后实现一个作为 Python 类。
