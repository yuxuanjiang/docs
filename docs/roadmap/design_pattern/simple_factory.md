# 简单工厂 (Simple Factory)

?> 定义: 由一个工厂对象决定创建出哪一种产品类的实例

* 类型: 创建型, 但是不属于 GOF23种设计模式
* 适用场景
    * 工厂类负责创建的对象比较少
    * 客户端(应用层)只知道传入工厂类的参数，对于如何创建对象(逻辑)不关系
* `优点`： 只需要传入一个正确的参数，就可以获取你所需要的对象而无需知道其创建细节
* `缺点`： 工厂类的职责相对过重，增加新的产品需要修改工厂类的判断逻辑，违背开闭原则



## 简单工厂的代码实现 (coding)

!> 1. 普通的简单工厂代码实现，缺点是不符合开闭原则, 因为要在工厂类中不断的添加新的元素

```java
public abstract class Video{
    public abstract void produce();
}

public class JavaVideo extends Video{
	public void produce(){
        System.out.println("producing Java video");
    }
}

public class PythonVideo extends Video{
    public void produce(){
        System.out.println("producing Python video");
    }
}

// 工厂类
public class VideoFactory{
    public Video getVideo(String type){
        if(type.equalsIgnoreCase("java")){
            return new JavaVideo;
        }else if(type.equalsIgnoreCase("python")){
            return new PythonVideo;
        }
        return null;
    }
}

public class Test{
    public static void main(String[] args){
        VideoFactory videoFactory = new VideoFactory();
        Video video = videoFactory.getVideo("java");
        if(video != null){
            video.produce();
        }
    }
}
```



!> 2. 在工厂类中使用反射机制来规避前面代码中违反开闭原则的行为

```java
public abstract class Video{
    public abstract void produce();
}

public class JavaVideo extends Video{
	public void produce(){
        System.out.println("producing Java video");
    }
}

public class PythonVideo extends Video{
    public void produce(){
        System.out.println("producing Python video");
    }
}

// 工厂类
public class VideoFactory{
    public Video getVideo(Class c){
        Video video = null;
        
        try {
            video = (Video) Class.forName(c.getName()).newInstance();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        
        return video;
    }
}

public class Test{
    public static void main(String[] args){
        VideoFactory videoFactory = new VideoFactory();
        Video video = videoFactory.getVideo(JavaVideo.class);
        if(video != null){
            video.produce();
        }
    }
}
```

