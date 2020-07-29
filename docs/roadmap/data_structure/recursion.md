# 递归

<p class="warn">本质上，递归将原来的问题，转化为更小的同一个问题。但是递归调用的代价就是 ：系统调用 + 系统栈空间</p>



例如: 数组求和

Sum(arr[0......n-1]) = arr[0] + Sum(arr[1.....n-1])   <--- 更小的同一个问题

Sum(arr[1......n-1]) = arr[1] + Sum(arr[2....n-1])

...

Sum(arr[n-1, n-1]) = arr[n-1] + Sum([])   <---  最基本的问题



```java
public class RecursiveSum {
    public static int sum(int[] arr){
        return sum(arr, 0);
    }

    // 用于计算 arr[l...n) 这个区间内所有的数字的和
    private static int sum(int[] arr, int l){
        if(l == arr.length){
            return 0;
        }
        return arr[l] + sum(arr, l+1);
    }

    public static void main(String[] args){
        int[] arr = {1,2,3,4,5,5,6,8,7,7,7};
        System.out.println(RecursiveSum.sum(arr));;
    }
}
```



## 链表天然的递归性

![image-20200729110636688](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh88sipwq8j31370jtn0a.jpg)



```java
public class solution{
    public class ListNode{
        int val;
        ListNode next;

        public ListNode(int val){
            this.val = val;
        }

        public ListNode(int[] arr){
            if(arr == null || arr.length == 0){
                throw new IllegalArgumentException("Array can't be empty");
            }

            this.val = arr[0];
            ListNode cur = this;

            for(int i=1; i<arr.length; i++) {
                cur.next  = new ListNode(arr[i]);
                cur = cur.next;
            }
        }

        @Override
        public String toString() {
            StringBuilder res = new StringBuilder();

            ListNode cur = this;
            while(cur != null){
                res.append(cur.val + " -> ");
                cur = cur.next;
            }
            res.append("NULL");
            return res.toString();
        }
    }

    public ListNode removeElements(ListNode head, int val){
        if(head == null){
            return null;
        }

        head.next = removeElements(head.next, val);
        if(head.val == val){
            return head.next;
        }else{
            return head;
        }
        
        // 使用三元运算符  return head.val == val? head.next : head;
    }
}
```

