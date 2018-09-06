## 6.4.列表表示

在由列表表示的树中，我们将从 Python 的列表数据结构开始，并编写上面定义的函数。虽然将接口作为一组操作在列表上编写与我们实现的其他抽象数据类型有点不同，但这样做是有趣的，因为它为我们提供了一个简单的递归数据结构，我们可以直接查看和检查。在列表树的列表中，我们将根节点的值存储为列表的第一个元素。列表的第二个元素本身将是一个表示左子树的列表。列表的第三个元素将是表示右子树的另一个列表。为了说明这种存储技术，让我们看一个例子。 Figure 1 展示了一个简单的树和相应的列表实现。

![6.4.列表表示.figure1](assets/6.4.%E5%88%97%E8%A1%A8%E8%A1%A8%E7%A4%BA.figure1.png)

*Figure 1*

```python
myTree = ['a',   #root
      ['b',  #left subtree
       ['d', [], []],
       ['e', [], []] ],
      ['c',  #right subtree
       ['f', [], []],
       [] ]
     ]
```

注意，我们可以使用标准列表索引来访问列表的子树。树的根是 `myTree[0]`，根的左子树是 `myTree[1]`，右子树是 `myTree[2]`。 ActiveCode 1 说明了使用列表创建一个简单的树。一旦树被构建，我们可以访问根和左右子树。 该列表方法的一个非常好的属性是表示子树的列表的结构遵守树定义的结构; 结构本身是递归的！具有根值和两个空列表的子树是叶节点。列表方法的另一个很好的特性是它可以推广到一个有许多子树的树。在树超过二叉树的情况下，另一个子树只是另一个列表。

```python
myTree = ['a', ['b', ['d',[],[]], ['e',[],[]] ], ['c', ['f',[],[]], []] ]
print(myTree)
print('left subtree = ', myTree[1])
print('root = ', myTree[0])
print('right subtree = ', myTree[2])
```

*Activecode 1*

让我们提供一些使我们能够使用列表作为树的函数来形式化树数据结构的这个定义。注意，我们不会定义一个二叉树类。我们写的函数只是帮助我们操纵一个标准列表，就像我们正在使用一棵树。

```python
def BinaryTree(r):
    return [r, [], []]
```

`BinaryTree` 函数简单地构造一个具有根节点和两个子列表为空的列表。要将左子树添加到树的根，我们需要在根列表的第二个位置插入一个新的列表。我们必须小心。如果列表已经在第二个位置有东西，我们需要跟踪它，并沿着树向下把它作为我们添加的列表的左子节点。Listing 1 展示了插入左子节点的 Python 代码。

```python
def insertLeft(root,newBranch):
    t = root.pop(1)
    if len(t) > 1:
        root.insert(1,[newBranch,t,[]])
    else:
        root.insert(1,[newBranch, [], []])
    return root
```

*Listing 1*

注意，要插入一个左子节点，我们首先获得与当前左子节点对应的（可能为空的）列表。然后我们添加新的左子树，添加旧的左子数作为新子节点的左子节点。这允许我们在任何位置将新节点拼接到树中。`insertRight` 的代码与`insertLeft` 类似，如 Listing 2所示。

```python
def insertRight(root,newBranch):
    t = root.pop(2)
    if len(t) > 1:
        root.insert(2,[newBranch,[],t])
    else:
        root.insert(2,[newBranch,[],[]])
    return root
```

*Listing 2*

为了完成这组树形函数（见 Listing 3），让我们编写一些访问函数来获取和设置根节点的值，以及获取左或右子树。

```python
def getRootVal(root):
    return root[0]

def setRootVal(root,newVal):
    root[0] = newVal

def getLeftChild(root):
    return root[1]

def getRightChild(root):
    return root[2]
```

*Listing 3*
