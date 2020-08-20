# Singleton Pattern

?> 定义: 保证一个类仅有一个实例，并提供一个全局访问点

* 类型: 创建型
* `适用场景`:
    * 想确保任何情况下都绝对只有一个实例
* `优点`
    * 在内存中有且只有一个实例，减少了内存的开销
    * 可以避免对资源的多种占用
    * 设置全局访问点，严格控制访问
* `缺点`
    * 没有借口，扩展比较困难
* `重点`
    * 私有构造器：禁止外部创建一个新的实例
    * 线程安全
    * 延迟加载
    * 序列化和反序列化的安全
    * 反射
* `使用技能`
    * 反编译
    * 内存原理
    * 多线程 Debug
* `相关设计模式`
    * 单例模式 + 工厂模式
    * 单例模式 + 享元模式



### 懒汉式多线程单例模式

```java
public class LazySingleton {
    private static LazySingleton lazySingleton = null;
    private LazySingleton(){}

    public synchronized static LazySingleton getInstance(){
        if(lazySingleton == null){
            lazySingleton = new LazySingleton();
        }
        return lazySingleton;
    }

//    public static LazySingleton getInstance(){
//        synchronized(LazySingleton.class){
//            if(lazySingleton == null){
//                lazySingleton = new LazySingleton();
//            }
//        }
//        return lazySingleton;
//    }
}

// Multithreading Code
public class T implements Runnable{
    public void run() {
        LazySingleton lazySingleton = LazySingleton.getInstance();
        System.out.println(Thread.currentThread().getName() +" : "+ lazySingleton);
    }
}

// Testing Code
public class Test {
    public static void main(String[] args) {
        Thread t1 = new Thread(new T());
        Thread t2 = new Thread(new T());
        t1.start();
        t2.start();
        System.out.println("Program End");
    }
}
```



### 双层检查懒汉式单例模式 (Double-check Lazy Singleton)

```java
public class LazyDoubleCheckSingleton {

    private volatile static LazyDoubleCheckSingleton lazyDoubleCheckSingleton = null;
    private LazyDoubleCheckSingleton(){};

    public static LazyDoubleCheckSingleton getInstance(){
        if(lazyDoubleCheckSingleton == null){
            synchronized (LazyDoubleCheckSingleton.class){
                if(lazyDoubleCheckSingleton == null){
                    lazyDoubleCheckSingleton = new LazyDoubleCheckSingleton();
                    // 1. 分配内存给这个对象
                    // 2. 初始化对象
                    // 3. 设置 lazyDoubleCheckSingleton 指向刚分配的内存地址

                    // 注意: 上面的2和3的执行顺序可能会发生排序颠倒问题, 使用 volatile 关键字用于保证重排序
                }
            }
        }
        return lazyDoubleCheckSingleton;
    }
}

```

![image-20200818150646419](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghvk48zah0j30rq0lwwj9.jpg)

![image-20200818150737662](https://tva1.sinaimg.cn/large/007S8ZIlly1ghvk53eizrj31az0lcn47.jpg)



### 静态内部类懒汉单例模式 (Static Inner-class Lazy Singleton)

```java
public class StaticInnerClassLazySingleton {

    private StaticInnerClassLazySingleton(){}

    private static class InnerClass{
        private static StaticInnerClassLazySingleton  staticInnerClassLazySingleton = new StaticInnerClassLazySingleton();
    }

    public static StaticInnerClassLazySingleton getInstance(){
        return InnerClass.staticInnerClassLazySingleton;
    }
}
```

![image-20200818151632397](https://tva1.sinaimg.cn/large/007S8ZIlly1ghvked9f1fj31f60lxai4.jpg)



### 饿汉式单例模式 (Hungry Singletion )

```java
public class HungrySingleton {
    private final static HungrySingleton hungrySingleton;

    static {
        hungrySingleton = new HungrySingleton();
    }

    private HungrySingleton(){

    }

    public static HungrySingleton getInstance(){
        return hungrySingleton;
    }
}
```



### 序列化破坏单例模式原理&解决方案

```java
public class Test {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        HungrySingleton instance = HungrySingleton.getInstance();

        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("singleton_file"));
        oos.writeObject(instance);

        File file = new File("singleton_file");
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(file));
        HungrySingleton newInstance = (HungrySingleton) ois.readObject();

        System.out.println(instance);
        System.out.println(newInstance);
        System.out.println(instance == newInstance);

    }
}

// 通过上面的测试代码可以得出结论: 序列化之后的 newInstance 与 instance 并不是一个实例对象
com.design.creational.singleton.HungrySingleton@1d44bcfa
com.design.creational.singleton.HungrySingleton@4e50df2e
false
```



#### 解决方案

```java
import java.io.Serializable;

public class HungrySingleton implements Serializable {
    private final static HungrySingleton hungrySingleton;

    static {
        hungrySingleton = new HungrySingleton();
    }

    private HungrySingleton(){

    }

    public static HungrySingleton getInstance(){
        return hungrySingleton;
    }

    // 解决单例的序列化破坏
    private Object readResolve(){
        return hungrySingleton;
    }
}
```



### 反射攻击原理 & 解决方案

```java
public class Test {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class objectClass = HungrySingleton.class;
        Constructor constructor = objectClass.getDeclaredConstructor();
        constructor.setAccessible(true);

        HungrySingleton instance = HungrySingleton.getInstance();
        HungrySingleton object = (HungrySingleton) constructor.newInstance();

        System.out.println(instance);
        System.out.println(object);
        System.out.println(instance == object);
    }
}
```



#### 解决方案

```java
import java.io.Serializable;

public class HungrySingleton implements Serializable {
    private final static HungrySingleton hungrySingleton;

    static {
        hungrySingleton = new HungrySingleton();
    }

    private HungrySingleton(){
        // 解决单例的反射攻击
        if(hungrySingleton != null){
            throw new RuntimeException("Rejecting to invoke a singleton instance using reflection.");
        }
    }

    public static HungrySingleton getInstance(){
        return hungrySingleton;
    }

    // 解决单例的序列化破坏
    private Object readResolve(){
        return hungrySingleton;
    }
}
```



### [推荐] 枚举单例模式 (Enum Singleton) 

```java
public enum EnumInstance {
    INSTANCE;

    private Object data;

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }

    public static EnumInstance getInstance(){
        return INSTANCE;
    }
}
```



#### 枚举单例的序列化与反序列化测试

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        EnumInstance instance = EnumInstance.getInstance();
        instance.setData(new Object());

        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("enum_singleton"));
        oos.writeObject(instance);

        File file = new File("enum_singleton");
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(file));

        EnumInstance newInstance = (EnumInstance) ois.readObject();

        System.out.println(instance.getData());
        System.out.println(newInstance.getData());
        System.out.println(instance.getData() == newInstance.getData());
    }
}
```



#### 枚举单例的反射测试

```java
public class Test {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class objectClass = EnumInstance.class;
        Constructor constructor = objectClass.getDeclaredConstructor(String.class, Integer.class);

        constructor.setAccessible(true);
        EnumInstance instance = (EnumInstance) constructor.newInstance("Hello", 2);
    }
}
```





### 容器单例模式 (Container Singleton)

* 优点：便于统一管理单例
* 缺点：线程不安全

```java
public class ContainerSingleton {
    private ContainerSingleton(){};

    private static Map<String, Object> singletonMap = new HashMap<String, Object>();

    public static void putInstance(String key, Object instance){
        if(!key.equalsIgnoreCase("") && (instance != null)){
            if (!singletonMap.containsKey(key)) {
                singletonMap.put(key,instance);
            }
        }
    }

    public static Object getInstance(String key){
        return singletonMap.get(key);
    }
}
```



### ThreadLocal 单例

* 不能保证整个应用全局唯一，但是可以保证线程唯一

```java
public class ThreadLocalInstance {
    private static final ThreadLocal<ThreadLocalInstance> threadLocalInstance = new ThreadLocal<ThreadLocalInstance>(){
        @Override
        protected ThreadLocalInstance initialValue() {
            return new ThreadLocalInstance();
        }
    };

    private ThreadLocalInstance(){

    }

    public static ThreadLocalInstance getInstance(){
        return threadLocalInstance.get();
    }
}
```

 