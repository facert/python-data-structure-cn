## 4.8.谢尔宾斯基三角形

另一个展现自相似性的分形是谢尔宾斯基三角形。 Figure 3 是一个示例。谢尔宾斯基三角形阐明了三路递归算法。用手绘制谢尔宾斯基三角形的过程很简单。 从一个大三角形开始。通过连接每一边的中点，将这个大三角形分成四个新的三角形。忽略刚刚创建的中间三角形，对三个小三角形中的每一个应用相同的过程。 每次创建一组新的三角形时，都会将此过程递归应用于三个较小的角三角形。 如果你有足够的铅笔，你可以无限重复这个过程。在继续阅读之前，你可以尝试运用所描述的方法自己绘制谢尔宾斯基三角形。

![4.8.谢尔宾斯基三角形.figure3](assets/4.8.%E8%B0%A2%E5%B0%94%E5%AE%BE%E6%96%AF%E5%9F%BA%E4%B8%89%E8%A7%92%E5%BD%A2.figure3.png)

*Figure 3*

因为我们可以无限地应用算法，什么是基本情况？ 我们将看到，基本情况被任意设置为我们想要将三角形划分成块的次数。有时我们把这个数字称为分形的“度”。 每次我们进行递归调用时，我们从度中减去 1，直到 0。当我们达到 0 度时，我们停止递归。在 Figure 3 中生成谢尔宾斯基三角形的代码见 ActiveCode 1。

```python
import turtle

def drawTriangle(points,color,myTurtle):
    myTurtle.fillcolor(color)
    myTurtle.up()
    myTurtle.goto(points[0][0],points[0][1])
    myTurtle.down()
    myTurtle.begin_fill()
    myTurtle.goto(points[1][0],points[1][1])
    myTurtle.goto(points[2][0],points[2][1])
    myTurtle.goto(points[0][0],points[0][1])
    myTurtle.end_fill()

def getMid(p1,p2):
    return ( (p1[0]+p2[0]) / 2, (p1[1] + p2[1]) / 2)

def sierpinski(points,degree,myTurtle):
    colormap = ['blue','red','green','white','yellow',
                'violet','orange']
    drawTriangle(points,colormap[degree],myTurtle)
    if degree > 0:
        sierpinski([points[0],
                        getMid(points[0], points[1]),
                        getMid(points[0], points[2])],
                   degree-1, myTurtle)
        sierpinski([points[1],
                        getMid(points[0], points[1]),
                        getMid(points[1], points[2])],
                   degree-1, myTurtle)
        sierpinski([points[2],
                        getMid(points[2], points[1]),
                        getMid(points[0], points[2])],
                   degree-1, myTurtle)

def main():
   myTurtle = turtle.Turtle()
   myWin = turtle.Screen()
   myPoints = [[-100,-50],[0,100],[100,-50]]
   sierpinski(myPoints,3,myTurtle)
   myWin.exitonclick()

main()

```

*Activecode 1*

ActiveCode 1 中的程序遵循上述概念。谢尔宾斯基的第一件事是绘制外三角形。接下来，有三个递归调用，每个使我们在连接中点获得新的三角形。我们再次使用 Python 附带的 `turtle` 模块。你可以通过使用 `help('turtle')` 了解 `turtle` 可用方法的详细信息。

看下代码，想想绘制三角形的顺序。虽然三角的确切顺序取决于如何指定初始集，我们假设三角按左下，上，右下顺序。由于谢尔宾斯基函数调用自身，谢尔宾斯基以它的方式递归到左下角最小的三角形，然后开始填充其余的三角形。填充左下角顶角中的小三角形。最后，它填充在左下角中右下角的最小三角形。

有时，根据函数调用图来考虑递归算法是有帮助的。Figure 4 展示了递归调用总是向左移动。活动函数以黑色显示，非活动函数显示为灰色。向 Figure 4 底部越近，三角形越小。该功能一次完成一次绘制; 一旦它完成了绘制，它移动到左下方底部中间位置，然后继续这个过程。

![4.8.谢尔宾斯基三角形.figure4](assets/4.8.%E8%B0%A2%E5%B0%94%E5%AE%BE%E6%96%AF%E5%9F%BA%E4%B8%89%E8%A7%92%E5%BD%A2.figure4.png)

*Figure 4*

谢尔宾斯基函数在很大程度上依赖于 `getMid` 函数。 `getMid` 接受两个端点作为参数，并返回它们之间的中点。 此外，ActiveCode 1 还有一个函数，使用 `begin_fill` 和 `end_fill` 方法绘制填充一个三角形。
