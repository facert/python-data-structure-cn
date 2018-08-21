## 3.5.Python实现栈

现在我们已经将栈清楚地定义了抽象数据类型，我们将把注意力转向使用 Python 实现栈。回想一下，当我们给抽象数据类型一个物理实现时，我们将实现称为数据结构。

正如我们在第1章中所描述的，在 Python 中，与任何面向对象编程语言一样，抽象数据类型（如栈）的选择的实现是创建一个新类。栈操作实现为类的方法。此外，为了实现作为元素集合的栈，使用由 Python 提供的原语集合的能力是有意义的。 我们将使用列表作为底层实现。

回想一下，Python 中的列表类提供了有序集合机制和一组方法。例如，如果我们有列表 [2,5,3,6,7,4]，我们只需要确定列表的哪一端将被认为是栈的顶部。一旦确定，可以使用诸如 append 和 pop 的列表方法来实现操作。

以下栈实现（ActiveCode 1）假定列表的结尾将保存栈的顶部元素。随着栈增长（push 操作），新项将被添加到列表的末尾。 pop 也操作列表末尾的元素。

```python
class Stack:
     def __init__(self):
         self.items = []

     def isEmpty(self):
         return self.items == []

     def push(self, item):
         self.items.append(item)

     def pop(self):
         return self.items.pop()

     def peek(self):
         return self.items[len(self.items)-1]

     def size(self):
         return len(self.items)

```

*ActiveCode 1*

记住我们只定义类的实现，我们需要创建一个栈，然后使用它。ActiveCode 2 展示了我们通过实例化 Stack 类执行 Table 1中的操作。注意，Stack 类的定义是从 pythonds 模块导入的。

> **Note**
pythonds 模块包含本书中讨论的所有数据结构的实现。它根据以下部分构造：基本数据类型，树和图。 该模块可以从 [pythonworks.org](http://www.pythonworks.org/pythonds)下载。

```python
from pythonds.basic.stack import Stack

s=Stack()

print(s.isEmpty())
s.push(4)
s.push('dog')
print(s.peek())
s.push(True)
print(s.size())
print(s.isEmpty())
s.push(8.4)
print(s.pop())
print(s.pop())
print(s.size())

```

*ActiveCode 2*
