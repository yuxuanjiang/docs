# 建造者模式 (Builder Pattern)

?> 定义: 将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

* 用户只需要指定需要建造的类型就可以得到它们，建造过程以及细节不需要知道
* `类型`: 创建型
* `使用场景`
    * 如果一个对象有非常复杂的内部结构(很多属性)
    * 想把复杂对象的创建和使用分离
* `优点`
    * 封装性好，创建和使用分离
    * 扩展性好、创造类之间独立、一定程度上解耦
* `缺点`
    * 产生多余的 Builder 对象
    * 产品内部发生变化，建造者都要修改，成本较大



## 代码演示

```java
public class Course {
    private String courseName;
    private String courseNote;
    private String courseVideo;
    private String courseArticle;
    private String courseQA;

    public String getCourseName() {
        return courseName;
    }

    public void setCourseName(String courseName) {
        this.courseName = courseName;
    }

    public String getCourseNote() {
        return courseNote;
    }

    public void setCourseNote(String courseNote) {
        this.courseNote = courseNote;
    }

    public String getCourseVideo() {
        return courseVideo;
    }

    public void setCourseVideo(String courseVideo) {
        this.courseVideo = courseVideo;
    }

    public String getCourseArticle() {
        return courseArticle;
    }

    public void setCourseArticle(String courseArticle) {
        this.courseArticle = courseArticle;
    }

    public String getCourseQA() {
        return courseQA;
    }

    public void setCourseQA(String courseQA) {
        this.courseQA = courseQA;
    }

    @Override
    public String toString() {
        return "Course{" +
                "courseName='" + courseName + '\'' +
                ", courseNote='" + courseNote + '\'' +
                ", courseVideo='" + courseVideo + '\'' +
                ", courseArticle='" + courseArticle + '\'' +
                ", courseQA='" + courseQA + '\'' +
                '}';
    }
}

// Course Builder
public abstract class CourseBuilder {
    public abstract void buildCourseName(String courseName);
    public abstract void buildCourseNote(String courseNote);
    public abstract void buildCourseVideo(String courseVideo);
    public abstract void buildCourseArticle(String courseArticle);
    public abstract void buildCourseQA(String courseQA);

    public abstract Course makeCourse();
}

// Course Actual Builder
public class CourseActualBuilder extends CourseBuilder{
    private Course course = new Course();

    public void buildCourseName(String courseName) {
        course.setCourseName(courseName);
    }

    public void buildCourseNote(String courseNote) {
        course.setCourseNote(courseNote);
    }

    public void buildCourseVideo(String courseVideo) {
        course.setCourseVideo(courseVideo);
    }

    public void buildCourseArticle(String courseArticle) {
        course.setCourseArticle(courseArticle);
    }

    public void buildCourseQA(String courseQA) {
        course.setCourseQA(courseQA);
    }

    public Course makeCourse() {
        return course;
    }
}

// Course Coach
public class Coach {
    private CourseBuilder courseBuilder;

    public void setCourseBuilder(CourseBuilder courseBuilder) {
        this.courseBuilder = courseBuilder;
    }

    public Course makeCourse(String courseName, String courseNote, String courseVideo, String courseArticle, String courseQA){
        this.courseBuilder.buildCourseName(courseName);
        this.courseBuilder.buildCourseNote(courseNote);
        this.courseBuilder.buildCourseVideo(courseVideo);
        this.courseBuilder.buildCourseArticle(courseArticle);
        this.courseBuilder.buildCourseQA(courseQA);

        return this.courseBuilder.makeCourse();
    }
}

// Testing Code
public class Test {
    public static void main(String[] args) {
        CourseBuilder courseBuilder = new CourseActualBuilder();
        Coach coach = new Coach();
        coach.setCourseBuilder(courseBuilder);

        Course course = coach.makeCourse("Java 设计模式", "PPT", "Video", "Article", "Question & Answers");
        System.out.printf(course.toString());
    }
}

```



### Version Two: using inner class

```java
package com.design.creational.builder.v2;

public class Course {
    private String courseName;
    private String courseNote;
    private String courseVideo;
    private String courseArticle;
    private String courseQA;

    public Course(CourseBuilder courseBuilder){
        this.courseName = courseBuilder.courseName;
        this.courseNote = courseBuilder.courseNote;
        this.courseVideo = courseBuilder.courseVideo;
        this.courseArticle = courseBuilder.courseArticle;
        this.courseQA = courseBuilder.courseQA;
    }

    @Override
    public String toString() {
        return "Course{" +
                "courseName='" + courseName + '\'' +
                ", courseNote='" + courseNote + '\'' +
                ", courseVideo='" + courseVideo + '\'' +
                ", courseArticle='" + courseArticle + '\'' +
                ", courseQA='" + courseQA + '\'' +
                '}';
    }

    public static class CourseBuilder{
        private String courseName;
        private String courseNote;
        private String courseVideo;
        private String courseArticle;
        private String courseQA;

        public CourseBuilder buildCourseName(String courseName){
            this.courseName = courseName;
            return this;
        }

        public CourseBuilder buildCourseNote(String courseNote) {
            this.courseNote = courseNote;
            return this;
        }

        public CourseBuilder buildCourseVideo(String courseVideo) {
            this.courseVideo = courseVideo;
            return this;
        }

        public CourseBuilder buildCourseArticle(String courseArticle) {
            this.courseArticle = courseArticle;
            return this;
        }

        public CourseBuilder buildCourseQA(String courseQA) {
            this.courseQA = courseQA;
            return this;
        }

        public Course build(){
            return new Course(this);
        }
    }
}

// Testing Code
public class Test {
    public static void main(String[] args) {
        Course course = new Course.CourseBuilder()
                .buildCourseName("Java 设计模式")
                .buildCourseNote("Note")
                .buildCourseVideo("Video")
                .buildCourseArticle("Article")
                .buildCourseQA("QA")
                .build();
        System.out.println(course);
    }
}
```

