# Tree



* 将数据使用树结构存储后，出奇的高效
    * 二分搜索树 (Binary Search Tree)
    * 平衡二叉树: AVL; 红黑树
    * 堆; 并查集
    * 线段树; Tire (字典树, 前缀树)



![image-20200729165856442](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh8iyrolixj30qf0icq6g.jpg)



## 什么是二叉树

* 和链表一样，动态数据结构

```java
class Node{
    T e;
    Node left;
    Node right;
}
```

* 二叉树具有唯一根节点 (root)
* 每一个节点都有对应着 `左 (left )` 和 `右 (right)` 的节点
* 二叉树每个节点最多有`两个子节点 (2 children)`
* 一个子节点都没有的节点称之为 `leaf` 节点
* 每个节点最多只能有一个父节点, 除了根节点之外
* 二叉树具有天然的递归结构
    * 每个节点`左/右子树`也是二叉树



## 什么是二分搜索树

* 二分搜索树是二叉树
* 二分搜索树的每个节点的值
    * 大于其左子树的所有节点的值
    * 小于其右子树的所有节点的值
* 存储的元素必须有可比较性

![image-20200729171852552](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh8jjikk50j30wt0lb43p.jpg)



## BST 的基本结构

```java
public class BST<T extends Comparable<T>> {
    private class Node{
        public T e;
        public Node left;
        public Node right;

        public Node(T e){
            this.e = e;
            left = null;
            right = null;
        }
    }

    private Node root;
    private int size;

    public BST(){
        root =  null;
        size = 0;
    }

    // 查看当前BST中的元素个数
    public int size(){
        return size();
    }

    // 查看当前BST是否为空
    public boolean isEmpty(){
        return size == 0;
    }
}
```



## 向BST中添加元素的功能

```java
// 向BST中添加新的元素
public void add(T e){
    // 如果根节点本身就是空的
    if(root == null) {
        root = new Node(e);
        size++;
    }else{
        add(root, e);
    }
}

// 向以node 为根节点的二分搜索树中插入元素e，递归算法
private void add(Node node, T e){
    // 基础条件
    if(e.equals(node.e)){
        return;
    }else if(e.compareTo(node.e) < 0 && node.left == null){
        node.left = new Node(e);
        size++;
        return;
    }else if(e.compareTo(node.e) > 0 && node.right == null){
        node.right = new Node(e);
        size++;
        return;
    }

    if(e.compareTo(node.e) < 0){
        add(node.left, e);
    }else{
        add(node.right, e);
    }
}
```



## BST 添加元素功能的改进

```java
// 向BST中添加新的元素
public void add(T e){
    // 如果根节点本身就是空的
    root = add(root, e);
}

// 向以node 为根节点的二分搜索树中插入元素e，递归算法
// 返回插入新节点后二分搜索树的根
private Node add(Node node, T e){
    // 基础条件
    if(node == null){
        size++;
        return new Node(e);
    }

    if(e.compareTo(node.e) < 0){
        node.left = add(node.left, e);
    }else if(e.compareTo(node.e) > 0){
        node.right = add(node.right, e);
    }

    return node;
}
```



## 在BST中查询的功能

```java
// 查看BST中是否包含元素e
public boolean contains(T e){
    return contains(root, e);
}

// 查看以node 为根节点的二分搜索树中是否包含元素e，递归算法
private boolean contains(Node node, T e){
    if(node == null){
        return false;
    }

    if(e.compareTo(node.e) == 0){
        return true;
    }else if(e.compareTo(node.e) < 0){
        return contains(node.left, e);
    }else{
        return contains(node.right, e);
    }
}
```



## 什么是遍历 (Traverse) 操作?

<p class="warn">遍历操作就是把所有的节点都访问一遍。</p>

* 对于遍历操作，两棵子树都要顾及
* 遍历分为
    * 深度优先遍历
        * Pre Order
        * In Order
        * Post Order
    * 广度优先遍历
        * Level Order

```java
public void traverse(Node node){
    if(node == null){
        return null;
    }
    
    // 访问该节点
    traverse(node.left);
    traverse(node.right);
}
```



## BST的前序遍历（Pre Order）

* 先访问节点，再访问左, 右子树
* 最自然的遍历方式
* 最常用的遍历方式

```java
// BST的前序遍历
public void preOrder(){
    preOrder(root);
}

// 前序遍历以 node 为根的BST, 递归算法
private void preOrder(Node node){
    if(node == null){
        return;
    }

    System.out.println(node.e);
    preOrder(node.left);
    preOrder(node.right);
}
```



<p class="tip">二分搜索树遍历的非递归实现，比递归实现复杂很多，并且实际应用并不广泛。</p>

```java
// 二分搜索树的非递归前序遍历
public void preOrderNR(){
    Stack<Node> stack = new Stack<>();
    stack.push(root);

    while(!stack.isEmpty()){
        Node cur = stack.pop();
        System.out.println(cur.e);

        if(cur.right != null){
            stack.push(cur.right);
        }else if(cur.left != null){
            stack.push(cur.left);
        }
    }
}
```



## BST的中序遍历 (In Order)

* 先访问节点的左子树，访问该节点本身，在访问节点的右子树

* BST的中序遍历结果是顺序的

```java
// BST的中序遍历
public void inOrder(){
    inOrder(root);
}

// 中序遍历以 node 为根的BST，递归算法
private void inOrder(Node node){
    if(node == null){
        return;
    }

    inOrder(node.left);
    System.out.println(node.e);
    inOrder(node.right);
}
```



## BST的后续遍历（Post Order）

*  先遍历左子树，再遍历右子树，最后访问root节点
* 后续遍历的应用
    * 为BST释放内存 （c/c++语言的手动释放内存）

```java
// BST后续遍历
public void postOrder(){
    postOrder(root);
}

// 后续遍历以 node 为根的BST，递归算法
private void postOrder(Node node){
    if(node == null){
        return;
    }

    postOrder(node.left);
    postOrder(node.right);
    System.out.println(node.e);
}
```



## BST 的层序遍历（广度优先遍历）

* 需要使用到 Queue 来逐层遍历BST
* 更快的找到问题的解
* 常用语算法设计中 - 最短路径

```java
// BST的层序遍历
public void levelOrder(){
    Queue<Node> q = new LinkedList<>();
    q.add(root);

    while(!q.isEmpty()){
        Node cur = q.remove();
        System.out.println(cur.e);

        if(cur.left != null){
            q.add(cur.left);
        }
        
        if(cur.right != null){
            q.add(cur.right);
        }
    }
}
```



## BST删除节点的功能

* 删除BST的最小值和最大值

```java
// 寻找BST的最小元素
public T minimum(){
    if(size == 0){
        throw new IllegalArgumentException("BST is empty");
    }
    return minimum(root).e;
}

private Node minimum(Node node){
    if(node.left == null){
        return node;
    }
    return minimum(node.left);
}

// 寻找BST的最大元素
public T maximum(){
    if(size == 0){
        throw  new IllegalArgumentException("BST is empty");
    }
    return maximum(root).e;
}

private Node maximum(Node node){
    if(node.right == null){
        return node;
    }
    return maximum(node.right);
}

// 从BST中删除最小值所在的节点，并返回最小值
public T removeMin(){
    T ret = minimum();
    root = removeMin(root);
    return ret;
}

// 删除以 node 为根的BST中最小节点
// 返回删除节点后新的BST的根
private Node removeMin(Node node){
    if(node.left == null){
        Node rightNode = node.right;
        node.right = null;
        size --;
        return rightNode;
    }

    node.left = removeMin(node.left);
    return node;
}

// 从BST中删除最大值所在的节点，并返回最大值
public T removeMax(){
    T ret = maximum();
    root = removeMax(root);
    return ret;
}

// 删除以 node 为根的BST中最大值
// 返回删除节点后新的BST的根
private Node removeMax(Node node){
    if(node.right == null){
        Node leftNode = node.left;
        node.left = null;
        size --;
        return leftNode;
    }

    node.right = removeMax(node.right);
    return node;
}
```



* BST 中删除任意节点
* 如果Node存在同时拥有左右子树
    * 将该Node的右子树的最小节点替换该Node

```java
// 从BST中删除元素为e的节点
private void remove(T e){
    root = remove(root, e);
}

private Node remove(Node node, T e){
    if(node == null){
        return null;
    }

    if(e.compareTo(node.e) < 0){
        node.left = remove(node.left, e);
        return node;
    }else if(e.compareTo(node.e) > 0){
        node.right = remove(node.right, e);
        return node;
    }else{ // e == node.e
        if(node.left == null){
            Node rightNode = node.right;
            node.right = null;
            size --;
            return rightNode;
        }

        if(node.right == null){
            Node leftNode = node.left;
            node.left = null;
            size --;
            return leftNode;
        }

        // 带删除的节点左右子树均不为空的情况
        // 找到比待删除节点大的最小节点，即待删除几点右子树的最小节点
        // 用这个节点顶替待删除节点的位置
        Node successor = minimum(node.right);
        successor.right = removeMin(node.right);
        successor.left = node.left;

        node.left = node.right = null;
        return successor;
    }
}
```

