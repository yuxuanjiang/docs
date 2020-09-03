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



```java
public class PriorityQueue<E extends Comparable<E>> implements Queue<E> {

    private MaxHeap<E> maxHeap;

    public PriorityQueue(){
        maxHeap = new MaxHeap<E>();
    }

    @Override
    public int getSize() {
        return maxHeap.size();
    }

    @Override
    public boolean isEmpty() {
        return maxHeap.isEmpty();
    }

    @Override
    public void enqueue(E e) {
        maxHeap.add(e);
    }

    @Override
    public E dequeue() {
        return maxHeap.extractMax();
    }

    @Override
    public E getFront() {
        return maxHeap.findMax();
    }
}
```



## 在 N 个元素中选出前 M 个元素

* 使用优先队列，维护当前看到的前M个元素
* 需要最小堆



## [Leetcode#347]前 K 个高频元素

* 使用自定义的 priority queue

```java
package MaxHeap;

import java.util.LinkedList;
import java.util.List;
import java.util.TreeMap;

public class Solution {

    private class Frequency implements Comparable<Frequency>{
        public int e, freq;

        public Frequency(int e, int freq){
            this.e = e;
            this.freq = freq;
        }

        @Override
        public int compareTo(Frequency o) {
            if(this.freq < o.freq){
                return 1;
            }else if(this.freq > o.freq){
                return -1;
            }else{
                return 0;
            }
        }
    }

    public List<Integer> topKFrequent(int[] nums, int k){
        TreeMap<Integer, Integer> map = new TreeMap<>();

        for(int num: nums){
            if(map.containsKey(num)){
                map.put(num, map.get(num) + 1);
            }else{
                map.put(num, 1);
            }
        }

        PriorityQueue<Frequency> pq = new PriorityQueue<Frequency>();
        for(int key: map.keySet()){
            if(pq.getSize() < k){
                pq.enqueue(new Frequency(key, map.get(key)));
            }else if(map.get(key) > pq.getFront().freq){
                pq.dequeue();
                pq.enqueue(new Frequency(key, map.get(key)));
            }
        }

        LinkedList<Integer> res = new LinkedList<>();
        while(!pq.isEmpty()) {
            res.add(pq.dequeue().e);
        }
        return res;
    }
}
```



*  使用 java 标准库的 priority queue (java 标准库的 priority queue 是一个最小堆)

```java
import java.util.*;
import java.util.PriorityQueue;

public class Solution {
    public List<Integer> topKFrequent(int[] nums, int k){
        TreeMap<Integer, Integer> map = new TreeMap<>();

        for(int num: nums){
            if(map.containsKey(num)){
                map.put(num, map.get(num) + 1);
            }else{
                map.put(num, 1);
            }
        }

        // 使用自定义的比较器来比较元素的优先级
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>(new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {
                return map.get(a) - map.get(b);
            }
        });
        
        // lambda 表达式
        //PriorityQueue<Integer> pq = new PriorityQueue<Integer>(
        //        (a, b) -> map.get(a) - map.get(b)
        //);

        for(int key: map.keySet()){
            if(pq.size() < k){
                pq.add(key);
            }else if(map.get(key) > map.get(pq.peek())){
                pq.remove();
                pq.add(key);
            }
        }

        LinkedList<Integer> res = new LinkedList<>();
        while(!pq.isEmpty()) {
            res.add(pq.remove());
        }
        return res;
    }
}
```

