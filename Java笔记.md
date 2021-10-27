# Java笔记（第一版）

## 异常

1. 异常分类（编译时异常、运行时异常）

   - 错误error
   - 受控异常（强制用try-catch或throws处理）
   - 非受控异常（也叫运行时异常，不强制用try-catch或throws处理，一般是逻辑性错误）

2. 异常举例

   - 空指针异常
   - 数组角标越界异常
   - 类型转换异常
   - 文件未找到异常
   - 算术异常
   - 方法不存在异常
   - 方法传递参数异常
   - 转换异常
   - IO异常
   - ......

3. throws和throw区别

   throws用在方法声明处，后边跟的是异常类；throw主动抛出异常，后边跟的是异常对象，throw下边不要写其他代码，因为抛出异常后执行不到；throws代表可能有异常，throw代表一定有异常。

4. Error与Exception区别

   都继承了Throwable类，error是系统错误（如栈溢出、内存不足），不能通过程序进行处理，exception能通过程序捕捉并处理。

5. 如何自定义异常

   继承RuntimeException或Exception类

6. 异常处理机制有哪些

   try-catch-finally和throws
   
7. try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？

   执行，如果finally中也有return则返回的是finally中的值，不建议在finally中使用return

8. final、finally、finalize 有什么区别？

   - final是用来修饰类、方法和变量的。修饰类表示该类不能被继承，修饰方法表示该方法不能被重写，修饰变量表示该变量是一个常量。
   - finally用在try-catch中处理异常，可省略，表示不管出不出现异常都回执行，一般用来关闭资源。
   - finalize是Object类中的一个方法，而Object是所有类的父类，用finalize()方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。
   
9. try-catch-finally

   处理异常，使得编译时不报错，但是运行时仍可能报错，相当于将编译时的异常延迟到运行时出现。运行时异常一般不处理。

10. 子类重写父类方法抛出的异常应比父类方法抛出的异常小，如果父类方法没抛异常则子类重写的父类方法也不能抛异常。

11. 什么时候使用try-catch-finally，什么时候使用throws

    - 父类方法没用throws处理异常，子类重写方法只能使用try-catch-finally处理异常

    - 执行方法a中，先后调用了另外几个方法，这几个方法是递进关系执行的，建议这几个方法用throws方式处理，执行的方法a用try-catch-finally处理。

      main：1→2→3，如果1中调用的方法产生异常数据，则2 3中也应继续使用1中产生的异常数据，而如果在方法中早已将异常处理了，则2 3中使用的是处理后的数据，产生误差。
    
12. 自定义异常如果继承Exception则可能出现异常的代码应用try-catch，不能throws；如果继承RuntimeException则能使用throws

![image-20210707214833332](C:\Users\Administrator\Desktop\JAVA\image-20210707214833332.png)

## 枚举

1. 类的对象只有有限个，确定的，我们称此类为枚举类
2. 当需要定义一组常量时，强烈建议使用枚举类
3. 如果枚举类中只有一个对象，则可以作为单例模式的实现方式
4. 枚举类对象都是public static final的（静态常量），名称都是大写
5. 定义枚举类

```java
public class EnumDemo {

    public static void main(String[] args){
        //直接引用
        Day day =Day.MONDAY;
    }

}
//定义枚举类型
enum Day {
    MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY, SUNDAY;
}
```

## 接口

1. 接口中的属性都是public static final（可省略），方法都是public abstract（可省略）

2. 接口像是一种标签，标明实现接口的类具有该接口中的所有功能，例如：USB定义了一种标准，鼠标键盘实现了该标准，就能插USB接口上用

3. 一个类如果实现了一个接口，它就必须实现该接口中的所有方法（方法都是抽象的），如果接口中没有任何内容，则该接口只起标识作用（如Serializable可序列化）

4. 接口中不能定义构造器，意味着接口不能实例化

5. 接口多态：

   ![image-20210710114055824](C:\Users\Administrator\Desktop\JAVA\image-20210710114055824.png)

  6. 类是单继承的，接口可以多继承，既有父接口中的方法也能有自己的方法

## 静态工厂

1. 静态工厂方法：每次被调用的时候不一定要创建一个新的对象

2. ![image-20210710114055824](C:\Users\Administrator\Desktop\JAVA\image-20210710114055824.png)

   使用静态工厂：

   ```java
   class Child{
       int age = 10;
       int weight = 30;
       public static Child newChild(int age, int weight) {
           Child child = new Child();
           child.weight = weight;
           child.age = age;
           return child;
       }
       public static Child newChildWithWeight(int weight) {
           Child child = new Child();
           child.weight = weight;
           return child;
       }
       public static Child newChildWithAge(int age) {
           Child child = new Child();
           child.age = age;
           return child;
       }
   }
   ```

3. 静态工厂的优势：

   - 静态工厂方法与构造器不同的第一优势在于，他们有名字。如果构造器的参数本身并不能描述清楚返回的对象，那么具有确切名称的静态工厂则代码可读性更佳
   
   - 不用每次被调用时都创建新对象
   
   - 可以返回原返回类型的子类
   
     ```java
     public class Person {//父类
         public static Person p (){
             return new Child();//返回子类
         }
         public static void main(String[] args) {
             Person.p();//调用
         }
     }
     class Child extends Person{//子类继承父类
     }
     ```
   
   - 在创建带泛型的实例时，能使代码变得简洁
   
   - 可以有多个参数相同但名称不同的工厂方法（2中的图就是个反例）
   
   - 可以减少对外暴露的属性
   
   - 多了一层控制，方便统一修改


## 单例模式

1. - 单例类只能有一个实例
   - 单例类必须自己创建自己的唯一实例
   - 单例类必须给所有其他对象提供这一实例
   
2. 懒汉式：

   ```java
   //懒汉式单例类.在第一次调用的时候实例化自己 
   public class Singleton {
       //1.私有构造方法
       private Singleton() {}
       //2.创建对象
       private static Singleton single=null;
       //3.静态工厂方法，返回对象 
       public static Singleton getInstance() {
            if (single == null) {  
                single = new Singleton();
            }  
           return single;
       }
   }
   ```
   
   - 线程不安全，第一次使用该单例才会实例化对象，第一次调用才会初始化，若果做得工作比较多，性能上会有些延迟
   
   - 处理线程安全：
   
     - 方法一：
   
       在方法上加同步
   
       ```java
       public static synchronized Singleton getInstance() {
                if (single == null) {  
                    single = new Singleton();
                }  
               return single;
       }
       ```
     
     - 方法二：
     
       双重检查锁定
     
       ```java
       public class Singleton{
       	private Singleton(){}
       	private static volatile Singleton singleton=null;
       	public static Singleton getInstance() {
               	if (singleton == null) {  
                  	 synchronized (Singleton.class) {  
                      	if (singleton == null) {  
                         	singleton = new Singleton(); 
                     	 }  
                  	 }  
              	 }  
              	 return singleton; 
       	}
       }
       ```
     
      - 方法三
     
        静态内部类
     
        ```java
        public class Singleton {  
            private static class LazyHolder {  
               private static final Singleton INSTANCE = new Singleton();  
            }  
            private Singleton (){}  
            public static final Singleton getInstance() {  
               return LazyHolder.INSTANCE;  
            }  
        }
        ```
   
  3. 饿汉式

     ```java
     //饿汉式单例类.在类初始化时，已经自行实例化 
     public class Singleton1 {
         //私有构造方法
         private Singleton1() {}
         //创建对象
         private static final Singleton1 single = new Singleton1();
         //3.静态工厂方法，返回对象 
         public static Singleton1 getInstance() {
             return single;
         }
     }
     ```

     - 饿汉式在类创建的同时就已经创建好一个静态的对象供系统使用，以后不再改变，所以天生是线程安全的

## 集合

1. Collection接口下有三个接口，分别是List、Set、Queue；Map为独立接口

2. List（列表）：有序的容器，允许元素重复，可以插入null，每个元素有索引，适合搜索和修改节点内容。

   - ArrayList：数组列表，List的一个主要实现类，实现了动态扩容数组，底层是通过数组实现的，默认长度为10，允许快速随机存取，适用于查找和修改数据，能直接通过索引位置定位数据，但不适合插入删除数据，插入删除会导致数组重新排列，ArrayList是线程不安全的。查找快，增删慢

   - LinkedList：链表列表，双向链表，可以对链表的头尾进行插入和删除，与ArrayList先比更适合插入和删除，LinkList也是线程不安全的。查找慢，增删快

   - Vector：和ArrayList大致相同，用synchronize同步了方法，线程安全的，也因此效率不如ArrayList。扩容时，ArrayList每次增加50%，Vector增加一倍。基本已被弃用。
   
  3. ArrayList、LinkedList和Vector异同
     - 同：都实现了List接口，都是有序的可重复的
     - 异：ArrayList底层是数组，查找快，增删慢，线程不安全；LinkedList底层是双向链表，查找慢，增删快，线程不安全；Vector和ArrayList基本相同，是线程安全的
     
  4. 数组的遍历

     - 迭代器

       ![image-20210710114055824](C:\Users\Administrator\Desktop\JAVA\image-20210710114055824.png)

      - foreach循环

        ![image-20210714092732020](C:\Users\Administrator\Desktop\JAVA\image-20210714092732020.png)

  5. Set（组）：无序的，不可重复的，向Set中添加元素所在的类一定要重写hashcode和equals方法
     
     - HashSet：Set接口的主要实现类，线程不安全的，可以存储null，底层是数组+链表
     
     - LinkedHashSet：HashSet的子类，遍历内部数据时可以按照添加的顺序遍历，但其本身依然是无序的
     
     - TreeSet：可以按照添加对象的指定属性进行排序
     
     - 用Set不重复性去除List的重复数据
     
       ![image-20210714152241950](C:\Users\Administrator\Desktop\JAVA\image-20210714152241950.png)
     
  6. Map（图）：是一个键值对的集合，key是无序且唯一的，用Set存储，value允许重复，用Collection存储

     - HashMap：Map接口的主要实现类，结构为链表+数组+红黑树，默认长度为16，当链表长度大于阈值8且当前数组长度大于64时就会将这个节点上的所有数据改为用红黑树存储
       - 存取是无序的
       - 键和值都可以是null，但键值要求唯一，即只能存一个null
       - 键唯一，值可以相同
       - 线程不安全，效率高
       
     - HashMap的底层实现原理
     
       现将key-vlaue封装到Node对象中，然后底层调用key的hashcode方法得出hash值，在通过哈希算法将hash值转换成数组的下标，如果下标位置没有任何元素，则将Node添加到这个位置，如果有元素或链表，则调用key的equals方法逐个比较，如果都为false则将这个元素添加到末尾，如果返回true则覆盖这个位置的value
     
      - HashMap中能put两个相同key吗？
     
        不能，后一个会覆盖前一个的值。通过计算可能得出相同的数组下标，调用equals方法判断是否要添加新元素
     
       - HashMap的扩容机制
     
         7：默认长度为16，负载因子为0.75，当超出临界值16*0.75=12时且要存放的位置非空时扩容为原来的2倍
     
         8：
     
     - HashMap遍历
     
       ![image-20210715145114744](C:\Users\Administrator\Desktop\JAVA\image-20210715145114744.png)
     
     - Hashtable：线程安全的，效率低，不能存储null。基本已被弃用。
     
     - LinkedHashMap：HashMap的子类，能按照添加的顺序遍历，频繁的遍历操作执行效率高于HashMap
     
     - TreeMap：底层使用红黑树，按照添加的key-value进行排序，实现排序遍历，用key进行自然排序或定制排序
     
     - HashTable：线程安全的，不能存储null。基本已弃用
     
     - Properties：HashTable的子类，用于处理配置文件，由于配置文件里的属性都是字符串类型的，所以Properties的key-value也都是字符串类型的
     
     - Collections工具类：操作Collection和Map接口的工具类

## 反射





## 注解



## 泛型

1. 



## 多线程

1. 线程通信在哪里wait就在哪里唤醒，可能会出现虚假唤醒问题，所以不能用if要用while，虽然两者都有条件判断，但if是判断语句，while是循环语句

   ![image-20210720094541914](C:\Users\Administrator\Desktop\JAVA\image-20210720094541914.png)
   
   ![image-20210721111645261](C:\Users\Administrator\Desktop\JAVA\image-20210721111645261.png)
   
2. 多线程4种方法：

   - 继承Thread类
   - 实现Runnable接口（无返回值，run方法，无异常）
   - 实现Callable接口（有返回值，call方法，无法计算结果会抛出异常）
   - 线程池

3. 创建Callable

   ![image-20210722094636165](C:\Users\Administrator\Desktop\JAVA\image-20210722094636165.png)

4. FutureTask只汇总一次，再调用直接出结果。例如：试了各种大小款式的东西最终才决定买哪个，下次不用各种试直接买

5. JUC强大的辅助类

   - CountDownLatch

     设置一个初始值，



## 锁

1. synchronized锁

   - java内置的关键字
   - 自动完成，如果发生异常会自动释放锁，不会出现死锁
   - 等待的线程会一直等下去，不能够响应中断

   ![image-20210719205033526](C:\Users\Administrator\Desktop\JAVA\image-20210719205033526.png)

2. lock锁

   - 是一个接口
   - 需要手动释放锁
   - 能知道是否获得锁，比较灵活
   - 能提高多个线程进行读操作的效率
   - 当竞争资源不激烈时，lock和synchronized性能差不多，当竞争资源非常激烈时（即有大量线程同时竞争），此时lock的性能远远优于synchronized

   ![image-20210720094933822](C:\Users\Administrator\Desktop\JAVA\image-20210720094933822.png)
   
  3. 集合线程安全

     ![image-20210722091534971](C:\Users\Administrator\Desktop\JAVA\image-20210722091534971.png)

  4. synchronized和lock都是可重入锁。lock要手动上锁解锁，上锁解锁次数要一样

  5. 死锁：多线程因为争夺资源而造成的一种互相等待的现象，若果没有外力干涉他们就无法再执行下去

     ![image-20210722092728868](C:\Users\Administrator\Desktop\JAVA\image-20210722092728868.png)

6. 

## 容器





## 内部类

1. 成员内部类
   - 定义在一个类的内部，成员内部类可以无条件访问外部类的所有成员属性和成员方法（包括private和static）
   - 如果内部类属性或方法和外部类重名，使用 *外部类.this.属性/方法* 访问
   - ![image-20210719100854888](C:\Users\Administrator\Desktop\JAVA\image-20210719100854888.png)
2. 局部内部类
   - 在方法内定义的类，不能有修饰符
   - 仅在此方法内使用
   - 无法创建静态信息
   - 可以直接访问方法内的局部变量和参数，但不能更改
   - 可以随意的访问外部类的任何信息
3. 匿名内部类
   - 匿名内部类不能定义构造函数
   - 匿名内部类不能存在任何静态成员和静态方法
   - 匿名内部类不能是抽象的，必须实现继承的类或接口的所有抽象方法
   - ![image-20210719104438591](C:\Users\Administrator\Desktop\JAVA\image-20210719104438591.png)
4. 静态内部类
   - ![image-20210719101815526](C:\Users\Administrator\Desktop\JAVA\image-20210719101815526.png)

## String



## 其他

2. 里氏代换原则：能用父类的地方一定能用子类
3. 任何时候，如果要执行类中的static方法（包括抽象类），都可以在没有对象的情况下直接调用
4. 静态方法和静态属性都属于类，不属于对象，可以直接调用；非静态方法也叫实例方法、成员方法，属于对象，不属于类，必须创建对象，通过对象调用。
5. 软件开发中有一条很重要的经验：对外暴露的属性越多，调用者越容易出错
6. 类的加载顺序：父类静态属性方法、父类静态代码块、子类静态属性方法、子类静态代码块、父类成员变量（非静态字段）、父类非静态代码块、父类构造方法（创建对象）、子类成员变量（非静态字段）、子类非静态代码块、子类构造方法（创建对象）
7. sout输出对象，如果没重写tostring方法输出的是地址值，重写了则默认调用tostring方法
8. 被final修饰的成员属性必须被初始化赋值，如果是基本数据类型则不可被修改，引用类型则不能再指向另一个对象
9. 

# GIT

1. 初始化git管理目录：git init，创建空的git目录：git init 目录名字
2. 查看状态：git status
3. 将修改的文件添加到缓存区：git add 文件名，添加所有修改的文件git add .
4. 将缓存区文件提交到版本库：git commit -m “版本名”
5. 查看提交记录：git log，查看某一记录之前的记录：git log 版本号，查看提交修改记录：git reflog
6. 回滚回某个版本：git reset --hard 版本号（缓存区，暂存区都清空），git reset soft 版本号（由版本库回滚到暂存区），git reset mix 版本号（由版本号回滚到修改状态），git reset head 文件名（将add提交的文件恢复到修改状态），git checkout 文件名（将修改状态恢复到未修改状态）
7. 创建分支：git branch 分支名，切换分支：git checkout 分支名，查看分支：git branch，合并分支：git merge 分支名（增加的增加，删减的删减，修改的修改），删除分支：git branch -d 分支名
8. 提交github：git remote add 名字 地址，git push -u 名字 分支名
9. 111





