## 4.11.探索迷宫

在这一节中，我们将讨论一个与扩展机器人世界相关的问题：你如何找到自己的迷宫？ 如果你在你的宿舍有一个扫地机器人（不是所有的大学生？）你希望你可以使用你在本节中学到的知识重新给它编程。 我们要解决的问题是帮助我们的乌龟在虚拟迷宫中找到出路。 迷宫问题的根源与希腊的神话有关，传说忒修斯被送入迷宫中以杀死人身牛头怪。忒修斯用了一卷线帮助他找到回去的退路，当他完成杀死野兽的任务。在我们的问题中，我们将假设我们的乌龟在迷宫中间的某处，必须找到出路。 看看 Figure 2，了解我们将在本节中做什么。

![4.11.探索迷宫.figure2](assets/4.11.%E6%8E%A2%E7%B4%A2%E8%BF%B7%E5%AE%AB.figure2.png)

*Figure 2*

为了使问题容易些，我们假设我们的迷宫被分成“正方形”。迷宫的每个正方形是开放的或被一段墙壁占据。乌龟只能通过迷宫的空心方块。 如果乌龟撞到墙上，它必须尝试不同的方向。乌龟将需要一个程序，以找到迷宫的出路。这里是过程：

1. 从我们的起始位置，我们将首先尝试向北一格，然后从那里递归地尝试我们的程序。
2. 如果我们通过尝试向北作为第一步没有成功，我们将向南一格，并递归地重复我们的程序。
3. 如果向南也不行，那么我们将尝试向西一格，并递归地重复我们的程序。
4. 如果北，南和西都没有成功，则应用程序从当前位置递归向东。
5. 如果这些方向都没有成功，那么没有办法离开迷宫，我们失败。

现在，这听起来很容易，但有几个细节先谈谈。假设我们第一步是向北走。按照我们的程序，我们的下一步也将是向北。但如果北面被一堵墙阻挡，我们必须看看程序的下一步，并试着向南。不幸的是，向南使我们回到原来的起点。如果我们从那里再次应用递归过程，我们将又回到向北一格，并陷入无限循环。所以，我们必须有一个策略来记住我们去过哪。在这种情况下，我们假设有一袋面包屑可以撒在我们走过的路上。如果我们沿某个方向迈出一步，发现那个位置上已经有面包屑，我们应该立即后退并尝试程序中的下一个方向。我们看看这个算法的代码，就像从递归函数调用返回一样简单。

正如我们对所有递归算法所做的一样，让我们回顾一下基本情况。其中一些你可能已经根据前一段的描述猜到了。在这种算法中，有四种基本情况要考虑：

1. 乌龟撞到了墙。由于这一格被墙壁占据，不能进行进一步的探索。
2. 乌龟找到一个已经探索过的格。我们不想继续从这个位置探索，否则会陷入循环。
3. 我们发现了一个外边缘，没有被墙壁占据。换句话说，我们发现了迷宫的一个出口。
4. 我们探索了一格在四个方向上都没有成功。

为了我们的程序工作，我们将需要有一种方式来表示迷宫。为了使这个更有趣，我们将使用 `turtle` 模块来绘制和探索我们的迷宫，以使我们看到这个算法的功能。迷宫对象将提供以下方法让我们在编写搜索算法时使用：

* `__init__` 读取迷宫的数据文件，初始化迷宫的内部表示，并找到乌龟的起始位置。
* `drawMaze` 在屏幕上的一个窗口中绘制迷宫。
* `updatePosition` 更新迷宫的内部表示，并更改窗口中乌龟的位置。
* `isExit` 检查当前位置是否是迷宫的退出位置。

Maze 类还重载索引运算符 `[]` ，以便我们的算法可以轻松访问任何特定格的状态。

让我们来查看称为 `searchFrom` 的搜索函数的代码。代码如 Listing 3 所示。请注意，此函数需要三个参数：迷宫对象，起始行和起始列。 这很重要，因为作为递归函数，搜索在每次递归调用时开始。

```python
def searchFrom(maze, startRow, startColumn):
    maze.updatePosition(startRow, startColumn)
   #  Check for base cases:
   #  1. We have run into an obstacle, return false
   if maze[startRow][startColumn] == OBSTACLE :
        return False
    #  2. We have found a square that has already been explored
    if maze[startRow][startColumn] == TRIED:
        return False
    # 3. Success, an outside edge not occupied by an obstacle
    if maze.isExit(startRow,startColumn):
        maze.updatePosition(startRow, startColumn, PART_OF_PATH)
        return True
    maze.updatePosition(startRow, startColumn, TRIED)

    # Otherwise, use logical short circuiting to try each
    # direction in turn (if needed)
    found = searchFrom(maze, startRow-1, startColumn) or \
            searchFrom(maze, startRow+1, startColumn) or \
            searchFrom(maze, startRow, startColumn-1) or \
            searchFrom(maze, startRow, startColumn+1)
    if found:
        maze.updatePosition(startRow, startColumn, PART_OF_PATH)
    else:
        maze.updatePosition(startRow, startColumn, DEAD_END)
    return found
```

*Listing 3*

你会看到代码的第一行（行 2）调用 `updatePosition`。这只是为了可视化算法，以便你可以看到乌龟如何探索通过迷宫。接下来算法检查四种基本情况中的前三种：乌龟是否碰到墙（行 5）？乌龟是否回到已经探索过的格子（行 8）？乌龟有没有到达出口（行 11）？如果这些条件都不为真，则我们继续递归搜索。

你会注意到，在递归步骤中有四个对 `searchFrom` 的递归调用。很难预测将有多少个递归调用，因为它们都由 `or` 语句连接。如果对 `searchFrom` 的第一次调用返回 `True` ，则不需要最后三个调用。你可以理解这一步向 `(row-1,column)`（或北，如果你从地理位置上思考）是在迷宫的路径上。如果没有一个好的路径向北，那么尝试下一个向南的递归调用。如果向南失败，然后尝试向西，最后向东。如果所有四个递归调用返回 `False`，那么认为是一个死胡同。你应该下载或输入整个程序，并通过更改这些调用的顺序进行实验。

`Maze` 类的代码如 Listing 4，Listing 5 和 Listing 6 所示。`__init__` 方法将文件的名称作为其唯一参数。此文件是一个文本文件，通过使用 `+` 字符表示墙壁，空格表示空心方块，并使用字母 `S` 表示起始位置。Figure 3 是迷宫数据文件的示例。迷宫的内部表示是列表的列表。 `mazelist` 实例变量的每一行也是一个列表。此辅助列表使用上述字符，每格表示一个字符。Figure 3 中的数据文件，内部表示如下所示：

```python
[ ['+','+','+','+',...,'+','+','+','+','+','+','+'],
  ['+',' ',' ',' ',...,' ',' ',' ','+',' ',' ',' '],
  ['+',' ','+',' ',...,'+','+',' ','+',' ','+','+'],
  ['+',' ','+',' ',...,' ',' ',' ','+',' ','+','+'],
  ['+','+','+',' ',...,'+','+',' ','+',' ',' ','+'],
  ['+',' ',' ',' ',...,'+','+',' ',' ',' ',' ','+'],
  ['+','+','+','+',...,'+','+','+','+','+',' ','+'],
  ['+',' ',' ',' ',...,'+','+',' ',' ','+',' ','+'],
  ['+',' ','+','+',...,' ',' ','+',' ',' ',' ','+'],
  ['+',' ',' ',' ',...,' ',' ','+',' ','+','+','+'],
  ['+','+','+','+',...,'+','+','+',' ','+','+','+']]
```

`drawMaze` 方法使用这个内部表示在屏幕上绘制迷宫的初始视图。

Figure 3：示例迷宫数据文件

```python
++++++++++++++++++++++
+   +   ++ ++     +
+ +   +       +++ + ++
+ + +  ++  ++++   + ++
+++ ++++++    +++ +  +
+          ++  ++    +
+++++ ++++++   +++++ +
+     +   +++++++  + +
+ +++++++      S +   +
+                + +++
++++++++++++++++++ +++
```

*Figure 3*

如 Listing 5 所示，`updatePosition` 方法使用相同的内部表示来查看乌龟是否遇到了墙。它还用 `.` 或 `-` 更新内部表示，以表示乌龟已经访问了特定格子或者格子是死角。 此外，`updatePosition` 方法使用两个辅助方法`moveTurtle` 和 `dropBreadCrumb` 来更新屏幕上的视图。

最后，`isExit` 方法使用乌龟的当前位置来检测退出条件。退出条件是当乌龟已经到迷宫的边缘时，即行零或列零，或者在最右边列或底部行。

```python
class Maze:
    def __init__(self,mazeFileName):
        rowsInMaze = 0
        columnsInMaze = 0
        self.mazelist = []
        mazeFile = open(mazeFileName,'r')
        rowsInMaze = 0
        for line in mazeFile:
            rowList = []
            col = 0
            for ch in line[:-1]:
                rowList.append(ch)
                if ch == 'S':
                    self.startRow = rowsInMaze
                    self.startCol = col
                col = col + 1
            rowsInMaze = rowsInMaze + 1
            self.mazelist.append(rowList)
            columnsInMaze = len(rowList)

        self.rowsInMaze = rowsInMaze
        self.columnsInMaze = columnsInMaze
        self.xTranslate = -columnsInMaze/2
        self.yTranslate = rowsInMaze/2
        self.t = Turtle(shape='turtle')
        setup(width=600,height=600)
        setworldcoordinates(-(columnsInMaze-1)/2-.5,
                            -(rowsInMaze-1)/2-.5,
                            (columnsInMaze-1)/2+.5,
                            (rowsInMaze-1)/2+.5)
```

*Listing 4*

```python
def drawMaze(self):
    for y in range(self.rowsInMaze):
        for x in range(self.columnsInMaze):
            if self.mazelist[y][x] == OBSTACLE:
                self.drawCenteredBox(x+self.xTranslate,
                                     -y+self.yTranslate,
                                     'tan')
    self.t.color('black','blue')

def drawCenteredBox(self,x,y,color):
    tracer(0)
    self.t.up()
    self.t.goto(x-.5,y-.5)
    self.t.color('black',color)
    self.t.setheading(90)
    self.t.down()
    self.t.begin_fill()
    for i in range(4):
        self.t.forward(1)
        self.t.right(90)
    self.t.end_fill()
    update()
    tracer(1)

def moveTurtle(self,x,y):
    self.t.up()
    self.t.setheading(self.t.towards(x+self.xTranslate,
                                     -y+self.yTranslate))
    self.t.goto(x+self.xTranslate,-y+self.yTranslate)

def dropBreadcrumb(self,color):
    self.t.dot(color)

def updatePosition(self,row,col,val=None):
    if val:
        self.mazelist[row][col] = val
    self.moveTurtle(col,row)

    if val == PART_OF_PATH:
        color = 'green'
    elif val == OBSTACLE:
        color = 'red'
    elif val == TRIED:
        color = 'black'
    elif val == DEAD_END:
        color = 'red'
    else:
        color = None

    if color:
        self.dropBreadcrumb(color)
```

*Listing 5*

```python
def isExit(self,row,col):
     return (row == 0 or
             row == self.rowsInMaze-1 or
             col == 0 or
             col == self.columnsInMaze-1 )

def __getitem__(self,idx):
     return self.mazelist[idx]
```

*Listing 6*
