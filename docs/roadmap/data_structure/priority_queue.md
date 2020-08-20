# 优先队列 (Priority Queue)

?> 普通队列：先进先出；后进后出 <br/> 优先队列：出对顺序和入队顺序无关；和优先级相关



## 优先队列的接口

```java
Interface Queue<E>
    void enqueue(E)
    E dequeue()
    E getFront()
    int getSize()
    boolean isEmpty()
```



## 优先队列的实现时间复杂度

|              | enqueue | dequeue |
| ------------ | ------- | ------- |
| 普通线性结构 | O(1)    | O(n)    |
| 顺序线性结构 | O(n)    | O(1)    |
| 堆           | O(logn) | O(long) |



