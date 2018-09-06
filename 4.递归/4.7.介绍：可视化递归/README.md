## 4.7.介绍：可视化递归

在上一节中，我们讨论了一些使用递归很容易解决的问题; 然而，我们可能很难找到一个模型或一种可视化方法知道在递归函数中发生了什么。这使得递归难以让人掌握。在本节中，我们将看到几个使用递归绘制一些有趣图片的例子。当你看到这些图片的形状，你会对递归过程有新的认识，可能有助于巩固你对递归理解。

我们使用的插图的工具是 Python 的 `turtle` 模块称为 `turtle`。`turtle` 是 Python 所有版本的标准库，并且非常易于使用。比喻很简单。你可以创建一只乌龟，乌龟能前进，后退，左转，右转等。乌龟可以让它的尾巴或上或下。当乌龟的尾巴向下，它移动时会画一条线。为了增加乌龟的艺术价值，你可以改变尾巴的宽度以及尾巴浸入的墨水的颜色。

这里有一个简单的例子来说明龟图形基础。我们将使用 `turtle` 模块递归绘制螺旋。 见 ActiveCode 1。导入 `turtle` 模块后，我们创建一个乌龟。当乌龟被创建时，它也创建一个窗口来绘制。接下来我们定义 `drawSpir​​al` 函数。这个简单函数的基本情况是当我们想要绘制的线的长度（由 len 参数给出）减小到零或更小时。如果线的长度大于零，我们让乌龟以 len 单位前进，然后向右转 90 度。当我们再次调用 `drawSpir​​al` 并缩短长度时递归。在ActiveCode 1 结束时，你会注意到我们调用函数 `myWin.exitonclick()`，这是一个方便的缩小窗口的方法，使乌龟进入等待模式，直到你单击窗口，然后程序清理并退出。

```python
import turtle

myTurtle = turtle.Turtle()
myWin = turtle.Screen()

def drawSpiral(myTurtle, lineLen):
    if lineLen > 0:
        myTurtle.forward(lineLen)
        myTurtle.right(90)
        drawSpiral(myTurtle,lineLen-5)

drawSpiral(myTurtle,100)
myWin.exitonclick()
```

这是关于你知道的所有龟图形，以制作一些令人印象深刻的涂鸦。我们的下一个程序，将绘制一个分形树。分形来自数学的一个分支，并且与递归有很多共同之处。分形的定义是，当你看着它时，无论你放大多少，分形有相同的基本形状。大自然的一些例子是大陆的海岸线，雪花，山脉，甚至树木或灌木。这些自然现象中的许多的分形性质使得程序员能够为计算机生成的电影生成非常逼真的风景。在我们的下一个例子中，将生成一个分形树。

要理解这如何工作，需要想一想如何使用分形词汇来描述树。记住，我们上面说过，分形是在所有不同的放大倍率下看起来是一样的。如果我们将它翻译成树木和灌木，我们可能会说，即使一个小树枝也有一个整体树的相同的形状和特征。有了这个想法，我们可以说一棵树是树干，一棵较小的树向右走，另一棵较小的树向左走。如果你用递归的思想考虑这个定义，这意味着我们将树的递归定义应用到较小的左树和右树。

让我们把这个想法转换成一些 Python 代码。Listing 1 展示了如何使用我们的乌龟来生成分形树。让我们更仔细地看一下代码。你会看到在第 5 行和第 7 行，我们正在进行递归调用。在第 5 行，我们在乌龟向右转 20 度之后立即进行递归调用;这是上面提到的右树。然后在第 7 行，乌龟进行另一个递归调用，但这一次后左转 40 度。乌龟必须向左转 40 度的原因是，它需要撤消原来的向右转 20 度，然后再向左转 20 度，以绘制左树。还要注意，每次我们对树进行递归调用时，我们从 `branchLen` 参数中减去一些量; 这是为了确保递归树越来越小。你还应该看到到第 2 行的初始 if 语句是检查 `branchLen` 的基本情况大小。
 
```python
def tree(branchLen,t):
    if branchLen > 5:
        t.forward(branchLen)
        t.right(20)
        tree(branchLen-15,t)
        t.left(40)
        tree(branchLen-10,t)
        t.right(20)
        t.backward(branchLen)
```

*Listing 1*

此树示例的完整程序在 ActiveCode 2 中。在运行代码之前，请思考你希望看到的树形状。看着递归调用，并想想这棵树将如何展开。它会对称地绘制树的右半边和左半边吗？ 它会先绘制右侧然后左侧？

```python
import turtle

def tree(branchLen,t):
    if branchLen > 5:
        t.forward(branchLen)
        t.right(20)
        tree(branchLen-15,t)
        t.left(40)
        tree(branchLen-15,t)
        t.right(20)
        t.backward(branchLen)

def main():
    t = turtle.Turtle()
    myWin = turtle.Screen()
    t.left(90)
    t.up()
    t.backward(100)
    t.down()
    t.color("green")
    tree(75,t)
    myWin.exitonclick()

main()

```

*Activecode 2*

![4.7.介绍：可视化递归.a](assets/4.7.%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%8F%AF%E8%A7%86%E5%8C%96%E9%80%92%E5%BD%92.ac1.png)

注意树上的每个分支点如何对应于递归调用，并注意树的右半部分如何一直绘制到它的最短的树枝。你可以在 Figure 1 中看到这一点。现在，注意程序如何工作，它的方式是直到树的整个右侧绘制完成回到树干。你可以在 Figure 2 中看到树的右半部分。然后绘制树的左侧，但不是尽可能远地向左移动。相反，直到我们进入到左树最小的枝干，左树的右半部分才开始绘制。

![4.7.介绍：可视化递归.figure1](assets/4.7.%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%8F%AF%E8%A7%86%E5%8C%96%E9%80%92%E5%BD%92.figure1.png)

*Figure 1*

![4.7.介绍：可视化递归.figure2](assets/4.7.%E4%BB%8B%E7%BB%8D%EF%BC%9A%E5%8F%AF%E8%A7%86%E5%8C%96%E9%80%92%E5%BD%92.figure2.png)
*Figure 2*

这个简单的树程序只是一个起点，你会注意到树看起来不是特别现实，因为自然不像计算机程序那样对称。
