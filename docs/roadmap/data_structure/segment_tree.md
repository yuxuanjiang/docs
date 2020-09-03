# 线段树(区间树) Segment Tree

?> 实质：基于区间的统计查询

* 2017年注册用户中消费最高的用户？消费最少的用户？学习时间最长的用户？
* 某个太空区间中天体的总量是多少？



`对于给定的区间`

* 更新：更新区间中一个元素或者一个区间的值

* 查询一个区间[i, j]的最大值，最小值，或者区间数字的和

    

## 实现的复杂度

|      | 使用数组实现 | 使用线段树 |
| ---- | ------------ | ---------- |
| 更新 | O(n)         | O(logn)    |
| 查询 | O(n)         | O(logn)    |



![image-20200822143856959](https://tva1.sinaimg.cn/large/007S8ZIlgy1gi05sopypwj31cw0kpdp8.jpg)



## 如果区间有 n 个元素，数组表示需要多少个节点?

0 层: 1

1 层: 2

2 层: 4

3 层: 8

...

h-1 层: 2*(h-1)

h 层: 2*h



* 如果区间有 n 个元素，数组表示需要 `4n` 的空间, 即 4n 的静态空间。



## 基本实现

```java
public class SegmentTree<E> {

    private E[] tree;
    private E[] data;

    public SegmentTree(E[] arr){
        data = (E[]) new Object[arr.length];
        for(int i=0; i<arr.length ;i++){
            data[i] = arr[i];
        }

        tree = (E[]) new Object[4 * arr.length];
    }

    public E get(int index){
        if(index < 0 || index >= data.length)
            throw new IllegalArgumentException("Index is Illegal");
        return data[index];
    }

    public int getSize(){
        return data.length;
    }

    private int leftChild(int index){
        return 2 * index + 1;
    }

    private int rightChild(int index){
        return 2 * index + 2;
    }
}
```



## 更新 Segment Tree

