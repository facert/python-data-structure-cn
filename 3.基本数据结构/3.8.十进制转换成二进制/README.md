## 3.8.十进制转换成二进制

在你学习计算机的过程中，你可能已经接触了二进制。二进制在计算机科学中是很重要的，因为存储在计算机内的所有值都是以 0 和 1 存储的。如果没有能力在二进制数和普通字符串之间转换，我们与计算机之间的交互非常棘手。

整数值是常见的数据项。他们一直用于计算机程序和计算。我们在数学课上学习它们，当然最后用十进制或者基数 10 来表示它们。十进制 $$233_{10}$$ 以及对应的二进制表示 $$11101001_2$$ 分别解释为

$$
2\times10^{2} + 3\times10^{1} + 3\times10^{0}
$$

和

$$
1\times2^{7} + 1\times2^{6} + 1\times2^{5} + 0\times2^{4} + 1\times2^{3} + 0\times2^{2} + 0\times2^{1} + 1\times2^{0}
$$

但是我们如何能够容易地将整数值转换为二进制呢？答案是“除 2”算法，它用栈来跟踪二进制结果的数字。

“除 2” 算法假定我们从大于 0 的整数开始。不断迭代的将十进制除以 2，并跟踪余数。第一个除以 2 的余数说明了这个值是偶数还是奇数。偶数有 0 的余数，记为 0。奇数有余数 1，记为 1.我们将得到的二进制构建为数字序列，第一个余数实际上是序列中的最后一个数字。见 Figure 5 , 我们再次看到了反转的属性，表示栈可能是解决这个问题的数据结构。

![3.8.十进制转换成二进制.figure5](assets/3.8.%E5%8D%81%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%8D%A2%E6%88%90%E4%BA%8C%E8%BF%9B%E5%88%B6.figure5.png)

*Figure 5*

Activecode 1 中的 Python 代码实现了 “除 2” 算法，函数 divideBy2 传入了一个十进制的参数，并重复除以 2。第 7 行使用内置的模运算符 % 来提取余数，第 8 行将余数压到栈上。当除到 0 后，11-13 行构造了一个二进制字符串。

```python
from pythonds.basic.stack import Stack

def divideBy2(decNumber):
    remstack = Stack()

    while decNumber > 0:
        rem = decNumber % 2
        remstack.push(rem)
        decNumber = decNumber // 2

    binString = ""
    while not remstack.isEmpty():
        binString = binString + str(remstack.pop())

    return binString

print(divideBy2(42))

```

*ActiveCode 1*

这个用于二进制转换的算法可以很容易的扩展以执行任何基数的转换。在计算机科学中，通常会使用很多不同的编码。其中最常见的是二级制，八进制和十六进制。

十进制 $$233$$ 和它对应的八进制和十六进制 $$351_8$$, $$E9_{16}$$

$$
3\times8^{2} + 5\times8^{1} + 1\times8^{0}
$$

和

$$
14\times16^{1} + 9\times16^{0}
$$

可以修改 `divideBy2` 函数，使它不仅能接受十进制参数，还能接受预期转换的基数。‘除 2’ 的概念被简单的替换成更通用的 ‘除基数’。在 ActiveCode2 展示的是一个名为 `baseConverter` 函数。采用十进制数和 2 到 16 之间的任何基数作为参数。余数部分仍然入栈，直到被转换的值为 0。我们创建一组数字，用来表示超过 9 的余数。

```python
from pythonds.basic.stack import Stack

def baseConverter(decNumber,base):
    digits = "0123456789ABCDEF"

    remstack = Stack()

    while decNumber > 0:
        rem = decNumber % base
        remstack.push(rem)
        decNumber = decNumber // base

    newString = ""
    while not remstack.isEmpty():
        newString = newString + digits[remstack.pop()]

    return newString

print(baseConverter(25,2))
print(baseConverter(25,16))
```

*ActiveCode2*
