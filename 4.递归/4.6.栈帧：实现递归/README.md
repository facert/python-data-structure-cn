## 4.6.栈帧：实现递归
假设不是将递归调用的结果与来自 convertString 的字符串拼接到 toStr，我们修改了算法，以便在进行递归调用之前将字符串入栈。此修改的算法的代码展示在 ActiveCode 1 中。

```python
from pythonds.basic.stack import Stack

rStack = Stack()

def toStr(n,base):
    convertString = "0123456789ABCDEF"
    while n > 0:
        if n < base:
            rStack.push(convertString[n])
        else:
            rStack.push(convertString[n % base])
        n = n // base
    res = ""
    while not rStack.isEmpty():
        res = res + str(rStack.pop())
    return res

print(toStr(1453,16))
```

*ActiveCode 1*

每次我们调用 toStr，我们在栈上推入一个字符。回到前面的例子，我们可以看到在第四次调用 toStr 之后，栈看起来像 Figure 5 。注意，现在我们可以简单地将字符从栈中弹出，并将它们连接成最终结果 “1010”。

![4.6.栈帧：实现递归.figure5](assets/4.6.%E6%A0%88%E5%B8%A7%EF%BC%9A%E5%AE%9E%E7%8E%B0%E9%80%92%E5%BD%92.figure5.png)

*Figure 5*

前面的例子让我们了解了 Python 如何实现一个递归函数调用。 当在 Python 中调用函数时，会分配一个栈来处理函数的局部变量。当函数返回时，返回值留在栈的顶部，以供调用函数访问。 Figure 6 说明了第 4 行返回语句后的调用栈。

![4.6.栈帧：实现递归.figure6](assets/4.6.%E6%A0%88%E5%B8%A7%EF%BC%9A%E5%AE%9E%E7%8E%B0%E9%80%92%E5%BD%92.figure6.png)

*Figure 6*

注意，对 `toStr(2//2,2)` 的调用在栈上返回值为 “1”。 然后，在表达式 `“1” + convertString[2％2]` 中使用此返回值替换函数调用`(toStr(1,2))`，这将在栈顶部留下字符串 “10”。 这样，Python 调用栈就代替了我们在 Listing 4 中明确使用的栈。在我们的列表求和示例中，你可以认为栈上的返回值取代了累加器变量。

栈帧还为函数使用的变量提供了一个作用域。 即使我们重复地调用相同的函数，每次调用都会为函数本地的变量创建一个新的作用域。
