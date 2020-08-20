# 堆 (Heap)



## 堆的实现 - Binary Heap

* Binary Heap 是一个完全二叉树
    * 完全二叉树:  把元素一层一层顺序排列成树的形状
    * 堆中某个节点的值总是不大于其父节点的值
* root 节点的 index 为 1 时

![image-20200819130216727](https://tva1.sinaimg.cn/large/007S8ZIlly1ghwm51sytzj31gd0mk13y.jpg)

* root 节点的 index 为 0 时

![image-20200819130544695](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghwm8sfhiqj31gg0mu7fv.jpg)



## 基础的 Heap 结构

!> 这里的 Heap 实现使用了自己编写的 Array 结构

```java
public class MaxHeap<E extends Comparable<E>>{
    private Array[E] data;
    
    public MaxHeap(int capacity){
        data = new Array<E>(capacity);
    }
    
    public MaxHeap(){
        data = new Array<E>();
    }
    
    // 返回堆中的元素个数
    public int size(){
        retun data.getSize();
    }
    
    // 返回一个boolean值，表示堆中是否为空
    public boolean isEmpty(){
        return data.isEmpty();
    }
    
    // 查看堆中最大元素
    public E findMax(){
        if(isEmpty()){
            throw new IllegalArgumentException("Can't find max when heap is empty");
        }
        return data.get(0);
    }
}
```



## Heap 的三个节点辅助函数

```java
// 返回完全二叉树的数组表示中，一个索引所表示的元素的父亲节点的索引
public int parent(int index){
    if(index == 0){
        throw new IllegalArgumentException("Index-0 doesn't have parent");
    }
    return (index - 1) / 2;
}

// 返回完全二叉树的数组表示中，一个索引所表示的元素的左孩子节点的索引
public int leftChild(int index){
    return index * 2 + 1
}

// 返回完全二叉树的数组表示中，一个索引所表示的元素的右孩子节点的索引
public int rightChild(int index){
    return index * 2 + 2;
}
```



## 向堆中添加元素 & Sift Up 操作

```java
// 向堆中添加元素和 sift up
public void add(E e){
    data.addLast(e);
    siftUp(data.getSize() - 1);
}

private void siftUp(int k){
    // k 必须大于 0 && data 中 k 位置的元素的父节点小于 k 位置的元素
    while(k > 0 && data.get(parent(k)).compareTo(data.get(k)) < 0){
        data.swap(k, parent(k));
        k = parent(k);
    }
}
```



## 从堆中移除元素 & Sift Down 操作

```java
// 取出堆中的最大元素 extract max 和 sift down
public E extractMax(){
    E ret = findMax();
    
    // 1. 交换最大值 与 数组末尾的元素，让最大值移动到末尾
    data.swap(0, data.getSize() - 1);
    
    // 2. 移除最大值
    data.removeLast();
    
    // 3. sift down
    siftDown(0);
    
    return ret;
}

private void siftDown(int k){
    while(leftChild(k) < data.getSize()){
        
        int j = leftChild(k);
        if(j + 1 < data.getSize() && data.get(j+1).compareTo(data.get(j)) > 0){
            j = rightChild(k);
            // data[j] 是 leftChild 和 rightChild 中的最大值
        }
        
        if(data.get(k).compareTo(data.get(j)) >= 0){
            break;
        }
           
        data.swap(k, j);
        k = j;
    }
}
```



##  Replace 功能 ：取出最大的元素后，放入一个新的元素

* replace 操作: 取出最大的元素后，放入一个新的元素
* 实现1: 可以先 extractMax, 再 add, 两次 O(logn) 的操作
* 实现2: 可以直接将堆顶元素替换以后 sift down, 一次 O(logn) 的操作

```java
// 取出堆中最大元素，并且替换成元素e
public E replace(E e){
    E ret = findMax();
    data.set(0, e);
    siftDown(0);
    return ret;
}
```





