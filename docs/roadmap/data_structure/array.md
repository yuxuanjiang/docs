# Array



<p class="warn">数据的本质就是把数据码成一排进行存放</p>

![image-20200725224301524](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh46fqvwdtj31cs0h6goe.jpg)



在 Java 中创建数据的方式十分简单

```java
public class Array {
    public static void main(String[] args) {

        int[] arr = new int[10];
        
        // 通过循环为数组添加元素
        for(int i=0; i<arr.length; i++){
            arr[i] = i;
        }

        int[] scores = new int[]{90,89,100};
        
        // 通过循环遍历数据的元素
        for(int i=0; i<scores.length; i++){
            System.out.println(scores[i]);
        }

        for(int score: scores){
            System.out.println(score);
        }
    }
}
```



<p class="tip">使用数组最大的优点: 快速查找, 比如 scores[2]。 但是，一个数组应该如何表示没有元素？如何添加元素？如何删除元素？ 这些是数组本身并不具备的功能，因此我们需要基于java的数组（静态数组），进行一次二次封装称为一个动态数组。</p>

```java
package Array;

public class Main {
    public static void main(String[] args) {
        Array<Integer> arr = new Array<Integer>(10);
        for(int i=0; i<10 ;i++){
            arr.addLast(i);
        }
        System.out.println(arr.getCapacity());
        arr.addLast(100);
        System.out.println(arr.getCapacity());
        arr.removeLast();
        System.out.println(arr.getCapacity());

        arr.add(1, 200);
        System.out.println(arr);

        arr.remove(1);
        System.out.println(arr);

        arr.removeFirst();
        System.out.println(arr);

        arr.removeElement(4);
        System.out.println(arr);
    }
}

class Array<T>{
    private T[] data;
    private int size; // 数组的元素个数

    // 构造函数, 传入数组的容量 capacity 构造函数
    public Array(int capacity){
        data = (T[]) new Object[capacity];
        size = 0;
    }

    public Array(){
        this(10);
    }

    // 获取数组中的元素个数
    public int getSize(){
        return size;
    }

    // 获取数组的容量
    public int getCapacity(){
        return data.length;
    }

    // 返回数组是否为空
    public boolean isEmpty(){
        return size == 0;
    }

    // 向数组的尾部添加新的元素
    public void addLast(T e){
        add(size, e);
    }

    // 向数组的头部添加新的元素
    public void addFirst(T e){
        add(0, e);
    }

    // 向数组中的指定位置插入新的元素
    public void add(int index, T e){


        if(index < 0 || index > size){
            throw new IllegalArgumentException("add failed, require index >= 0 || index <= size");
        }

        if(size == data.length){
            resize(2 * data.length);
        }

        for(int i=size-1; i>=index; i--){
            data[i+1] = data[i];
        }

        data[index] = e;
        size++;
    }

    // 插叙数组中的元素
    @Override
    public String toString(){
        StringBuilder res = new StringBuilder();
        res.append(String.format("Array: size = %d, capacity = %d\n", size, data.length));
        res.append("[");
        for(int i=0; i<size; i++){
            res.append(data[i]);
            if(i != size - 1){
                res.append(", ");
            }
        }
        res.append("]");
        return res.toString();
    }

    // 获取 index 索引位置的元素
    public T get(int index){
        if(index < 0 || index > size){
            throw new IllegalArgumentException("Get failed, Index is out of bound.");
        }
        return data[index];
    }

    // 修改 index 索引位置的元素
    public void set(int index, T e){
        if(index < 0 || index > size){
            throw new IllegalArgumentException("Set failed, Index is out of bound.");
        }
        data[index] = e;
    }

    // 查询数组中是否包含某个元素
    public boolean contains(T e){
        for(int i=0; i<size; i++){
            if(data[i].equals(e)){
                return true;
            }
        }
        return false;
    }

    // 查找数组中元素 e 所在的索引，如果不存在元素 e， 则返回 -1
    public int find(T e){
        for(int i=0; i<size; i++){
            if(data[i].equals(e)){
                return i;
            }
        }
        return -1;
    }

    // 从数组中删除指定 index 的元素， 返回删除的元素
    public T remove(int index){
        if(index < 0 || index >= size){
            throw new IllegalArgumentException("Remove failed, Index is out of bound.");
        }

        T ret = data[index];
        for(int i=index+1; i<size; i++){
            data[i - 1] = data[i];
        }
        size--;
        data[size] = null; // loitering objects != memory leak

        if(size == data.length / 2){
            resize(data.length/2);
        }
        return ret;
    }

    // 从数组中删除第一个位置的元素
    public T removeFirst(){
        return remove(0);
    }

    // 从数组中删除最后一个位置的元素
    public T removeLast(){
        return remove(size-1);
    }

    // 删除数组中的元素 e
    public void removeElement(T e){
        int index = find(e);
        if(index != -1){
            remove(index);
        }
    }

    // 动态数组扩容
    private void resize(int newCapacity){
        T[] newData = (T[]) new Object[newCapacity];
        for(int i=0; i<size ;i++){
            newData[i] = data[i];
        }
        data = newData;
    }
}
```

## 数组的基本结构

```java
class Array{
	private int[] data;
    private int size;
    
    public Array(int capacity){
        data = new int[capacity];
        size = 0;
    }
    
    // 无参数构造函数，默认给予数组的长度为10
    public Array(){
        this(10);
    }
}
```



## 使用泛型构建数组结构

```java
class Array<T>{
	private T[] data;
	private int size;
	
	public Array(int capacity){
		data = (T[])new Object[capacity];
		size = 0;
	}
	
	public Array(){
		this(10);
	}
}
```



## 获取数组的原信息功能

```java
public boolean isEmpty(){
	return size == 0;
}

public boolean isFull(){
    return size == data.length;
}

public int getSize(){
	return size;
}

public int getCapacity(){
	return data.length;
}

@Override
public String toString(){
    StringBuilder str = new StringBuilder();
    str.append(String.format("Array: size = %d, capacity = %d\n", size, data.length));
    str.append("[");
    for(int i=0; i<size ;i++){
        str.append(data[i]);
        if(i != size - 1){
            str.append(", ");
        }
    }
    str.append("]");
    return str.toString();
}
```



## 动态数组的实现

```java
private void resize(int newCapacity){
	T[] newData = (T[])new Object[newCapacity];
	for(int i=0; i<data.length ;i++){
		newData[i] = data[i];
	}
	data = newData;
}
```



## 增加元素的功能

```java
public void insert(int index, T element){
	if(index < 0 || index > size){
		throw new IllegalArgumentException("Insert new element failed, array index is out of bounds");
	}
    
    if(isFull()){ 
        resize(data.length * 2); 
    }
    
    for(int i=size-1; i>=index; i++){
        data[i+1] = data[i];
    }
    
    data[index] = element;
   	size++;
}

public void addFirst(T element){
    insert(0, element);
}

public void addLast(T element){
    insert(size, element);
}
```



## 删除元素的功能

```java
public T remove(int index){
	T res = data[index];
	
	for(int i=index+1; i<size ;i++){
		data[i-1] = data[i];
	}
    
    size--;
    data[index] = null; // loitering objects != memory leak
    
    if(data.length == data.length / 4 && data.length / 2 != 0){
        resize(data.length / 2);
    }
	return res;
}

public T removeFirst(){
    return remove(0);
}

public T removeLast(){
    return remove(size-1);
}

public void removeElement(E element){
    int index = find(element);
    if(index != -1){
        remove(element);
    }
}
```



## 查找元素的功能

```java
public boolean contains(T element){
	for(int i=0; i<size ;i++){
		if(data[i] == element){
			return true;
		}
	}
	return false;
}

public int find(T element){
	for(int i=0; i<size ;i++){
		if(data[i].equals(element)){
			return i;
		}
	}
	return -1;
}

public T get(int index){
    if(index < 0 || index > size){
        throw new IllegalArgumentException("Get element failed, array index is out of bounds.");
    }
    return data[index];
}
```



## 修改元素的功能

```java
public void set(int index, T element){
	if(index < 0 || index > size){
		throw new IllegalArgumentException("Set element failed, array index is out of bounds.");
	}
	
	data[index] = element;
}
```



## 时间复杂度分析

* O(1), O(n), O(lgn), O(nlogn), O(n^2)
* 大O描述的是算法的运行时间和输入数据之间的关系 (渐进时间复杂度，描述 n 趋近于无穷的情况)



```java
public static int sum(int[] nums){
    int sum = 0;
    for(int num: nums) sum += num;
    return sum;
}
```



<p class="warn">O(n) <br/> n 是 nums 中的元素个数 <br/> 算法和 n 呈线性关系 </p>



* 上面的代码实际时间为 : T = c1*n + c2 （忽略常数）



<p class="warn">因此，在实际中，T = 2  * n + 2 与 T = 2000 * n + 1000 的时间复杂度都是 O(n)<br/>  但是 T = 1*n*n + 0 的时间复杂度就是 O(n^2)</p>



* 添加操作: `O(n)`
    * addLast(e) : `O(1)`
    * addFirst(e) : `O(n)`
    * add(index, e) : `O(n/2) = O(n)`
    * resize(capacity) : `O(n)`
* 删除操作: `O(n)`
    * removeLast : `O(1)`
    * removeFirst : `O(n)`
    * remove(index, e) : `O(n/2) = O(n)`
    * resize(capacity) : `O(n)`
* 修改操作 : 已知索引 `O(1)` 位置索引 `O(n)`
    * set(index, e) : `O(1)`
* 查询操作 : 已知索引 `O(1)` 位置索引 `O(n)`
    * get(index) : `O(1)`
    * contains(e) : `O(n)`
    * find(e) : `O(n)`



## resize 的时间复杂度

* resize : `O(n)`

如果一个数组的 `capacity` 一开始为8，那么在第九次进行 addLast操作时，触发了 resize 函数, 总共进行了 9 + 8 = 17 次基本操作。平均每次 addLast 操作，进行了 2次基本操作。



假设capacity = n,  n+1 次 addLast, 触发 resize, 总共进行 2n+1 次基本操作平均，每次 addLast 操作，进行了2次基本操作。这样均摊计算的话，时间复杂度是 `O(1)`.



<p class="tip">在这个例子中，这样的均摊复杂度计算(amortized time complexity)，比计算最坏情况有意义。</p>



## 复杂度震荡 & resize 功能的优化

![image-20200726003756137](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh49r5vvlkj31di0igjw6.jpg)

* 出问题的原因: removeLast 时 resize 过于着急 (eager)
* 解决方案: 使用 lazy
    * 当 `size == capacity / 4` 时，才将 capacity 减半



```java
// 从数组中删除指定 index 的元素， 返回删除的元素
public T remove(int index){
    if(index < 0 || index >= size){
        throw new IllegalArgumentException("Remove failed, Index is out of bound.");
    }

    T ret = data[index];
    for(int i=index+1; i<size; i++){
        data[i - 1] = data[i];
    }
    size--;
    data[size] = null; // loitering objects != memory leak

    if(size == data.length / 4 && data.length / 2 != 0){
        resize(data.length/2);
    }
    return ret;
}
```

