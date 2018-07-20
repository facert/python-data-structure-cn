## 1.11.处理异常

在编写程序时通常会出现两种类型的错误。第一个被称为语法错误，仅仅意味着程序员在语句或表达式的结构中犯了一个错误。例如，写一个for语句而忘记冒号是错误的。

````
>>> for i in range(10)
SyntaxError: invalid syntax (<pyshell#61>, line 1)
````

在这种情况下，Python解释器发现它不能完成这个指令的处理，因为它不符合语言的规则。当你第一次学习一门语言时，语法错误通常会更频繁。

另一种类型的错误，称为逻辑错误，表示程序执行但给出错误结果的情况。 这可能是由于基础算法中的错误或该算法的转换错误。 在某些情况下，逻辑错误会导致非常糟糕的情况，例如尝试除以零或尝试访问列表中的索引超出列表边界的项目。 在这种情况下，逻辑错误会导致运行时错误，导致程序终止。 这些类型的运行时错误通常称为exceptions。

大多数时候，小白程序员只是把异常看作是导致执行结束的致命的运行时错误。然而，大多数编程语言提供了一种处理这些错误的方法，这些错误将允许程序员在选择时进行某种类型的干预。此外，如果程序员能保证检查到程序执行中的情况，就可以创建自己的异常。

当一个异常发生时，我们说它已经被“抛出”了。使用try语句处理所提出的异常。例如，思考下面的会话，让用户输入一个整数，然后从数学库调用平方根函数。如果用户输入大于或等于0的值，打印将显示平方根。但是，如果用户输入负值，平方根函数将报出ValueError异常。

````
>>> anumber = int(input("Please enter an integer "))
Please enter an integer -23
>>> print(math.sqrt(anumber))
Traceback (most recent call last):
  File "<pyshell#102>", line 1, in <module>
    print(math.sqrt(anumber))
ValueError: math domain error
>>>
````

我们可以通过在try块中调用print函数来处理这个异常。相应的except块“捕获”异常，并在发生异常时将消息打印给用户。例如:

````
>>> try:
       print(math.sqrt(anumber))
    except:
       print("Bad Value for square root")
       print("Using absolute value instead")
       print(math.sqrt(abs(anumber)))

Bad Value for square root
Using absolute value instead
4.79583152331
>>>
````

它将捕获sqrt引发的异常，将消息打印回用户并使用绝对值来确保我们采用非负数的平方根。 这意味着程序不会终止，而是继续执行下一个语句。

程序员也可以通过使用raise语句来引发运行时异常。 例如，我们可以先检查该值，然后引发自己的异常，而不是使用负数调用平方根函数。 下面的代码片段显示了创建新的RuntimeError异常的结果。 请注意，程序仍然会终止，但现在导致终止的异常是程序员明确创建的。

````
>>> if anumber < 0:
...    raise RuntimeError("You can't use a negative number")
... else:
...    print(math.sqrt(anumber))
...
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
RuntimeError: You can't use a negative number
>>>
````

除了上面显示的RuntimeError之外，还可以引发多种异常。 有关所有可用异常类型的列表以及如何创建自己的异常类型，请参阅Python参考手册。



