# java10
方法重写覆盖  OVerride
方法重写/覆盖override
基本介绍：
简单地说：方法覆盖就是子类有一个方法，和父类的某个方法的名称，返回类型，参数一样，name我们就说子类的这个方法覆盖了父类的那个方法
==============

public class Animal {
    public void cry(){
        System.out.println("动物叫唤");
    }
}
=================


public class Dog extends Animal{
    //老韩解读
    //1.因为Dog是Animal子类
    //2.Dog的cry方法和Animal的cry的定义的形式一样（名称、返回类型、参数）
    //3.这时我们就说Dog的cry方法，重写Animal的cry方法
    public void cry(){
        System.out.println("小狗汪汪叫..");
    }
}


public class Override01 {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.cry();
    }
}
方法重写注意事项
1.子类的方法的参数，方法名称，要和父类方法的参数，方法名称完全一样。
2.子类方法的返回类型和父类方法返回类型一样，或者是父类返回类型的子类
比如父类犯规类型是Object，子类方法返回类型是String
3.子类方法不能缩小父类方法的访问权限


重载：
发生范围： 本类
方法名：必须一样
形参列表：类型，个数或者顺序至少有一个不同。
返回类型：无要求  输入字符用String
输入整数用int
修饰符：无要求，因为在同一个本类里做

重写：
发生范围：父子类
方法名：必须一样
形参列表：相同
返回类型：子类重写的方法，放回的类型和父类返回的类型一致，或者是其子类
修饰符：子类方法要比父类大或相同


package com.hspedu.encapsulate.override_;

public class OverrideExcercise {
    public static void main(String[] args) {

        Person person = new Person("jack",25);
        System.out.println(person.say());

        Student smith = new Student("smith", 122, "11s", 55);

        System.out.println(smith.say());
    }
}

class Person {
    private String name;
    private int age;


    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }



    public String say() {


        return "name="+ name+"age="+age;

    }
}

class Student extends Person {
    private String id;
    private int score;


    public Student(String name, int age, String id, int score) {
        super(name, age);
        this.id = id;
        this.score = score;
    }

    public String say() {
     return super.say()+"id=" + id + "score=" + score;
    }
}

Object 类详解
1.提高具有哈希结构的容器的效率！
2.两个引用，如果只想的是同一个对象，则哈希值肯定是一样
3.两个引用，如果指向的是不同对象，则哈希值是不一样的
4.哈希值主要跟句地址号来的！，不能完全将哈希值等价于地址
5.案例演示
后面在集合，中hashcode 如果需要的话，也会重写

public class HashCode {
    public static void main(String[] args) {
        AAA aa = new AAA();
        AAA aa2 = new AAA();
        AAA aa3 = aa;//两个引用两个哈希值不一样 aa 和aa3一样
//aa.hashCode ==aa3.hashCode但两个不是地址
    }
}
class AAA{}

toString 方法
基本介绍
默认返回： 全类名+ @+ 哈希值的十六进制
子类往往重写toString方法，用于返回对象的属性信息
重写toString方法， 打印对象或拼接对象时，都会自动调用该对象的toString形式
当直接输出一个对象时，toString方法会被默认的调用
package com.hspedu.encapsulate.ToString_;

public class ToString {
//    public String toString() {
//        return getClass().getName() + "@" + Integer.toHexString(hashCode());
//    }getClass().getName() 类的全类名（包名+ 类名）
//       Integer.toHexString(hashCode()) 将对象的hasCode
public static void main(String[] args) {
    Monster monster = new Monster("小妖怪", "巡山的", 1000);
    System.out.println(monster.toString()+" "+ monster.hashCode());
    System.out.println(monster.toString());
 System.out.println(monster);//默认加了toString
    //重写toString方法，输出对象的属性
    //使用快捷键即可 alt + insert

}
}
class Monster{
    private String name;
    private String job;
    private double sal;

    @Override
    public String toString() {//重写
        return "Monster{" +
                "name='" + name + '\'' +
                ", job='" + job + '\'' +
                ", sal=" + sal +
                '}';
    }

    public Monster(String name, String job, double sal) {
        this.name = name;
        this.job = job;
        this.sal = sal;

    }
}

Monster{name='小妖怪', job='巡山的', sal=1000.0} 460141958
Monster{name='小妖怪', job='巡山的', sal=1000.0}
Monster{name='小妖怪', job='巡山的', sal=1000.0}

实际开发中几乎不会使用===========
Object类详解
finalize方法
1.当对象被回收时，系统自动调用该对象的finalize方法。子类可以重写该方法，做一些释放资源的操作
2.什么时候被回收：当某个对象没有任何引用时，则jvm就认为这个对象是一个垃圾对象，就会使用垃圾回收机制来销毁该对象，在销毁该对象前，会先调用finalize方法。
3.垃圾回收机制的调用，是由系统来决定，也可以通过System.gc()主动触发垃圾回收机制，测试 Car【name】


package com.hspedu.encapsulate.Object_;

public class Finalize {
    public static void main(String[] args) {
        Car bmw = new Car("宝马");
        bmw = null;//这时 car对象就是一个垃圾，垃圾回收器就会回收（销毁
//对象 在销毁对象前，会调用该对象的finalize方法// 程序员就可以在fina中，写自己的业务逻辑代码
        //（比如是释放资源：数据库链接，或者打开文件..）
        //如果程序员不重写finalize，name就会调用Object类的finalize，即默认处理
        //如果程序员重写了finalize方法就可以实现自己的逻辑了
        System.gc();//主动调用垃圾回收期
        System.out.println("成绩退出了");
}
}

class Car{
    private String name;
    public Car(String name){
        this.name = name;
    }

    @Override//重写
    protected void finalize() throws Throwable {
        System.out.println("我们销毁汽车" + name);
        System.out.println("释放了某些资源...");
    }
}

=======================

断掉调式Debug系统
一个实际需求
1.在开发中，新手程序员在查找错误时，这时老程序员就会温馨提示，可以用断电调试
一步一步的看源码执行的过程，从而发现错误所在。
2.重要提示：在断点调试过程中，是运行状态，是以对象的 运行类型来执行的。
A extends B; B b = new A（）；b.xx();

断点调试介绍

1.断点调试是指在程序的某一行设置一个断点，调试时，程序运行到这一行就会停住，然后你可以一步一步往下调试，调试过程中可以看各个变量当前的值，出错的话，调试到出错的代码行即显示错误，停下。进行分析从而找到这个Bug
2.断点调试是程序员必须掌握的技能。
3.断点调试也能帮助我们查看java底层源代码的执行过程，提高程序员的java水平。

断点调试的快捷键
F7（跳入）F8（跳过）shift+F8（跳出）F9（resume, 执行到下一个断点）
F7：跳入方法内
F8：逐行执行代码
shift + F8： 跳出方法

断点调试应用案例
看几段代码，演示调试过程
断点可以再debug过程中，动态的 下断点

1.使用断点调试的方法，追踪下一个对象创建的过程。Person【name，age，构造器】
2.我们使用断点调试，查看动态绑定机制的如何工作












