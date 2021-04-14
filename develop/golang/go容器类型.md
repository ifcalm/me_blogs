### 容器数据类型--container

- 堆, heap
- 链表, list
- 环, ring

go 的container 包实现了三个复杂的数据结构：堆，链表，环

### 堆

这里的堆使用的数据结构是小顶堆，即根节点比左边子树和右边子树的所有值都小。 go 的堆包只是实现了一个接口：

```
type Interface interface {
    sort.Interface
    Push(x interface{}) // add x as element Len()
    Pop() interface{}   // remove and return element Len() - 1.
}
```

这个堆结构继承自 sort.Interface, 回顾下 sort.Interface，它需要实现三个方法:
- Len() int
- Less(i, j int) bool
- Swap(i, j int)

加上堆接口定义的两个方法:
- Push(x interface{})
- Pop() interface{}

就是说你定义了一个堆，就要实现五个方法:

```
type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

那么 IntHeap 就实现了这个堆结构，我们就可以使用堆的方法来对它进行操作

### 链表

