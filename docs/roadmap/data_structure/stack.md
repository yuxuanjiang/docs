# Stack 栈



* Stack 是一种线性结构
* 相对于数组，Stack 对应的操作时数组的子集
* 只能从一端添加元素，也只能从一端取出元素

* 着一段称为栈顶
* Stack  是一种后进先出的数据结构
* Last In First Out (LIFO)



![image-20200727145312802](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh643fdyfsj309o0kcq3j.jpg)



## 栈的应用

### Undo 操作 (撤销)

![image-20200727145610509](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh646ezhujj31990jk0vk.jpg)



### 程序调用的系统栈

![image-20200727150039112](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh64b52sqsj31dz0ktdk9.jpg)



## 栈的实现

### Stack\<T>

* void push(T) - 入栈
* T pop() - 出栈
* T peek() - 查看栈顶元素
* int getSize() - 获得栈里的元素个数
* Boolean isEmpty() - 查询是否为空



```java
Interface Stack<E> <-- implement -- ArrayStack<E>
* void push(E)
* T pop()
* T peek()
* int getSize()
* boolean isEmpty()
```



## 栈的复杂度分析

### ArrayStack\<T>

* void push(T) - O(1) 均摊
* T pop() - O(1) 均摊
* T peek() - O(1)
* int getSize() - O(1)
* Boolean isEmpty() - O(1)



## 栈的应用 - 括号匹配 (编译器)

* 栈顶元素反映了在嵌套的层次关系中，`最近的`需要匹配的元素

![image-20200727164543206](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh67cfesv0j315k0gd75n.jpg)



```java
import java.util.Stack;

class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack();
        
        for(int i=0; i<s.length(); i++){
            char c = s.charAt(i);
            
            if(c == '(' || c == '{' || c == '['){
                stack.push(c);
            }else{
                if(stack.isEmpty()){
                    return false;
                }
                
                char topChar = stack.pop();
                if(c == ')' && topChar != '('){
                    return false;
                }else if(c == '}' && topChar != '{'){
                    return false;
                }else if(c == ']' && topChar != '['){
                    return false;
                }
            }
        }
        
        return stack.isEmpty();
    }
}
```

