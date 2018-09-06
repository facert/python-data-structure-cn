## 6.17.AVL平衡二叉搜索树实现

现在我们已经证明保持 AVL树的平衡将是一个很大的性能改进，让我们看看如何增加过程来插入一个新的键到树。由于所有新的键作为叶节点插入到树中，并且我们知道新叶的平衡因子为零，所以刚刚插入的节点没有新的要求。但一旦添加新叶，我们必须更新其父的平衡因子。这个新叶如何影响父的平衡因子取决于叶节点是左孩子还是右孩子。如果新节点是右子节点，则父节点的平衡因子将减少1。如果新节点是左子节点，则父节点的平衡因子将增加1。这个关系可以递归地应用到新节点的祖父节点，并且应用到每个祖先一直到树的根。由于这是一个递归过程，我们来看一下用于更新平衡因子的两种基本情况：

* 递归调用已到达树的根。
* 父节点的平衡因子已调整为零。你应该说服自己，一旦一个子树的平衡因子为零，那么它的祖先节点的平衡不会改变。

我们将实现 AVL 树作为 `BinarySearchTree` 的子类。首先，我们将覆盖`_put` 方法并编写一个新的 `updateBalance` 辅助方法。这些方法如Listing 1所示。你将注意到，`_put` 的定义与简单二叉搜索树中的完全相同，除了第 7 行和第 13 行上对 `updateBalance` 的调用的添加。

```python
def _put(self,key,val,currentNode):
    if key < currentNode.key:
        if currentNode.hasLeftChild():
                self._put(key,val,currentNode.leftChild)
        else:
                currentNode.leftChild = TreeNode(key,val,parent=currentNode)
                self.updateBalance(currentNode.leftChild)
    else:
        if currentNode.hasRightChild():
                self._put(key,val,currentNode.rightChild)
        else:
                currentNode.rightChild = TreeNode(key,val,parent=currentNode)
                self.updateBalance(currentNode.rightChild)

def updateBalance(self,node):
    if node.balanceFactor > 1 or node.balanceFactor < -1:
        self.rebalance(node)
        return
    if node.parent != None:
        if node.isLeftChild():
                node.parent.balanceFactor += 1
        elif node.isRightChild():
                node.parent.balanceFactor -= 1

        if node.parent.balanceFactor != 0:
                self.updateBalance(node.parent)
```

*Listing 1*

新的 `updateBalance` 方法完成了大多数工作。这实现了我们刚才描述的递归过程。 `updateBalance` 方法首先检查当前节点是否不够平衡，需要重新平衡（第16行）。如果平衡，则重新平衡完成，并且不需要对父节点进行进一步更新。如果当前节点不需要重新平衡，则调整父节点的平衡因子。如果父的平衡因子不为零，那么算法通过递归调用父对象上的 `updateBalance`，继续沿树向根向上运行。

当需要树重新平衡时，我们如何做呢？有效的重新平衡是使AVL树在不牺牲性能的情况下正常工作的关键。为了使AVL树恢复平衡，我们将在树上执行一个或多个旋转。

要理解旋转是什么让我们看一个非常简单的例子。考虑 Figure 3左半部分的树。这棵树平衡因子为 -2，不平衡。为了使这棵树平衡，我们将使用以节点 A 为根的子树的左旋转。

![6.16.平衡二叉搜索树实现.figure3](assets/6.16.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E5%AE%9E%E7%8E%B0.figure3.png)

*Figure 3*

要执行左旋转，我们基本上执行以下操作：

* 提升右孩子（B）成为子树的根。
* 将旧根（A）移动为新根的左子节点。
* 如果新根（B）已经有一个左孩子，那么使它成为新左孩子（A）的右孩子。注意：由于新根（B）是A的右孩子，A 的右孩子在这一点上保证为空。这允许我们添加一个新的节点作为右孩子，不需进一步的考虑。


虽然这个过程在概念上相当容易，但是代码的细节有点棘手，因为我们需要按照正确的顺序移动事物，以便保留二叉搜索树的所有属性。此外，我们需要确保适当地更新所有的父指针。

让我们看一个稍微更复杂的树来说明右旋转。Figure 4的左侧显示了树的左重，在根处的平衡因子为 2。要执行右旋转，我们基本上执行以下操作：

* 提升左子节点（C）为子树的根。
* 将旧根（E）移动为新根的右子树。
* 如果新根（C）已经有一个正确的孩子（D），那么使它成为新的右孩子（E）的左孩子。注意：由于新根（C）是 E 的左子节点，因此 E 的左子节点在此时保证为空。这允许我们添加一个新节点作为左孩子，不需进一步的考虑。

![6.16.平衡二叉搜索树实现.figure4](assets/6.16.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E5%AE%9E%E7%8E%B0.figure4.png)

*Figure 4*

现在你已经看到了旋转，并且有旋转的工作原理的基本概念，让我们看看代码。Listing 2展示了右旋转和左旋转的代码。在第2行中，我们创建一个临时变量来跟踪子树的新根。正如我们之前所说的，新的根是上一个根的右孩子。现在对这个临时变量存储了一个对右孩子的引用，我们用新的左孩子替换旧根的右孩子。

下一步是调整两个节点的父指针。如果 newRoot 有一个左子节点，那么左子节点的新父节点变成旧的根节点。新根的父节点设置为旧根的父节点。如果旧根是整个树的根，那么我们必须设置树的根以指向这个新根。否则，如果旧根是左孩子，则我们将左孩子的父节点更改为指向新根;否则我们改变右孩子的父亲指向新的根。（行10-13）。最后，我们将旧根的父节点设置为新根。这是一个很复杂的过程，所以我们鼓励你跟踪这个功能，同时看下 Figure 3。 `rotateRight` 方法是对称的 `rotateLeft`，所以我们将留给你来研究 `rotateRight` 的代码。

```python
def rotateLeft(self,rotRoot):
    newRoot = rotRoot.rightChild
    rotRoot.rightChild = newRoot.leftChild
    if newRoot.leftChild != None:
        newRoot.leftChild.parent = rotRoot
    newRoot.parent = rotRoot.parent
    if rotRoot.isRoot():
        self.root = newRoot
    else:
        if rotRoot.isLeftChild():
                rotRoot.parent.leftChild = newRoot
        else:
            rotRoot.parent.rightChild = newRoot
    newRoot.leftChild = rotRoot
    rotRoot.parent = newRoot
    rotRoot.balanceFactor = rotRoot.balanceFactor + 1 - min(newRoot.balanceFactor, 0)
    newRoot.balanceFactor = newRoot.balanceFactor + 1 + max(rotRoot.balanceFactor, 0)
```

*Listing 2*

最后，第16-17行需要一些解释。 在这两行中，我们更新旧根和新根的平衡因子。 由于所有其他移动都是移动整个子树，所以所有其他节点的平衡因子不受旋转的影响。 但是我们如何在不完全重新计算新子树的高度的情况下更新平衡因子呢？ 以下推导应该能说服你这些行是正确的。

![6.16.平衡二叉搜索树实现.figure5](assets/6.16.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E5%AE%9E%E7%8E%B0.figure5.png)

*Figure 5*

Figure 5 展示了左旋转。 B 和 D 是关键节点，A，C，E 是它们的子树。 设hx 表示以节点 x 为根的特定子树的高度。 根据定义，我们知道以下：

$$
\begin{aligned}
newBal(B)&=hA-hC\\
oldBal(B)&=hA-hD
\end{aligned}
$$

但我们知道，D 的旧高度也可以由 `1 + max(hC，hE)`给出，也就是说，D 的高度比其两个孩子的最大高度大 1。 记住，`hC`和 `hE` 没有改变。 所以，让我们用第二个方程来代替它

$$
oldBal(B)=hA-(1+max(hC,hE))
$$

然后减去这两个方程。 以下步骤进行减法并使用一些代数来简化 `newBal(B)` 的等式。

$$
\begin{aligned}
newBal(B)-oldBal(B)&=hA-hC-(hA-(1+max(hC,hE)))\\
newBal(B)-oldBal(B)&=hA-hC-hA+(1+max(hC,hE))\\
newBal(B)-oldBal(B)&=hA-hA+1+max(hC,hE)-hC\\
newBal(B)-oldBal(B)&=1+max(hC,hE)-hC
\end{aligned}
$$

接下来我们将 oldBal(B) 移动到方程的右边，并利用 `max(a，b) -c = max(a-c，b-c)`。

$$
newBal(B)=oldBal(B)+1+max(hC-hC,hE-hC)
$$

但是，$$hE-hC$$ 与 $$-oldBal(D)$$ 相同。因此，我们可以使用另一个表示 $$max(-a, -b) = -min(a, b)$$ 的标识。 因此，我们可以完成我们的 newBal(B) 的推导，具有以下步骤：

$$
\begin{aligned}
newBal(B)&=oldBal(B)+1+max(0,-oldBal(D))\\
newBal(B)&=oldBal(B)+1-min(0,oldBal(D))
\end{aligned}
$$

现在我们有所有的部分，我们很容易知道。 我们记住 B 是 rotRoot 和 D 是newRoot 然后我们可以看到这正好对应第16行的语句，或者：

```python
rotRoot.balanceFactor = rotRoot.balanceFactor + 1 - min(0,newRoot.balanceFactor)
```

类似的推导给出了更新的节点 D 的方程，以及右旋转后的平衡因子。 我们把这些作为你的练习。

现在你可能认为我们已经完成了。 我们知道如何做左右旋转，我们知道什么时候应该做左旋或右旋，但是看看 Figure 6。由于节点 A 的平衡因子为-2，我们应该做左旋转。 但是，当我们围绕A做左旋转时会发生什么？

![6.16.平衡二叉搜索树实现.figure6](assets/6.16.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E5%AE%9E%E7%8E%B0.figure6.png)

*Figure 6*

Figure 7 展示了我们在左旋后，我们现在已经在另一方面失去平衡。 如果我们做右旋以纠正这种情况，我们就回到我们开始的地方。
![6.16.平衡二叉搜索树实现.figure7](assets/6.16.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E5%AE%9E%E7%8E%B0.figure7.png)

*Figure 7*

要纠正这个问题，我们必须使用以下规则集：

* 如果子树需要左旋转使其平衡，首先检查右子节点的平衡因子。 如果右孩子是重的，那么对右孩子做右旋转，然后是原来的左旋转。
* 如果子树需要右旋转使其平衡，首先检查左子节点的平衡因子。 如果左孩子是重的，那么对左孩子做左旋转，然后是原来的右旋转。

Figure 8展示了这些规则如何解决我们在Figure 6和 Figure 7中遇到的困境。从围绕节点 C的 右旋转开始，将树放置在 A 的左旋转使整个子树恢复平衡的位置。

![6.16.平衡二叉搜索树实现.figure8](assets/6.16.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E5%AE%9E%E7%8E%B0.figure8.png)

*Figure 8*

实现这些规则的代码可以在我们的重新平衡方法中找到，如 Listing 3所示。上面的规则编号 1 是从第2行开始的if语句实现的。规则编号2是由第8行开始的elif语句实现的 。

```python
def rebalance(self,node):
  if node.balanceFactor < 0:
         if node.rightChild.balanceFactor > 0:
            self.rotateRight(node.rightChild)
            self.rotateLeft(node)
         else:
            self.rotateLeft(node)
  elif node.balanceFactor > 0:
         if node.leftChild.balanceFactor < 0:
            self.rotateLeft(node.leftChild)
            self.rotateRight(node)
         else:
            self.rotateRight(node)
```
*Listing 3*

通过保持树在所有时间的平衡，我们可以确保 get 方法将按 $$O(log2(n))$$ 时间运行。但问题是我们的 put 方法有什么成本？让我们将它分解为 put 执行的操作。由于将新节点作为叶子插入，更新所有父节点的平衡因子将需要最多 $$log2^n$$ 运算，树的每层一个运算。如果发现子树不平衡，则需要最多两次旋转才能使树重新平衡。但是，每个旋转在 $$O(1)$$ 时间中工作，因此我们的put操作仍然是 $$O(log2^n)$$。

在这一点上，我们已经实现了一个功能AVL树，除非你需要删除一个节点的能力。我们保留删除节点和随后的更新和重新平衡作为一个练习。
