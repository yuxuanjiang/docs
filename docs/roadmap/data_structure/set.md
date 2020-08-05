# Set 集合

* 集合就是承载元素的容器
* 每个元素只能承载一次，可以用于去重的操作



## 集合的结构

### Set\<T>

* void add(T)
* void remove(T)
* boolean contains(T)
* int getSize()
* boolean isEmpty()



## 集合的应用

* 去重操作
* 访客的统计
* 词汇量统计



## 使用BST编写的 Set

```java
public class BSTSet<T extends Comparable<T>> implements Set {

    private BST<T> bst;

    public BSTSet(){
        bst = new BST<T>();
    }

    @Override
    public void add(Object e) {
        bst.add((T) e);
    }

    @Override
    public void remove(Object e) {
        bst.remove((T) e);
    }

    @Override
    public boolean contains(Object e) {
        return bst.contains((T) e);
    }

    @Override
    public int getSize() {
        return bst.getSize();
    }

    @Override
    public boolean isEmpty() {
        return bst.isEmpty();
    }
}
```



## 使用 LinkedList 编写的 Set

```java
public class LinkedListSet<T> implements Set {
    private LinkedList<T> list;

    public LinkedListSet(){
        list = new LinkedList<>();
    }

    @Override
    public void add(Object e) {
        if(!list.contains((T) e)){
            list.addFirst((T) e);
        }
    }

    @Override
    public void remove(Object e) {
        if(list.contains((T) e)){
            list.removeElement((T) e);
        }
    }

    @Override
    public boolean contains(Object e) {
        return list.contains((T) e);
    }

    @Override
    public int getSize() {
        return list.getSize();
    }

    @Override
    public boolean isEmpty() {
        return list.isEmpty();
    }
}
```



## 时间复杂度分析

|               | Linked List Set | Binary Search Tree Set                        |
| ------------- | --------------- | --------------------------------------------- |
| 添加 add      | O(n)            | O(depth) = O(logn) 平均 \| 最差 = O(n) = 链表 |
| 查询 contains | O(n)            | O(depth) = O(logn)                            |
| 删除 remove   | o(n)            | O(depth) = O(logn)                            |



<p class="warn">n 和 depth 的关系为 : depth h-1 = 2^(depth -1); 因此 h = log2(n+1) = O(log2n) = O(logn)</p>



## 集合的练习

```java
// leetcode #804

public int uniqueMorseRepresentations(String[] words){
    String[] codes = {".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."};

    TreeSet<String> set = new TreeSet<>();

    for(String word: words){
        StringBuilder res = new StringBuilder();
        for(int i=0; i<word.length(); i++){
            res.append(codes[word.charAt(i) - 'a']);
        }
        set.add(res.toString());
    }

    return set.size();
}
```



## 有序集合 & 无序集合

* `BSTSet` : 有序集合中的元素具有顺序性
* `LinkedListSet` & `HashTable` :  无序集合中的元素没有顺序性



## 多重集合 (Multi Set)

* 集合中的元素可以重复