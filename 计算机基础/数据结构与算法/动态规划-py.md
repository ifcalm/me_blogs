## 动态规划 Python 实现

**几个重要思想:**

- 递归
- 回溯法
- 动态规划


### 什么是递归

**递归的定义: 函数内部调用函数本身**

**递归技术在很多算法中存在:**

- 回溯法
- 分治法
- 动态规划

递归分为两个过程:**递** 和 **归**, 这两个都是自动完成的

**递归一定要有终止条件**

无限递归会发生栈溢出

#### 递归实现斐波拉契数列

指的是这样一个数列: `1 1 2 3 5 8 13 21 34 ...`, 后面的数都等于前面两个数的和

**递归代码实现:**
```
def fib(k):
    if k in [1, 2]:
        return 1
    return fib(k-1) + fib(k-2)
```

**通过非递归代码实现:**
```
def fib_fdg(k):
    assert k > 0, "k > 0"
    if k in [1, 2]:
        return 1
    k_1 = 1
    k_2 = 1
    for i in range(3, k+1):
        tmp = k_1
        k_1 = k_1 + k_2
        k_2 = tmp
    return k_1
```

#### 递归求解示意图:

![](img/dp_1.PNG)

从递归过程图来看，内部执行重复量是非常大的，很多计算是重复的

![](img/dp_2.PNG)

从递归的耗时我们可以看出，fib_dp(30) 所需的时间是 0.2 秒左右，但 fib_dp(35) 所需的时间是 2 秒左右，计算量增加是惊人的

#### 二分查找

1. 二分查找，也叫折半查找
2. 二分查找针对有序数组
3. 算法很多时候考察的是对边界的了解

非递归代码实现:

```
def search(data_list, target):
    left = 0
    right = len(data_list) - 1
    while left <= right:
        # 找到 [left, right] 中间的值
        mid = int((left+right)/2)
        # 判断中间值与目标值的大小
        if data_list[mid] == target:
            print("select ok: {}".format(mid))
            break
        elif data_list[mid] < target:
            # 如果中间值小于目标值，则在右侧继续二分查找
            left = mid + 1
        else:
            # 如果中间值大于目标值，则在左侧继续二分查找
            right = mid - 1
```

递归实现二分查找:

```
def dg_search(left, right, data_list, target):
    if left > right:
        return -1
    mid = int((left + right) / 2)
    if data_list[mid] == target:
        return mid
    elif data_list[mid] < target:
        # 在右侧继续递归查找
        return dg_search(mid+1, right, data_list, target)
    else:
        # 在左侧继续递归查找
        return dg_search(left, mid-1, data_list, target)
```

#### 汉诺塔实现

![](img/dp_3.PNG)

**递归是一个栈的调用过程，先进后出**

阶乘递归实现:
```
def jc(n):
    if n == 1:
        return 1
    else:
        return n*jc(n-1)
```

**汉诺塔递归实现**

```
def move(index, start, mid, end):
    if index == 1:
        print("{}-->{}".format(start, end))
        return
    else:
        move(index-1, start, end, mid)
        print("{}-->{}".format(start, end))
        move(index-1, mid, start, end)

if __name__ == "__main__":
    move(3, "A", "B", "C")
```

#### 递归总结

- 写法简单
- 递归都能通过非递归的方法实现
- 递归由于是函数调用自身，而函数调用是有时间和空间的消耗，每一次函数调用，都需要在内存中分配空间来保存参数，返回地址以及临时变量，而往栈中压入数据和弹出数据都需要时间，所以效率不高
- 递归很多计算都是重复的，由于其本质是把一个问题分解成两个或多个小问题，多个小问题存在互相重叠的部分，则存在重复计算
- 调用栈可能会溢出，每一次函数调用会在内存栈中分配空间，而每个进程的栈容量是有限的，当调用的层次太多时，就会超出栈的容量，导致栈溢出


