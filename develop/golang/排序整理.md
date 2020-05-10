## go 实现排序

排序的原子操作：比较 和 交换

### 冒泡排序
冒泡排序只会操作相邻的两个数据，每次冒泡都会对相邻的两个元素进行比较
```
// a是数组，n表示数组的大小
func BubbleSort(a []int, n int) {
    if n <= 1 {
        return
    }
    for i := 0; i < n; i++ {
        //提前退出标志
        flag := false
        for j := 0; j < n-i-1; j++ {
            if a[j] > a[j+1] {
                a[j], a[j+1] = a[j+1], a[j]
                //此次冒泡有数据交换
                flag = true
            }
        }
        //如果没有交换数据，提前退出
        if !flag {
            break
        }
    }
}
```

### 插入排序
我们将数组中的数据分为两个区间，已排序区间和未排序区间，初始排序区间只有一个元素，就是数组的第一个元素。

插入算法的核心就是：取未排序区间中的元素，在已排序区间中找到合适的插入位置将其插入

```
func InsertSort(a []int, n int) {
    if n <= 1 {
        return
    }
    for i := 1; i < n; i++ {
        value := a[i]
        j := i - 1
        //查找要插入的位置并移动数据
        for ; j >= 0; j-- {
            if a[j] > value {
                a[j+1] = a[j]
            } else {
                break
            }
        }
        a[j+1] = value
    }
}
```

### 选择排序
选择排序也分已排序区间和未排序区间，但是选择排序每次会从未排序区间中找到最小的元素，将其放到已排序区间的末尾

```
func SelectSort(a []int, n int) {
    if n <= 1 {
        return
    }
    for i := 0; i < n; i++ {
        //查找最小值
        minIndex := i
        for  j := i + 1; j < n; j++ {
            if a[j] < a[minIndex] {
                minIndex = j
            }
        }
        //交换
        a[i], a[minIndex] = a[minIndex], a[i]
    }
}
```

冒泡排序，插入排序，选择排序 的时间复杂度都为 `O(n^2)`，而且都是基于数组的


### 归并排序
先把数组从中间分成前后两部分，然后对前后两部分分别排序，在将排序好的两部分合并在一起，这样整个数组就都有序了

归并排序使用的就是分治思想

```
func MergeSort(arr []int) {
    arrLen := len(arr)
    if arrLen <= 1 {
        return
    }
    mergeSort(arr, 0, arrLen-1)
}

func mergeSort(arr []int, start, end int) {
    if start >= end {
        return
    }
    mid := (start + end)/2
    mergeSort(arr, start, mid)
    mergeSort(arr, mid+1, end)
    merge(arr, start, mid, end)
}

func merge(arr []int, start, mid, end int) {
    tmpArr := make([]int, end-start+1)
    i := start
    j := mid + 1
    k := 0
    for ; i <= mid && j <= end; k++ {
        if arr[i] < arr[j] {
            tmpArr[k] = arr[i]
            i++
        } else {
            tmpArr[k] = arr[j]
            j++
        }
    }
    for ; i <= mid; i++ {
        tmpArr[k] = arr[i]
        k++
    }
    for ; j <= end; j++ {
        tmpArr[k] = arr[j]
        k++
    }
    copy(arr[start:end+1], tmpArr)
}
```

### 快速排序
快速排序利用的也是分治思想，假设要排序数组中下标从 p 到 r 之间的一组数据，我们选择 p 到 r 之间的任意一个数据作为分区点，我们遍历 p 到 r 之间的数据，将小于分区点的放到左边，将大于分区点的放到右边，将分区点放到中间

```
func QuickSort(arr []int) {
    separateSort(arr, 0, len(arr)-1)
}

func separateSort(arr []int, start, end int) {
    if start >= end {
        return
    }
    i := partition(arr, start, end)
    separateSort(arr, start, i-1)
    separateSort(arr, i+1, end)
}

func partition(arr []int, start, end int) int {
    pivot := arr[end]
    var i = start
    for j := start; j < end; j++ {
        if arr[j] < pivot {
            if !(i == j) {
                arr[i], arr[j] = arr[j], arr[i]
            }
            i++
        }
    }
    arr[i], arr[end] = arr[end], arr[i]
    return i
}
```