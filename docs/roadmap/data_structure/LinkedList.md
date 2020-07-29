# Linked List 链表

* 数据存储在 ”节点 (Node) “ 中

* `优点` : 真正的动态，不需要处理固定容量的问题

* ` 缺点` : 丧失了随机访问能力

* ```java
    class Node{
    	T e;
    	Node next;
    }
    ```

![image-20200727222153032](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh6h27qigaj316f0c8q4j.jpg)

## 数组和链表的对比

*  数组最好用于索引有语义的情况, scores[2]
* 最大的优点: 支持快速查询



* 链表不适合用于索引有语义的情况
* 最大的优点: 动态



## 链表的实现

```java
public class LinkedList<T> {

    private class Node{
        public T e;
        public Node next;

        public Node(T e, Node next){
            this.e = e;
            this.next = next;
        }

        public Node(T e){
            this(e, null);
        }

        public Node(){
            this(null, null);
        }

        @Override
        public String toString() {
            return e.toString();
        }
    }
}
```



## 添加元素的功能

```java
private Node head;
int size;

public LinkedList(){
    head = null;
    size = 0;
}

// 获取链表中元素的个数
public int getSize(){
    return size;
}

// 返回链表是否为空
public boolean isEmpty(){
    return size == 0;
}

// 在链表头添加元素
public void addFirst(T e){
//        Node node = new Node(e);
//        node.next = head;
//        head = node;

    head = new Node(e, head);
    size ++;
}

// 在链表的任意位置添加元素
public void add(int index, T e){
    if(index < 0 || index > size){
   		throw new IllegalArgumentException("Add failed, Illegal index");
	}

    if(index == 0){
        addFirst(e);
    }else{
        Node prev = head;
        for(int i=0; i<index-1; i++){
        	prev = prev.next;
        }
        //            Node node = new Node(e);
        //            node.next = prev.next;
        //            prev.next = node;

        prev.next = new Node(e, prev.next);

        size++;
    }
}

// 在链表的末尾添加新的元素
public void addLast(T e){
	add(size, e);
}
```



## 为链表设立虚拟头结点

![image-20200728140559900](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh78ck806xj31as09hgnd.jpg)



```java
private Node dummyHead;
int size;

public LinkedList(){
    dummyHead = new Node(null, null);
    size = 0;
}

// 获取链表中元素的个数
public int getSize(){
    return size;
}

// 返回链表是否为空
public boolean isEmpty(){
    return size == 0;
}

// 在链表的任意位置添加元素
public void add(int index, T e){
    if(index < 0 || index > size){
        throw new IllegalArgumentException("Add failed, Illegal index");
    }

    Node prev = dummyHead;
    for(int i=0; i<index; i++) {
        prev = prev.next;
    }

    prev.next = new Node(e, prev.next);

    size++;
}

// 在链表头添加元素
public void addFirst(T e){
    add(0, e);
}

// 在链表的末尾添加新的元素
public void addLast(T e){
    add(size, e);
}
```



## 链表的遍历, 查询和修改功能

```java
// 获得链表的第 index 个位置的元素(练习)
public T get(int index){
    if(index < 0 || index > size){
        throw new IllegalArgumentException("Get failed, Illegal index");
    }

    Node cur = dummyHead.next;
    for(int i=0; i<index ;i++){
        cur = cur.next;
    }

    return cur.e;
}

// 获取链表的第一个元素
public T getFirst(){
    return get(0);
}

// 获取链表的最后一个元素
public T getLast(){
    return get(size - 1);
}

// 修改链表的第 Index 个元素
public void set(int index, T e){
    if(index < 0 || index > size){
        throw new IllegalArgumentException("Set failed, Illegal index");
    }

    Node cur = dummyHead.next;
    for(int i=0; i< index; i++){
        cur = cur.next;
    }

    cur.e = e;
}

// 查找链表中是否存在元素 e
public boolean contains(T e){
    if(isEmpty()){
        return false;
    }

    Node cur = dummyHead.next;
    while(cur != null){
        if(cur.e.equals(e)){
            return true;
        }
        cur = cur.next;
    }

    return false;
}

// 打印链表的数据结构
@Override
public String toString() {
    StringBuilder res = new StringBuilder();

    Node cur = dummyHead.next;
    while(cur != null){
        res.append(cur + " -> ");
        cur = cur.next;
    }
    res.append("NULL");

    /*
        // for 循环版本
        for(Node cur = dummyHead.next; cur != null; cur = cur.next){
            res.append(cur + " -> ");
            cur = cur.next;
        }
        res.append("NULL");
        */

    return res.toString();
}
```



## 链表删除元素的功能

```java
// 删除链表的第 index 个元素
public T remove(int index){

    if(index < 0 || index > size){
        throw new IllegalArgumentException("Delete failed, Illegal Index");
    }

    Node prev = dummyHead;
    for(int i=0; i<index ;i++){
        prev = prev.next;
    }

    Node retNode = prev.next;
    prev.next = retNode.next;

    retNode.next = null;
    size --;

    return retNode.e;
}

public T removeFirst(){
    return remove(0);
}

public T removeLast(){
    return remove(size - 1);
}
```



## 时间复杂度分析

* 添加操作 O(n)
    * addLast(e) - O(n)
    * addFirst(e) - O(1)
    * add(index, e) - O(n/2) = O(n)
* 删除操作 O(n)
    * removeLast() - O(n)
    * removeFirst() - o(1)
    * remove(index) - O(n/2) = O(n)
* 修改操作 O(n)
    * set(index, e) - O(n)
    * contains(e) - O(n)



<p class="warn">链表的增删改查全部都是 O(n) 的时间复杂度。但是如果只对链表头进行操作的话 (增加、删除、查找)，那么它的时间复杂度将是 O(1)。</p>



## 使用链表实现一个栈

```java
public class LinkedListStack<T> implements Stack {

    private LinkedList<T> list;

    public LinkedListStack(){
        list = new LinkedList<>();
    }

    @Override
    public int getSize() {
        return list.getSize();
    }

    @Override
    public boolean isEmpty() {
        return list.isEmpty();
    }

    @Override
    public void push(Object e) {
        list.addFirst((T) e);
    }

    @Override
    public Object pop() {
        return list.removeFirst();
    }

    @Override
    public Object peek() {
        return list.getFirst();
    }

    @Override
    public String toString() {
        StringBuilder res = new StringBuilder();
        res.append("Stack: top ");
        res.append(list);
        return res.toString();
    }
}
```



## 使用链表实现一个队列(优化的 O(1) Queue)

![image-20200728151607841](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh7adivk5uj31620hwq5y.jpg)



```java
public class LinkedListQueue<T> implements Queue<T> {

    private class Node{
        public T e;
        public Node next;

        public Node(T e, Node next){
            this.e = e;
            this.next = next;
        }

        public Node(T e){
            this(e, null);
        }

        public Node(){
            this(null, null);
        }
    }

    private Node head, tail;
    private int size;

    public LinkedListQueue(){
        head = null;
        tail = null;
        size = 0;
    }

    @Override
    public int getSize() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    @Override
    public void enqueue(T e) {
        if(tail == null){
            tail = new Node(e);
            head = tail;
        }else{
            tail.next = new Node(e);
            tail = tail.next;
        }
        size ++;
    }

    @Override
    public T dequeue() {
        if(isEmpty()){
            throw new IllegalArgumentException("Cannot dequeue from an empty queue");
        }

        Node retNode = head;
        head = head.next;

        retNode.next = null;
        if(head == null){
            tail = null;
        }
        size --;

        return retNode.e;
    }

    @Override
    public T getFront() {
        if(isEmpty()){
            throw new IllegalArgumentException("Queue is empty");
        }

        return head.e;
    }

    @Override
    public String toString() {
        StringBuilder res = new StringBuilder();
        res.append("Queue: front ");

        Node cur = head;
        while(cur != null){
            res.append(cur.e + " -> ");
            cur = cur.next;
        }

        res.append("NUll tail");

        return res.toString();
    }
}
```



## 链表与递归 (LeetCode Practice)

```java
/**
 * LeetCode #203 Remove Linked List Elements
 * Definition for singly-linked list.
 * public class ListNode{
 *     int val;
 *     ListNode next;
 *     ListNode(int x){
 *         val = x;
 *     }
 * }
 */
public class Solution {
    public ListNode removeElements(ListNode head, int val){

        // 1. 列表head就是匹配的数值
        while(head != null && head.val == val){
            ListNode delNode = head;
            head = head.next;
            delNode.next = null;
        }

        // 2. 如果链表已经为空
        if(head == null){
            return null;
        }

        // 3. 删除聊表中间匹配的元素
        ListNode prev = head;
        while(prev.next != null){
            if(prev.next.val == val){
                ListNode delNode = prev.next;
                prev.next = delNode.next;
                delNode.next = null;
            }else{
                prev = prev.next;
            }
        }

        return head;
    }
}


/**
* DummyHead version solution for remove linked list elements
*/
public class DummyHeadSolution{
    public ListNode removeElements(ListNode head, int val){
        
        ListNode dummyHead = new ListNode(-1);
        dummyHead.next = head;
        
        ListNode prev = dummyHead;
        while(prev.next != null){
            if(prev.next.val == val){
                prev.next = prev.next.next;
            }else{
                prev = prev.next;
            }
        }
        
        return dummyHead.next;
    }
}
```



## 数组转链表的功能

```java
// 使用 arr 作为参数, 创建一个链表，当前的 listNode 为链表的头节点
public ListNode(int[] arr){
    if(arr == null || arr.length == 0){
        throw new IllegalArgumentException("Array cann't be empty");
    }
    
    this.val = arr[0];
    ListNode cur = this;
    
    for(int i=1; i<arr.length; i++){
    	cur.next = new ListNode(arr[i]);
        cur = cur.next;
    }
}

@Override
public String toString(){
    StringBuilder res = new StringBuilder();
    
    ListNode cur = this;
    while(cur != null){
        res.append(cur.val + " -> ");
        cur = cur.next;
    }
    res.append("NULL");
    
    return res.toString();
}
```

