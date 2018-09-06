## 4.12.动态规划

计算机科学中的许多程序是为了优化一些值而编写的; 例如，找到两个点之间的最短路径，找到最适合一组点的线，或找到满足某些标准的最小对象集。计算机科学家使用许多策略来解决这些问题。本书的目标之一是向你展示几种不同的解决问题的策略。`动态规划` 是这些类型的优化问题的一个策略。

优化问题的典型例子包括使用最少的硬币找零。假设你是一个自动售货机制造商的程序员。你的公司希望通过给每个交易最少硬币来简化工作。假设客户放入 1 美元的钞票并购买 37 美分的商品。你可以用来找零的最小数量的硬币是多少？答案是六个硬币：两个 25 美分，一个 10美分 和 三个一美分。我们如何得到六个硬币的答案？我们从最大的硬币（25 美分）开始，并尽可能多，然后我们去找下一个小点的硬币，并尽可能多的使用它们。这第一种方法被称为贪婪方法，因为我们试图尽快解决尽可能大的问题。

当我们使用美国货币时，贪婪的方法工作正常，但是假设你的公司决定在埃尔博尼亚部署自动贩卖机，除了通常的 1，5，10 和 25 分硬币，他们还有一个 21 分硬币 。在这种情况下，我们的贪婪的方法找不到 63 美分的最佳解决方案。 随着加入 21分硬币，贪婪的方法仍然会找到解决方案是六个硬币。然而，最佳答案是三个 21 分。

让我们看一个方法，我们可以确定会找到问题的最佳答案。由于这一节是关于递归的，你可能已经猜到我们将使用递归解决方案。让我们从基本情况开始，如果我们可以与我们硬币的价值相同的金额找零，答案很容易，一个硬币。

如果金额不匹配，我们有几个选项。我们想要的是最低一个一分钱加上原始金额减去一分钱所需的硬币数量，或者一个 5 美分加上原始金额减去 5 美分所需的硬币数量，或者一个 10 美分加上原始金额减去 10 美分所需的硬币数量，等等。因此，需要对原始金额找零硬币数量可以根据下式计算：

![4.12.动态规划.1](assets/4.12.%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92.1.png)

执行我们刚才描述的算法如 Listing 7 所示。在第 3 行，我们检查基本情况;也就是说，我们正试图支付硬币的确切金额。如果我们没有等于找零数量的硬币，我们递归调用每个小于找零额的不同的硬币值。第 6 行显示了我们如何使用列表推导将硬币列表过滤到小于当前找零的硬币列表。递归调用也减少了由所选硬币的值所需要的总找零量。递归调用在第 7 行。注意在同一行，我们将硬币数 `+1` ，以说明我们正在使用一个硬币的事实。

```python
def recMC(coinValueList,change):
   minCoins = change
   if change in coinValueList:
     return 1
   else:
      for i in [c for c in coinValueList if c <= change]:
         numCoins = 1 + recMC(coinValueList,change-i)
         if numCoins < minCoins:
            minCoins = numCoins
   return minCoins

print(recMC([1,5,10,25],63))
```

*Listing 7*

Listing 7 中的算法是非常低效的。事实上，它需要 `67,716,925` 个递归调用来找到 4 个硬币的最佳解决 63 美分问题的方案。要理解我们方法中的致命缺陷，请参见 Figure 5，其中显示了 377 个函数调用所需的一小部分，找到支付 26 美分的最佳硬币。

图中的每个节点对应于对 `recMC` 的调用。节点上的标签表示硬币数量的变化量。箭头上的标签表示我们刚刚使用的硬币。通过跟随图表，我们可以看到硬币的组合。主要的问题是我们重复做了太多的计算。例如，该图表示该算法重复计算了至少三次支付 15 美分。这些计算找到 15 美分的最佳硬币数量的步骤本身需要52个函数调用。显然，我们浪费了大量的时间和精力重新计算旧的结果。

![4.12.动态规划.figure5](assets/4.12.%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92.figure5.png)

*Figure 5*

减少我们工作量的关键是记住一些过去的结果，这样我们可以避免重新计算我们已经知道的结果。一个简单的解决方案是将最小数量的硬币的结果存储在表中。然后在计算新的最小值之前，我们首先检查表，看看结果是否已知。如果表中已有结果，我们使用表中的值，而不是重新计算。 ActiveCode 1 显示了一个修改的算法，以合并我们的表查找方案。

```python
def recDC(coinValueList,change,knownResults):
   minCoins = change
   if change in coinValueList:
      knownResults[change] = 1
      return 1
   elif knownResults[change] > 0:
      return knownResults[change]
   else:
       for i in [c for c in coinValueList if c <= change]:
         numCoins = 1 + recDC(coinValueList, change-i,
                              knownResults)
         if numCoins < minCoins:
            minCoins = numCoins
            knownResults[change] = minCoins
   return minCoins

print(recDC([1,5,10,25],63,[0]*64))
```

*ActiveCode 1*

注意，在第 6 行中，我们添加了一个测试，看看我们的列表是否包含此找零的最小硬币数量。如果没有，我们递归计算最小值，并将计算出的最小值存储在列表中。使用这个修改的算法减少了我们需要为四个硬币递归调用的数量，63美分问题只需 221 次调用！

虽然 AcitveCode 1 中的算法是正确的。事实上，我们所做的不是动态规划，而是我们通过使用称为 `记忆化` 的技术来提高我们的程序的性能，或者更常见的叫做 `缓存`。

一个真正的动态编程算法将采取更系统的方法来解决这个问题。我们的动态编程解决方案将从找零一分钱开始，并系统地找到我们需要的找零额。这保证我们在算法的每一步，已经知道为任何更小的数量进行找零所需的最小硬币数量。

让我们看看如何找到 11 美分所需的最小找零数量。Figure 4 说明了该过程。我们从一分钱开始。唯一的解决方案是一个硬币（一分钱）。下一行显示一分和两分的最小值。再次，唯一的解决方案是两分钱。第五行事情变得有趣。现在我们有两个选择，五个一分钱或一个五分钱。我们如何决定哪个是最好的？我们查阅表，看到需要找零四美分的硬币数量是四，再加一个一分钱是五，等于五个硬币。或者我们可以尝试 0 分加一个五分，五分钱等于一个硬币。由于一和五最小的是一，我们在表中存储为一。再次快进到表的末尾，考虑 11 美分。Figure 5 展示了我们要考虑的三个选项：

1. 一个一分钱加上 `11-1 = 10分（1）` 的最小硬币数
2. 一个五分钱加上 `11-5 = 6分（2）`的最小硬币数
3. 一个十分钱加上 `11-10 = 1 分（1）`最小硬币数

选项 1 或 3 总共需要两个硬币，这是 11 美分的最小硬币数。
![4.12.动态规划.figure6](assets/4.12.%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92.figure6.png)

Listing 8 用一个动态规划算法来解决我们的找零问题。 `dpMakeChange` 有三个参数：一个有效硬币值的列表，我们要求的找零额，以及一个包含每个值所需最小硬币数量的列表。 当函数完成时，`minCoins` 将包含从 0 到找零值的所有值的解。

```python
def dpMakeChange(coinValueList,change,minCoins):
   for cents in range(change+1):
      coinCount = cents
      for j in [c for c in coinValueList if c <= cents]:
            if minCoins[cents-j] + 1 < coinCount:
               coinCount = minCoins[cents-j]+1
      minCoins[cents] = coinCount
   return minCoins[change]
```

*Listing 8*

注意，`dpMakeChange` 不是递归函数，即使我们开始使用递归解决这个问题。重要的是要意识到，你可以为问题写一个递归解决方案但并不意味着它是最好的或最有效的解决方案。在这个函数中的大部分工作是通过从第 4 行开始的循环来完成的。在这个循环中，我们考虑使用所有可能的硬币对指定的金额进行找零。就像我们上面的 11 分的例子，我们记住最小值，并将其存储在我们的 `minCoins` 列表。

虽然我们的找零算法很好地找出最小数量的硬币，但它不帮助我们找零，因为我们不跟踪我们使用的硬币。我们可以轻松地扩展 `dpMakeChange` 来跟踪硬币使用，只需记住我们为每个条目添加的最后一个硬币到 `minCoins` 表。如果我们知道添加的最后一个硬币值，我们可以简单地减去硬币的值，在表中找到前一个条目，找到该金额的最后一个硬币。我们可以通过表继续跟踪，直到我们开始的位置。

ActiveCode 2 展示了 `dpMakeChange` 算法修改为跟踪使用的硬币，以及一个函数 `printCoins` 通过表打印出使用的每个硬币的值。前两行主要设置要找零的金额，并创建使用的硬币列表。 接下来的两行创建了我们需要存储结果的列表。`coinsUsed` 是用于找零的硬币的列表，并且 `coinCount` 是与列表中的位置相对应进行找零的最小硬币数。

注意，我们打印的硬币直接来自 `coinsUsed` 数组。对于第一次调用，我们从数组位置 `63` 开始，然后打印 `21`。然后我们取 `63-21 = 42`，看看列表的第 42 个元素。我们再次找到 21 存储在那里。 最后，数组第 21 个元素21 也包含 21，得到三个 21。

```python
def dpMakeChange(coinValueList,change,minCoins,coinsUsed):
   for cents in range(change+1):
      coinCount = cents
      newCoin = 1
      for j in [c for c in coinValueList if c <= cents]:
            if minCoins[cents-j] + 1 < coinCount:
               coinCount = minCoins[cents-j]+1
               newCoin = j
      minCoins[cents] = coinCount
      coinsUsed[cents] = newCoin
   return minCoins[change]

def printCoins(coinsUsed,change):
   coin = change
   while coin > 0:
      thisCoin = coinsUsed[coin]
      print(thisCoin)
      coin = coin - thisCoin

def main():
    amnt = 63
    clist = [1,5,10,21,25]
    coinsUsed = [0]*(amnt+1)
    coinCount = [0]*(amnt+1)

    print("Making change for",amnt,"requires")
    print(dpMakeChange(clist,amnt,coinCount,coinsUsed),"coins")
    print("They are:")
    printCoins(coinsUsed,amnt)
    print("The used list is as follows:")
    print(coinsUsed)

main()
```

*AcitveCode 2*

```python
Making change for 63 requires
3 coins
They are:
21
21
21
The used list is as follows:
[1, 1, 1, 1, 1, 5, 1, 1, 1, 1, 10, 1, 1, 1, 1, 5, 1, 1, 1, 1, 10, 21, 1, 1, 1, 25, 1, 1, 1, 1, 5, 10, 1, 1, 1, 10, 1, 1, 1, 1, 5, 10, 21, 1, 1, 10, 21, 1, 1, 1, 25, 1, 10, 1, 1, 5, 10, 1, 1, 1, 10, 1, 10, 21]
```
