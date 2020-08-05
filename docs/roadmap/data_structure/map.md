# Map 映射

* 在Python中，Map的数据结构就是 `Dictionary` 字典。
* 存储(键, 值) 数据对的数据结构(Key, Value)
* 根据 `key`, 寻找 `value`
* 非常容易的使用链表或者二分搜索树来实现一个 Map

```java
// BST
class Node{
	K key;
    V value;
	Node left;
	Node right;
}

// Linked List
class Node{
	K key;
    V value;
	Node next;
}
```



## Map的接口实现

### Map\<key, value>

* void add(K, V)
* V remove(K)
* boolean contains(K)
* V get(K)
* void set(K, V)
* int getSize()
* boolean isEmpty()



## 基于 Linked List 实现 Map

```java
public class LinkedListMap<K, V> implements Map<K, V> {

    private class Node{
        public K key;
        public V value;
        public Node next;

        public Node(K key, V value, Node next){
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public Node(K key){
            this(key, null, null);
        }

        public Node(){
            this(null, null, null);
        }

        @Override
        public String toString() {
            return String.format("%s : %s", key, value);
        }
    }

    private Node dummyHead;
    private int size;

    public LinkedListMap(){
        dummyHead = new Node();
        size = 0;
    }

    private Node getNode(K key){
        Node cur = dummyHead.next;
        while(cur != null){
            if(cur.key.equals(key)){
                return cur;
            }
            cur = cur.next;
        }
        return null;
    }

    @Override
    public void add(K key, V value) {
        Node node = getNode(key);

        if(node == null){
            dummyHead.next = new Node(key, value, dummyHead.next);
            size++;
        }else{
            node.value = value;
        }
    }

    @Override
    public V remove(K key) {
        Node prev = dummyHead;
        while(prev.next != null){
            if(prev.next.key.equals(key)){
                break;
            }
            prev = prev.next;
        }

        if(prev.next != null){
            Node delNode = prev.next;
            prev.next = delNode.next;
            delNode.next = null;
            size --;
            return delNode.value;
        }

        return null;
    }

    @Override
    public boolean contains(K key) {
        return getNode(key) != null;
    }

    @Override
    public V get(K key) {
        Node node = getNode(key);
        return node == null ? null : node.value;
    }

    @Override
    public void set(K key, V newValue) {
        Node node = getNode(key);
        if(node == null){
            throw new IllegalArgumentException(key + "doesn't exist!");
        }

        node.value = newValue;
    }

    @Override
    public int getSize() {
        return size;
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }
}
```



## 基于 BST 实现 Map

```java

```

