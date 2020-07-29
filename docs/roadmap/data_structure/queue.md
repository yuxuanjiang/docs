# Queue 队列

* Queue 是一种线性结构
* 相比数组，队列对应的操作时数组的子集
* 只能从一端 (队尾) 添加元素，只能从另外一端 (队首) 取出元素
* Queue 是一种先进先出的数据结构 
* First In First Out (FIFO)

![image-20200727171426763](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh686av942j30ap0i8q3f.jpg)



## 队列的实现

### Interface Queue\<T>

* void enqueue(T) - 添加元素
* T dequeue() - 取出元素
* T getFront() - 队首的元素
* int getSize() - 队列中的元素
* boolean isEmpty() - Queue 是否为空



```java
public class ArrayQueue<T> implements Queue {
    private Array<T> array;

    public ArrayQueue(int capacity){
        array = new Array<>(capacity);
    }

    public ArrayQueue(){
        array = new Array<>();
    }

    @Override
    public int getSize() {
        return array.getSize();
    }

    @Override
    public boolean isEmpty() {
        return array.isEmpty();
    }

    @Override
    public void enqueue(Object e) {
        array.addLast((T) e);
    }

    @Override
    public Object dequeue() {
        return array.removeFirst();
    }

    @Override
    public Object getFront() {
        return array.getFirst();
    }

    @Override
    public String toString(){
        StringBuilder res = new StringBuilder();
        res.append("Queue: ");
        res.append("Front [");
        for(int i=0; i<array.getSize() ;i++){
            res.append(array.get(i));
            if(i != array.getSize() - 1){
                res.append(", ");
            }
        }
        res.append("] Tail");
        return res.toString();
    }
}
```



## 复杂度分析

### Queue\<T>

* void enqueue(T) - O(1) 均摊
* T dequeue() - O(n)
* T getFront() - O(1)
* int getSize() - O(1)
* boolean isEmpty() - O(1)



## Queue 的优化  -   循环队列

![image-20200727173030995](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh68n08um4j311k0doad2.jpg)

![image-20200727173418227](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh68qy3ubdj311f0eawhy.jpg)

* if `front == tail` 则 Queue 为空
* if `(tail + 1) % capacity == front` 则  Queue 为满



```java
public class LoopQueue<T> implements Queue<T> {
    private T[] data;
    private int front, tail;
    private int size;

    public LoopQueue(int capacity){
        data = (T[]) new Object[capacity + 1];
        front = 0;
        tail = 0;
        size = 0;
    }

    public LoopQueue(){
        this(10);
    }

    @Override
    public int getSize() {
        return size;
    }

    public int getCapacity(){
        return data.length - 1;
    }

    @Override
    public boolean isEmpty() {
        return front == tail;
    }

    @Override
    public void enqueue(T e) {
        if((tail + 1) % data.length == front){
            resize(2 * getCapacity());
        }

        data[tail] = e;
        tail = (tail + 1) % data.length;
        size ++;
    }

    private void resize(int newCapacity){
        T[] newData = (T[]) new Object[newCapacity + 1];
        for(int i=0; i<size; i++){
            newData[i] = data[(i + front) % data.length];
        }
        data = newData;
        front = 0;
        tail = size;
    }

    @Override
    public T dequeue() {
        if(isEmpty()){
            throw new IllegalArgumentException("Cannot dequeue from an empty queue");
        }

        T ret = data[front];
        data[front] = null;
        front = (front + 1) % data.length;
        size--;

        if(size == getCapacity() / 4 && (getCapacity() / 2) != 0){
            resize(getCapacity() / 2);
        }

        return ret;
    }

    @Override
    public T getFront() {
        if(isEmpty()){
            throw new IllegalArgumentException("Cannot dequeue from an empty queue");
        }
        return data[front];
    }

    @Override
    public String toString(){
        StringBuilder res = new StringBuilder();
        res.append(String.format("Queue: size = %d, capacity = %d\n", size, getCapacity()));
        res.append("Front [");
        for(int i=front; i != tail; i = (i+1) % data.length){
            res.append(data[i]);
            if((i+1) % data.length != tail){
                res.append(", ");
            }
        }
        res.append("] Tail");
        return res.toString();
    }
}
```



## 循环队列的复杂度分析

### LoopQueue\<T>

* void enqueue(T) - O(1) 均摊 
* T dequeue() - O(1) 均摊
* T getFront() - O(1)
* int getSize() - O(1)
* boolean isEmpty() - O(1)



## 数组队列 & 循环队列的时间测试

```java
public class Testing{
	private static double testQueue(Queue<Integer> q, int opCount){
        
        long startTime = System.nanoTime();
        
        Random random = new Random();
        for(int i=0; i<opCount, i++){
            q.enqueue(random.nextInt(Integer.MAX_VALUE));
        }
        for(int i=0; i<opCount; i++){
            q.dequeue();
        }
        
        long endTime = System.nanoTime();
        return (endTime - startTime) / 1000000000.0;
    }
    
    public static void main(String[] args){
        int opCount = 10000;
        
        ArrayQueue<Integer> arrayQueue = new ArrayQueue<>();
        double time1 = testQueue(arrayQueue, opCount);
        System.out.println("ArrayQueue, time: " + time1 + " s");
        
        LoopQueue<Integer> loopQueue = new LoopQueue<Integer>();
        double time2 = testQueue(loopQueue, opCount);
        System.out.println("LoopQueue, time: " + time2 + " s");
    }
}
```

