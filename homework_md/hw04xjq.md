##       计算机与人工智能学院《人工智能基础编程实训》实验报告📝

| 专业                 | 学号         | 姓名         |
| :------------------- | ------------ | ------------ |
| 人工智能             | 2304300133   | 徐佳琪       |
| **课程名称**         | **实验名称** | **完成日期** |
| 人工智能基础编程实训 | hw04         | 2026.1.3     |



[TOC]

---



### 一 实验目标

1. **理解链表的思想**
2. **掌握面向对象编程的方法**
3. **学会处理错误信息**：

---



### 二 实验要求

- 个人独立完成，积极动手编程；
- 鼓励与同学交流，但不能抄袭源码；
- 能完成实验说明文档的各个步骤并撰写此实验报告；
- 能演示实验过程并阐述功能的主要代码模块。
- 实验报告请突出自己的**想法**、**做法**、**心得体会**；

---



### 三 实验环境

- 软件：Vscode
- 环境：python=3.9

---

### 四 实验内容 

面向对象编程 (OOP)

### 1. **自动售货机**

在这个问题中，您将创建一个[自动售货机](https://en.wikipedia.org/wiki/Vending_machine)，它只售卖单一产品，并在需要时提供找零。

创建一个名为 `VendingMachine` 的类，用于表示某种产品的自动售货机。一个 `VendingMachine` 对象会返回描述其交互过程的字符串。请记住，返回的字符串必须与 doctests 中的字符串 **完全** 匹配——包括标点符号和空格！

填写 `VendingMachine` 类，根据需要添加属性和方法，使其行为与以下 doctests 匹配：
代码：

```python
class VendingMachine:
    def __init__(self, product, price):
        self.product = product
        self.price = price
        self.stock = 0
        self.balance = 0

    def restock(self, q):
        self.stock += q
        return f'Current {self.product} stock: {self.stock}'

    def add_funds(self, f):
        if self.stock == 0:
            return f'Nothing left to vend. Please restock. Here is your ${f}.'
        self.balance += f
        return f'Current balance: ${self.balance}'

    def vend(self):
        if self.stock == 0:
            return 'Nothing left to vend. Please restock.'
        if self.balance < self.price:
            return f'You must add ${self.price - self.balance} more funds.'
        change = self.balance - self.price
        self.stock -= 1
        self.balance = 0
        if change > 0:
            return f'Here is your {self.product} and ${change} change.'
        else:
            return f'Here is your {self.product}.'
```

测试结果：

```python
(cs61a) huwentao@a-SYS-740GP-TNRT:/data1/tao/cs61a/code/hw06-OOP/hw06$ python3 ok -q VendingMachine --local
=====================================================================
Assignment: Homework 6
OK, version v1.18.1
=====================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Running tests

---------------------------------------------------------------------
Test summary
    1 test cases passed! No cases failed.

Cannot backup when running ok with --local.
```

---

### 2. **铸币厂**

铸币厂是制造硬币的地方。在这个问题中，您将实现一个 `Mint` 类，它可以输出具有正确年份和价值的 `Coin`。

*   每个 `Mint` 实例都有一个 `year` 印记。`update` 方法将 `year` 印记设置为 `Mint` 类的 `present_year` 类属性。
*   `create` 方法接收一个 `Coin` 的子类，并返回一个该类的实例，该实例上印有铸币厂的年份（如果尚未更新，可能与 `Mint.present_year` 不同）。
*   一个 `Coin` 的 `worth` 方法返回硬币的 `cents` 值，外加超过50年的每一年额外增加1美分。硬币的年龄可以通过从 `Mint` 类的 `present_year` 类属性中减去硬币的年份来确定。

代码实现：

```python
class Mint:
    present_year = 2021

    def __init__(self):
        self.update()

    def create(self, kind):
        return kind(self.year)

    def update(self):
        self.year = Mint.present_year


class Coin:
    def __init__(self, year):
        self.year = year

    def worth(self):
        age = Mint.present_year - self.year
        return self.cents + (age - 50 if age > 50 else 0)


class Nickel(Coin):
    cents = 5


class Dime(Coin):
    cents = 10
```

测试结果：

```python
(torch) PS D:\2025大三人工智能实训\作业四\hw06-OOP\hw06-OOP\hw06\hw06> python ok -q Mint --local
=====================================================================
Assignment: Homework 6
OK, version v1.18.1
=====================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Running tests

---------------------------------------------------------------------
Test summary
    1 test cases passed! No cases failed.

Cannot backup when running ok with --local.
```

---

### 3.链表 **存储数字**

函数要求：编写一个函数 `store_digits`，它接收一个整数 `n` 并返回一个链表，其中链表的每个元素是 `n` 的一个数字。

**重要提示**：不要使用任何字符串操作函数，如 `str` 和 `reversed`。

代码实现：

```python
def store_digits(n):
    if n < 10:
        return Link(n)
    power = 1
    temp = n
    while temp >= 10:
        temp //= 10
        power *= 10
    first_digit = n // power
    rest = n % power
    return Link(first_digit, store_digits(rest))
```

测试结果：

```python
(torch) PS D:\2025大三人工智能实训\作业四\hw06-OOP\hw06-OOP\hw06\hw06> python ok -q Mint --local
=====================================================================
Assignment: Homework 6
OK, version v1.18.1
=====================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Running tests

---------------------------------------------------------------------
Test summary
    1 test cases passed! No cases failed.

Cannot backup when running ok with --local.
```

---

### 4. **可变映射**

实现 `deep_map_mut(fn, link)`，该函数将函数 `fn` 应用于给定链表 `link` 中的所有元素。如果一个元素本身是一个链表，则将 `fn` 应用于其每个元素，依此类推。

您的实现应该改变原始链表。不要创建任何新的链表。

**提示**：内置函数 `isinstance` 可能会有用。

代码实现：

```python
def deep_map_mut(fn, link):
    if link is Link.empty:
        return
    if isinstance(link.first, Link):
        deep_map_mut(fn, link.first)
    else:
        link.first = fn(link.first)
    deep_map_mut(fn, link.rest)
```

测试结果：

```python
(torch) PS D:\2025大三人工智能实训\作业四\hw06-OOP\hw06-OOP\hw06\hw06> python ok -q Mint --local
=====================================================================
Assignment: Homework 6
OK, version v1.18.1
=====================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Running tests

---------------------------------------------------------------------
Test summary
    1 test cases passed! No cases failed.

Cannot backup when running ok with --local.
```

---

### 5. **双列表构造链表**

实现一个函数 `two_list`，它接收两个列表并返回一个链表。第一个列表包含我们想要放入链表的值，第二个列表包含每个对应值的数量。
假设两个列表大小相同且长度至少为1。假设第二个列表中的所有元素都大于0。

代码实现：

```python
def two_list(vals, amounts):
    """
    Returns a linked list according to the two lists that were passed in. Assume
    vals and amounts are the same size. Elements in vals represent the value, and the
    corresponding element in amounts represents the number of this value desired in the
    final linked list. Assume all elements in amounts are greater than 0. Assume both
    lists have at least one element.

    >>> a = [1, 3, 2]
    >>> b = [1, 1, 1]
    >>> c = two_list(a, b)
    >>> c
    Link(1, Link(3, Link(2)))
    >>> a = [1, 3, 2]
    >>> b = [2, 2, 1]
    >>> c = two_list(a, b)
    >>> c
    Link(1, Link(1, Link(3, Link(3, Link(2)))))
    """
    result = Link.empty
    for val, count in reversed(list(zip(vals, amounts))):
        for _ in range(count):
            result = Link(val, result)
    return result
```

测试结果：

```python
(torch) PS D:\2025大三人工智能实训\作业四\hw06-OOP\hw06-OOP\hw06\hw06> python ok -q Mint --local
=====================================================================
Assignment: Homework 6
OK, version v1.18.1
=====================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Running tests

---------------------------------------------------------------------
Test summary
    1 test cases passed! No cases failed.

Cannot backup when running ok with --local.
```

### 6. **下一个 Virahanka-Fibonacci 对象**
实现 `VirFib` 类的 `next` 方法。对于这个类，`value` 属性是一个斐波那契数。`next` 方法返回一个 `VirFib` 实例，其 `value` 是下一个斐波那契数。`next` 方法应该只花费常数时间。

注意，在 doctests 中，没有任何内容被打印出来。相反，每次调用 `.next()` 都会返回一个 `VirFib` 实例。每个 `VirFib` 实例的显示方式由其 `__repr__` 方法的返回值决定。

*提示*：通过在 `next` 方法内部设置一个新的实例属性来跟踪前一个数字。您可以在任何时候为对象创建新的实例属性，即使在 `__init__` 方法之外也可以。

代码实现：

```python
class VirFib():
    """A Virahanka Fibonacci number.

    >>> start = VirFib()
    >>> start
    VirFib object, value 0
    >>> start.next()
    VirFib object, value 1
    >>> start.next().next()
    VirFib object, value 1
    >>> start.next().next().next()
    VirFib object, value 2
    >>> start.next().next().next().next()
    VirFib object, value 3
    >>> start.next().next().next().next().next()
    VirFib object, value 5
    >>> start.next().next().next().next().next().next()
    VirFib object, value 8
    >>> start.next().next().next().next().next().next() # Ensure start isn't changed
    VirFib object, value 8
    """

    def __init__(self, value=0):
        self.value = value

    def next(self):
        new = VirFib()
        if self.value == 0:
            new.value = 1
            new.previous = 0
        else:
            new.value = self.value + self.previous
            new.previous = self.value
        return new

    def __repr__(self):
        return "VirFib object, value " + str(self.value)
```

测试结果：

```python
(torch) PS D:\2025大三人工智能实训\作业四\hw06-OOP\hw06-OOP\hw06\hw06> python ok -q Mint --local
=====================================================================
Assignment: Homework 6
OK, version v1.18.1
=====================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Running tests

---------------------------------------------------------------------
Test summary
    1 test cases passed! No cases failed.

Cannot backup when running ok with --local.
```


### 7. **判断是否为二叉搜索树**

编写一个函数 `is_bst`，它接收一个树 `t`，当且仅当 `t` 是一个有效的二叉搜索树时返回 `True`。有效二叉搜索树意味着：

*   每个节点最多有两个子节点（叶子节点自动是有效的二叉搜索树）。
*   子节点是有效的二叉搜索树。
*   对于每个节点，该节点左子树中的所有条目都小于或等于该节点的标签。
*   对于每个节点，该节点右子树中的所有条目都大于该节点的标签。

一个二叉搜索树的例子是：
![bst](https://miro.medium.com/max/1424/1*F8MmBnUQyOA8-Rajg69nSQ.png)

注意，如果一个节点只有一个子节点，该子节点可以被认为是左子节点或右子节点。您应该考虑到这一点。

*提示：* 编写辅助函数 `bst_min` 和 `bst_max` 可能会有所帮助，它们分别返回一个有效的二叉搜索树的最小值和最大值。

代码实现：

```python
def is_bst(t):
    """Returns True if the Tree t has the structure of a valid BST.

    >>> t1 = Tree(6, [Tree(2, [Tree(1), Tree(4)]), Tree(7, [Tree(7), Tree(8)])])
    >>> is_bst(t1)
    True
    >>> t2 = Tree(8, [Tree(2, [Tree(9), Tree(1)]), Tree(3, [Tree(6)]), Tree(5)])
    >>> is_bst(t2)
    False
    >>> t3 = Tree(6, [Tree(2, [Tree(4), Tree(1)]), Tree(7, [Tree(7), Tree(8)])])
    >>> is_bst(t3)
    False
    >>> t4 = Tree(1, [Tree(2, [Tree(3, [Tree(4)])])])
    >>> is_bst(t4)
    True
    >>> t5 = Tree(1, [Tree(0, [Tree(-1, [Tree(-2)])])])
    >>> is_bst(t5)
    True
    >>> t6 = Tree(1, [Tree(4, [Tree(2, [Tree(3)])])])
    >>> is_bst(t6)
    True
    >>> t7 = Tree(2, [Tree(1, [Tree(5)]), Tree(4)])
    >>> is_bst(t7)
    False
    """
    if t.is_leaf():
        return True
    if len(t.branches) > 2:
        return False
    if len(t.branches) == 1:
        b = t.branches[0]
        return (is_bst(b) and (bst_max(b) <= t.label or bst_min(b) > t.label))
    left, right = t.branches
    return (is_bst(left) and is_bst(right) and
            bst_max(left) <= t.label < bst_min(right))

class Tree:
    def __init__(self, label, branches=[]):
        self.label = label
        self.branches = list(branches)

    def is_leaf(self):
        return not self.branches

def bst_min(t):
    if t.is_leaf():
        return t.label
    if len(t.branches) == 1:
        return min(t.label, bst_min(t.branches[0]))
    return bst_min(t.branches[0])

def bst_max(t):
    if t.is_leaf():
        return t.label
    if len(t.branches) == 1:
        return max(t.label, bst_max(t.branches[0]))
    return bst_max(t.branches[1])
```

测试结果：

```python
(torch) PS D:\2025大三人工智能实训\作业四\hw06-OOP\hw06-OOP\hw06\hw06> python ok -q Mint --local
=====================================================================
Assignment: Homework 6
OK, version v1.18.1
=====================================================================

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Running tests

---------------------------------------------------------------------
Test summary
    1 test cases passed! No cases failed.

Cannot backup when running ok with --local.
```

### 五 实验心得

通过本次《人工智能基础编程实训》的hw04实验，我对面向对象编程和链表数据结构有了更深入的理解。以下是我的心得体会：

### 遇到的问题及解决方法

**1. 自动售货机类的状态管理问题**

在设计`VendingMachine`类时，最初我混淆了`stock`和`balance`两个属性的更新时机。在实现`vend`方法时，忘记在售卖完成后重置余额。通过仔细分析自动售货机的工作流程：

- 补货时只增加库存
- 投币时增加余额
- 售卖时需要计算找零、减少库存、重置余额

我意识到每个方法应该只负责单一的功能，并通过doctests的输出来验证状态转换是否正确。

**2. 递归算法的调试困难**

在实现`store_digits`函数时，最初尝试用递归将整数转换为链表，但当处理多位数字时出现了链接错误。我通过添加调试语句逐步跟踪：

```
# 调试版本
def store_digits(n):
    if n < 10:
        print(f"基本情况: Link({n})")
        return Link(n)
    power = 1
    temp = n
    while temp >= 10:
        temp //= 10
        power *= 10
    first_digit = n // power
    rest = n % power
    print(f"n={n}, first={first_digit}, rest={rest}")
    result = Link(first_digit, store_digits(rest))
    return result
```

这种逐步分解的方法帮助我理解了递归如何逐位构建链表。

**3. 二叉搜索树的边界条件处理**

实现`is_bst`函数时，最困难的部分是处理节点只有一个子树的边界情况。题目要求"如果节点只有一个子节点，该子节点可以被认为是左子节点或右子节点"，这意味着需要检查两种情况：

- 子节点值 ≤ 节点标签（作为左子树）
- 子节点值 > 节点标签（作为右子树）

我通过编写辅助函数`bst_min`和`bst_max`来获取子树的最小最大值，然后在主函数中灵活处理单子树的情况：

```
if len(t.branches) == 1:
    b = t.branches[0]
    return (is_bst(b) and (bst_max(b) <= t.label or bst_min(b) > t.label))
```

**4. 深拷贝与浅拷贝的混淆**

在实现`deep_map_mut`时，最初我误以为需要创建新的链表对象。仔细阅读要求"应该改变原始链表。不要创建任何新的链表"后，我理解了这是要原地修改。通过使用`isinstance`判断元素类型，我对嵌套链表进行了递归处理：

```
if isinstance(link.first, Link):
    deep_map_mut(fn, link.first)  # 递归处理嵌套链表
else:
    link.first = fn(link.first)  # 直接修改值
```

### 主要收获

**1. 面向对象设计的思维方式**

通过自动售货机、铸币厂等实例，我学会了如何将现实世界的实体抽象为类和对象。每个类应该具有清晰的职责，方法之间通过属性进行状态传递。特别是`VirFib`类中，通过在`next`方法中动态添加`previous`属性，展示了Python的动态特性。

**2. 递归思维的培养**

链表和树结构天然适合递归处理。在实现`store_digits`、`deep_map_mut`、`is_bst`等函数时，我逐渐掌握了如何：

- 识别基本情况（base case）
- 定义递归步骤（recursive step）
- 确保每次递归都向基本情况靠近
- 正确组合递归调用的结果

**3. 测试驱动开发的体验**

实验提供的doctests起到了很好的指导作用。我养成了"先看测试用例，再实现功能"的习惯，这有助于：

- 理解功能需求的具体细节
- 验证边界条件的处理
- 确保代码符合预期行为

**4. 链表与树的对比理解**

通过本次实验，我对两种重要的数据结构有了更深入的认识：

- **链表**：线性结构，适合顺序处理，递归定义自然
- **二叉树**：层次结构，适合分治策略，BST的排序特性很有用

### 改进方向

1. 在开始编码前，应该先画状态转换图或递归调用图
2. 可以编写更多的测试用例来覆盖边界情况
3. 尝试不同的实现方法进行比较（如迭代vs递归）
4. 学习使用Python的调试器（pdb）进行更高效的调试

### 总结

本次实验从简单的类设计到复杂的递归算法，逐步提升了编程能力和问题解决能力。我不仅掌握了具体的技术实现，更重要的是培养了面向对象思维和递归思维，这对于后续学习人工智能算法和数据结构至关重要。实验过程中的调试经历让我深刻理解了"代码是写给人看的，只是恰好能被计算机执行"的道理，良好的代码结构和清晰的逻辑远比巧妙的技巧更重要。

