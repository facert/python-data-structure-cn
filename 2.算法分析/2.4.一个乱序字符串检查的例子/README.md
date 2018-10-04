## 2.4.一个乱序字符串检查的例子

显示不同量级的算法的一个很好的例子是字符串的乱序检查。乱序字符串是指一个字符串只是另一个字符串的重新排列。例如，`'heart'` 和 `'earth'` 就是乱序字符串。`'python'` 和 `'typhon'` 也是。为了简单起见，我们假设所讨论的两个字符串具有相等的长度，并且他们由 26 个小写字母集合组成。我们的目标是写一个布尔函数，它将两个字符串做参数并返回它们是不是乱序。

### 2.4.1.解法1:检查

我们对乱序问题的第一个解法是检查第一个字符串是不是出现在第二个字符串中。如果可以检验到每一个字符，那这两个字符串一定是乱序。可以通过用 None 替换字符来了解一个字符是否完成检查。但是，由于 Python 字符串是不可变的，所以第一步是将第二个字符串转换为列表。检查第一个字符串中的每个字符是否存在于第二个列表中，如果存在，替换成 None。见 ActiveCode1

```python
def anagramSolution1(s1,s2):
    alist = list(s2)

    pos1 = 0
    stillOK = True

    while pos1 < len(s1) and stillOK:
        pos2 = 0
        found = False
        while pos2 < len(alist) and not found:
            if s1[pos1] == alist[pos2]:
                found = True
            else:
                pos2 = pos2 + 1

        if found:
            alist[pos2] = None
            pos1 = pos1 + 1
        else:
            stillOK = False

    return stillOK and (len(filter(None, alist)) == 0)

print(anagramSolution1('abcd','dcba'))
```

*ActiveCode1*

为了分析这个算法，我们注意到 s1 的每个字符都会在 s2 中进行最多 n 个字符的迭代。s2 列表中的 n 个位置将被访问一次来匹配来自 s1 的字符。访问次数可以写成 1 到 n 整数的和，可以写成 

$$
\sum_{i=1}^n i=\frac{ n(n+1) }{2}=\frac{1}{2}n^2+\frac{1}{2}n
$$

当 n 变大，$$n^2$$ 这项占据主导，1/2 可以忽略。所以这个算法复杂度为 $$O(n^2)$$。

### 2.4.2.解法2:排序和比较

另一个解决方案是利用这么一个事实：即使 s1,s2 不同，它们都是由完全相同的字符组成的。所以，我们按照字母顺序从 a 到 z 排列每个字符串，如果两个字符串相同，那这两个字符串就是乱序字符串。见 ActiveCode2。

```python
def anagramSolution2(s1,s2):
    alist1 = list(s1)
    alist2 = list(s2)

    alist1.sort()
    alist2.sort()

    pos = 0
    matches = True

    while pos < len(s1) and matches:
        if alist1[pos]==alist2[pos]:
            pos = pos + 1
        else:
            matches = False

    return matches

print(anagramSolution2('abcde','edcba'))
```

*ActiveCode2*

首先你可能认为这个算法是 $$O(n)$$，因为只有一个简单的迭代来比较排序后的 n 个字符。但是，调用 Python 排序不是没有成本。正如我们将在后面的章节中看到的，排序通常是 $$O(n^2)$$ 或 $$O(nlogn)$$。所以排序操作比迭代花费更多。最后该算法跟排序过程有同样的量级。

### 2.4.3.解法3: 穷举法

解决这类问题的强力方法是穷举所有可能性。对于乱序检测，我们可以生成 s1 的所有乱序字符串列表，然后查看是不是有 s2。这种方法有一点困难。当 s1 生成所有可能的字符串时，第一个位置有 n 种可能，第二个位置有 n-1 种，第三个位置有 n-2 种，等等。总数为 $$n*(n-1)*(n-2)*...*3*2*1$$， 即 n!。虽然一些字符串可能是重复的，程序也不可能提前知道这样，所以他仍然会生成 n! 个字符串。

事实证明，n! 比 n^2 增长还快，事实上，如果 s1 有 20个字符长，则将有 20! = 2,432,902,008,176,640,000 个字符串产生。如果我们每秒处理一种可能字符串，那么需要 77,146,816,596 年才能过完整个列表。所以这不是很好的解决方案。

### 2.4.4.解法4: 计数和比较

我们最终的解决方法是利用两个乱序字符串具有相同数目的 a, b, c 等字符的事实。我们首先计算的是每个字母出现的次数。由于有 26 个可能的字符，我们就用 一个长度为 26 的列表，每个可能的字符占一个位置。每次看到一个特定的字符，就增加该位置的计数器。最后如果两个列表的计数器一样，则字符串为乱序字符串。见  ActiveCode 3

```python
def anagramSolution4(s1,s2):
    c1 = [0]*26
    c2 = [0]*26

    for i in range(len(s1)):
        pos = ord(s1[i])-ord('a')
        c1[pos] = c1[pos] + 1

    for i in range(len(s2)):
        pos = ord(s2[i])-ord('a')
        c2[pos] = c2[pos] + 1

    j = 0
    stillOK = True
    while j<26 and stillOK:
        if c1[j]==c2[j]:
            j = j + 1
        else:
            stillOK = False

    return stillOK

print(anagramSolution4('apple','pleap'))

```

*ActiveCode 3*

同样，这个方案有多个迭代，但是和第一个解法不一样，它不是嵌套的。两个迭代都是 n, 第三个迭代，比较两个计数列表，需要 26 步，因为有 26 个字母。一共 `T(n)=2n+26`，即 $$O(n)$$，我们找到了一个线性量级的算法解决这个问题。

在结束这个例子之前，我们来讨论下空间花费，虽然最后一个方案在线性时间执行，但它需要额外的存储来保存两个字符计数列表。换句话说，该算法牺牲了空间以获得时间。

很多情况下，你需要在空间和时间之间做出权衡。这种情况下，额外空间不重要，但是如果有数百万个字符，就需要关注下。作为一个计算机科学家，当给定一个特定的算法，将由你决定如何使用计算资源。
