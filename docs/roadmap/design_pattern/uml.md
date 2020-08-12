# UML & 软件设计七大原则

## UML 的定义

* 统一建模语言 (Unified Modeling Language, UML)
* 非专利的第三代建模和规约语言
* UML 是一种开放的
* 用于说明、可视化、构建和编写一个正在开发的面向对象的、软件密集系统的制品的开放方法



## UML 2.2 分类

?> UML 2.2 中一共定义了14种图示



* `结构式图形`: 强调的是系统的建模
    * 静态图(类图，对象图，包图)
    * 实现图(组件图，部署图)
    * 剖面图
    * 复合结构图
* `行为式图形`: 强调系统模型中触发的事件
    * 活动图
    * 状态图
    * 用例图
* `交互式图形`: 属于行为式图形的子集合，强调系统模型中资料流程
    * 通信图
    * 交互概述图 (UML 2.0)
    * 时序图 (UML 2.0)
    * 时间图 (UML 2.0)



## UML 类图

* `Class Diagram`: 用于表示类、接口、实例等之间相互的静态关系
* 虽然名字叫类图，但是类图中并不只有类
* UML 箭头方向: 从子类指向父类
    * 定义子类时需要通过 **extends** 关键字指定父类
    * 子类一定是知道父类定义的，但是父类并不知道子类的定义
    * 只有知道对象信息时才能指向对方
    * 所以箭头方向是从子类指向父类的
* 空心三角箭头: 继承或者实现
    * `实线`: 继承 extends ( is a 关系， 扩展目的，不虚，很结实)
    * `虚线`: 实现 implements 

* `实线`: 关联关系
    * 表示一个类对象和另一个类对象有关联
    * 通常是一个类中有另一个类对象作为属性
* `虚线`: 依赖关系
    * 依赖关系，临时用一下
    * 表示一种使用关系，一个类需要借助另外一个类来实现功能
    * 一般是一个类使用另外一个类作为参数使用，或作为返回值
* `空心菱形`: 聚合
    * 聚合: 代表空容器中可以放很多相同东西，聚在一起(箭头方向所指向的类)
    * 整体和局部的关系，两者有着独立的生命周期，是 **has a** 的关系
* `实现菱形`: 组合
    * 组合: 代表容器中已经有实体结构的存在
    * 整体与局部的关系，和聚合的关系相比，关系更加强烈，两者有相同的生命周期，是 **contains a** 的关系
* `常见数字表达以及含义，假设有A类和B类，数字标记在A类侧`
    * **0...1:** 0 或者 1 个实例
    * **0...\* : **0 或者多个实例
    * **1...1:** 只有1个实例
    * **1:** 只能有一个实例
    * **1...\*:** 至少有一个实例



## UML 时序图

* `Sequence Diagram`: 是显示对象之间交互的图，这些对象是按时间顺序排列的
* 时序图中包含的建模元素主要有:
    * 对象 (Actor)
    * 生命线 (Lifeline)
    * 控制焦点 (Focus of Control)
    * 消息 (Message) 等



## 类图的表示

* `+` Public
* `-` Private
* `#` Protected
* `~` Package/Internal
* `*` Abstract e.g.: `someAbstractMethod()*`
* `$` Static e.g.: `someStaticMethod()$`



```
classDiagram
    class GeelyClass
    GeelyClass : +name: String
    GeelyClass : -age: int
    GeelyClass : #weight: double
    GeelyClass : ~height: double
    GeelyClass : +gender: char
    GeelyClass : +eat(food)
    GeelyClass : #drink()
    GeelyClass : -walk()
    GeelyClass : ~run()
    GeelyClass : +study()
    GeelyClass : +openMac(): boolean
    GeelyClass : +playGames()$
```



## 七大软件设计原则

### 1. 开闭原则 (Open Close)

?> 定义: 一个软件实体如类、模块和函数应该`对扩展开放，对修改关闭`

* 用抽象构建框架, 用实现扩展细节
* `优点`: 提高软件系统的可复用性及可维护性



```java
public interface ICourse{
    Integer getId();
    String getName();
    Double getPrice();
}

public class JavaCourse implements ICourse{
    private Integer id;
    private String name;
    private Double price;
    
    public JavaCourse(Integer id, String name, Double price){
        this.id = id;
        this.name = name;
        this.price = price;
    }
    
    public Integer getId(){
        return id;
    }
    
    public String getName(){
        return name;
    }
    
    public Double getPrice(){
        return price;
    }
}

// 扩展 Java 课程的打折需求
public class JavaDiscountCourse extends JavaCourse{
    public JavaDiscountCourse(Integer id, String name, Double price){
        super(id, name, price);
    }
	
    // 用于输出原价
    public Double getDiscountPrice(){
        return super().getPrice() * 0.8;;
    }
    
    
    // 用于输出优惠价格
    @Override
    public Double getPrice(){
    	return super().getPrice();    
    }
}

public class Test{
    public static void main(String[] args){
        ICourse course = new JavaDiscountCourse(96, "java", 348d);
        JavaDiscountCourse javaCourse = (JavaDiscountCourse) course;
        
       	StringBuilder res = new StringBuilder();
        res.append("课程ID: " + javaCourse.getId());
        res.append("课程名称: " + javaCourse.getName());
        res.append("课程价格: " + javaCourse.getPrice());
        res.apeend("优惠价格: " + javaCourse.getDiscountPrice())
        System.out.println(res.toString());
    }
}
```



### 2. 依赖倒置原则 (Dependence Inversion)

?> 定义: 高层模块不应该依赖低层模块，二者都应该依赖其抽象

* 抽象不应该依赖细节；细节应该依赖抽象
* 针对接口编程，不要针对实现编程
* `优点`: 可以减少类之间的耦合性，提高系统稳定性，提高代码可读性和可维护性，可降低修改程序造成的风险。



```java
public interface ICourse{
    void studyCourse();
}

public class JavaCourse implements ICourse{
	public void studyCourse(){
        System.out.println("Geely 在学习 Java 课程");
    }   
}

public class FECourse implements ICourse{
    public void studyCourse(){
        System.out.println("Geely 在学习前端课程");
    }
}

public class PythonCourse implements ICourse{
    public void studyCourse(){
        System.out.println("Geely 在学习Python课程");
    }
}

public class Geely{
    private ICourse icourse;
    
    public void setIcourse(ICourse icourse){
        this.icourse = icourse;
    }
    
    public void studyImoocCourse(){
        icourse.studyCourse();
    }
}


public class Test{
    public static void main(String[] args){
        Geely geely = new Geely();
        geely.setIcourse(new JavaCourse());
        geely.studyImoocCourse();
        
        geely.setICourse(new PythonCourse());
        geely.studyImoocCourse();
    }
}
```



### 3. 单一职责原则 (Single Responsibility)

?> 定义: 不要存在多于一个导致类变更的原因

* 一个类、接口、方法只负责一个职责
* `优点`: 降低类的复杂度、提高类的可读性，提高系统的可维护性，降低变更引起的风险



```java
// class layer
public class FlyBird{
    public static mainMoveMode(String birdName){
        System.out.println(birdName + "用翅膀飞");
    }
}

public class WalkBird{
    public static mainMoveMode(String birdName){
        System.out.println(birdName + "用脚跑");
    }
}


public class Test{
    public static void main(String[] args){
        FlyBird flybird = new FlyBird();
        flybird.mainMoveMode("大雁");
        
        WalkBird walkbird = new WalkBird();
        walkbird,mainMoveMode("鸵鸟");
    }
}


// interface layer
public interface ICourseManager{
    void studyCourse();
    void refundCourse();
}

public interface ICourseContent{
    String getCourseName();
    byte[] getCourseVideo();
}

public class CourseImpl implements ICourseManager, ICourseContent{
    @Override
    public void studyCourse(){
        ...
    }
    
    @Override
    public void refundCourse(){
        ...
    }
    
    @Override
    public String gteCourseName(){
        ...
    }
    
    @Override
    public byte[] getCourseVideo(){
        ...
    }
}

// method layer
public class Method{    
    private void updateUserUsername(String userName){
        userName = "Username";
    }
    
    private void updateUserAddress(String address){
        address = "New York";
    }
}
```



### 4. 接口隔离原则 (Interface Segregation)

?> 定义: 用多个专门的接口，而不使用单一的总接口，客户端不应该依赖它不需要的接口

* 一个类对一个类的依赖应该建立在最小的接口上
* 建立单一接口，不要建立庞大臃肿的接口
* 尽量细化接口，接口中的方法尽量少
* 注意适度原则，一定要适度
* `优点`: 符合我们常说的高内聚低耦合的设计思想，从而使得类聚有很好的可读性、可扩展性和可维护性。



```java
public interface IFlyAnimalAction{
    void fly();
}

public interface IEatAnimalAction{
    void eat();
}

public interface ISwimAnimalAction{
    void swim();
}

public class Dog implements IEatAnimalAction, ISwimAnimalAction{
    @Override
    public void eat(){
        ...
    }
    
    @Override
    public void swim(){
        ...
    }
}
```



### 5. 迪米特原则 (Demeter Principle)

?> 定义: 一个对象应该对其他对象保持最少的了解，又叫**最少知道原则****

* 尽量降低类与类之间的耦合
* `优点`：降低类之间的耦合
* 强调只和朋友交流，不和陌生人说话
* `朋友`: 出现在成员变量、方法的输入、输出参数中的类称为成员朋友类，而出现在方法体内部的类不属于朋友类。



```java
public class Boss{
    public void commandCheckNumber(Teamleader teamleader){
        teamLeader.checkNumberOfCourse();
    }
}

public class Course{
	...    
}

public class TeamLeader{
    List<Course> courseList = new ArrayList<>();
    for(int i=0; i<20 ;i++){
        courseList.add(new Course());
    }
    System.out.println("在线课程的数量是: " + courseList.size());
}

public class Test{
    public static void main(String[] args){
        Boss boss = new Boss();
        TeamLeader teamleader = new TeamLeader();
        boss.commandCheckNumber(teamleader);
    }
}
```



### 6. 里氏替换原则 (Liskov Substitution)

?> 定义: 如果对每一个类型为 T1 的对象 o1, 都有类型为 T2 的对象 o2, 使得以 T1 定义的所有程序 P 在所有的对象 o1 都替换成 o2 时，程序 P 的行为没有发生变化，那么类型 T2 是类型 T1 的子类型。<br/><br/>定义扩展: 一个软件实体如果使用一个父类的话，那一定适用于其子类，所有引用父类的地方必须能够透明地使用其子类的对象，子类对象能够替换父类对象，而程序逻辑不变。



* 引用意义: 子类可以扩展父类的功能，但不能改变父类原有的功能
* `含义1`：子类可以实现父类的抽象方法，但不能覆盖父类的非抽象方法
* `含义2`：子类中可以增加自己特有的方法
* `含义3`：当子类的方法重载父类的方法时，方法的前置条件(即方法的输入、输出)要比父类方法的输入参数更宽松。
* `含义4`：当子类的方法实现父类的方法时(重写/重载或实现抽象方法)，方法的后置条件(即方法的输出/返回值)要比父类更加严格或相等。
* 优点: 约束继承泛滥，开闭原则的一种表现。加强程序的健壮性，同时变更时可以做到非常好的兼容性提高程序的维护性、扩展性。降低需求变更时引入的风险。



```java
public interface Quardrangle{
    long getWidth();
    long getLength();
}

public class Reactangle implements Quardrangle{
    private long length;
    private long width;
    
    @Override
    public long getWidth(){
        return width;
    }
    
    @Override
    public long getLength(){
        return length;
    }
    
    public void setLength(long length){
        this.length = length;
    }
    
    public void setWidth(long width){
        this.width = width;
    }
}

public class Square() implements Quardrangle{
    private long sideLength;
    
    public void setSideLength(long sideLength){
        this.sideLength = sideLength;
    }
    
    public long getSideLength(){
        return sideLength;
    }
    
    @Overrdie
    public long getLength(){
        return sideLength;
    }
    
    @Overrdie
    public long getWidth(){
        return sideLength;
    }
}

public class Test{
    public static void resize(Rectangle rectangle){
        while(reactangle.getWidth <= rectangle.getLength()){
            rectangle.setWidth(rectangle.getWidth()++);
            System.out.println("Width: " + rectangle.getWidth() + "; length: " + rectangle.getLength());
        }
        System.out.println("resize 方法结束");
        System.out.println("Width: " + rectangle.getWidth() + "; length: " + rectangle.getLength());
    }
    
    
    public static void main(String[] args){
        Rectangle rectangle = new Rectangle();
        rectangle.setWidth(10);
        rectangle.setLength(20);
        resize(square);
    }
}
```



```java
public class Base{
    public void method(HashMap map){
        System.out.println("父类被执行");
    }
}

public class Child extends Base{
    @Override
    public void method(HashMap map){
        System.out.println("子类HashMap被执行");
    }
    
    public void method(Map map){
        System.out.println("子类Map被执行");
    }
}

public class Test{
    public static void main(String[] args){
        Child child = new Child();
        HashMap hashmap = new HashMap();
        
        child.method(hashmap);
    }
}
```



```java
// 子类返回值 <= 父类返回值
public abstract class Base{
    public abstract Map method();
}

public class Child extends Base{
    @Override
    public HashMap method(){
        HashMap hashMap = new HashMap();
        System.out.println("子类method 被执行");
        hashMap.put("message", "子类method被执行");
        return hashMap;
    }
}

public class Test{
    public static void main(String[] args){
        Child child = new Child();
        System.out.println(child.method());
    }
}
```



### 7. 合成(组合)/聚合复用原则 (Composition Aggregation)

?> 定义: 尽量使用对象组合/聚合, 而不是继承关系达到软件复用的目的

* 聚合 has-a 和组合 contains-a
* `优点`: 可以使系统更加灵活，降低类与类之间的耦合度，一个类的变化对其他类造成的影响相对较少
* 何时使用合成/聚合/继承
    * 组合 contains-a
    * 聚合 has-a
    * 继承 is-a



```java
public abstract class DBConnection{
    public abstract String getConnection();
}

public class MysqlConnection extends DBConnection{
    @Override
    public String getConnection(){
        return "Mysql 数据库连接";
    }
}

public class PostgreSQLConnection extends DBConnection{
    @Override
    public String getConnection(){
        return "PostgreSQL 数据库连接";
    }
}

public class ProductDao extends DBConnection{
    private DBConnection dbConnection;
    
    public void setDbConnection(DBConnection dbConnection){
        this.dbConnection = dbConnection;
    }
    
    public void addProduct(){
        String conn = dbConnection.getConnection();
        System.out.println("使用" + conn + ”增加产品“);
    }
}

public class Test{
    public static void main(String[] args){
        Production productDao = new Production();
        productDao.setDbConnection(new MysqlConnection());
        productDao.addProduct();
        
        productDao.setDbConnection(new PostgreSQLConnection());
        productDao.addProduct();
    }
}
```

