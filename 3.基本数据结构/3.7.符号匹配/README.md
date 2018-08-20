## 3.7.符号匹配

上面显示的匹配括号问题是许多编程语言都会出现的一般情况的特定情况。匹配和嵌套不同种类的开始和结束符号的情况经常发生。例如，在 Python 中，方括号 `[` 和 `]` 用于列表，花括号 `{` 和 `}` 用于字典。括号 `(` 和 `)` 用于元组和算术表达式。只要每个符号都能保持自己的开始和结束关系，就可以混合符号。符号字符串如

```
{ { ( [ ] [ ] ) } ( ) }

[ [ { { ( ( ) ) } } ] ]

[ ] [ ] [ ] ( ) { }
```

这些被恰当的匹配了，因为不仅每个开始符号都有对应的结束符号，而且符号的类型也匹配。

相反这些字符串没法匹配：

````
( [ ) ]

( ( ( ) ] ) )

[ { ( ) ]
````

上节的简单括号检查程序可以轻松的扩展处理这些新类型的符号。回想一下，每个开始符号被简单的压入栈中，等待匹配的结束符号出现。当出现结束符号时，唯一的区别是我们必须检查确保它正确匹配栈顶部开始符号的类型。如果两个符号不匹配，则字符串不匹配。如果整个字符串都被处理完并且没有什么留在栈中，则字符串匹配。

Python 程序见 ActiveCode 1。唯一的变化是 16 行，我们称之为辅助函数匹配。必须检查栈中每个删除的符号，以查看它是否与当前结束符号匹配。如果不匹配，则布尔变量 balanced 被设置为 False。

```python
from pythonds.basic.stack import Stack

def parChecker(symbolString):
    s = Stack()
    balanced = True
    index = 0
    while index < len(symbolString) and balanced:
        symbol = symbolString[index]
        if symbol in "([{":
            s.push(symbol)
        else:
            if s.isEmpty():
                balanced = False
            else:
                top = s.pop()
                if not matches(top,symbol):
                       balanced = False
        index = index + 1
    if balanced and s.isEmpty():
        return True
    else:
        return False

def matches(open,close):
    opens = "([{"
    closers = ")]}"
    return opens.index(open) == closers.index(close)


print(parChecker('{{([][])}()}'))
print(parChecker('[{()]'))

```

*ActiveCode 1*

这两个例子表明，栈是计算机语言结构处理非常重要的数据结构。几乎你能想到的任何嵌套符号必须按照平衡匹配的顺序。栈还有其他重要的用途，我们将在接下来的部分讨论。
