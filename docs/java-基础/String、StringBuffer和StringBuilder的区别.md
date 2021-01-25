# String、StringBuffer 和 StringBuilder 的区别

## 可变性

- String 类中使用 **final** 关键字修饰字符数组来保存字符串，所以 String 对象是不可变的。

  - 在 Java 9 之前：

    > String 类的实现使用 char 数组存储字符串。
    >
    > ```java
    > private final char[] value
    > ```

  - 在 Java 9 之后：

    > String 类的实现改用 byte 数组存储字符串。
    >
    > ~~~java
    > private final byte[] value
    > ~~~

- StringBuffer 继承自 AbstractStringBuilder 类，没有用 **final** 关键字修饰，所以 StringBuffer 对象是可变的。

- StringBuilder 也继承 AbstractStringBuilder 类，没有用 **final** 关键字修饰，所以 StringBuilder 对象也是可变的。



> AbstractStringBuilder类源码
>
> ```java
> abstract class AbstractStringBuilder implements Appendable, CharSequence {
>     /**
>      * The value is used for character storage.
>      */
>     char[] value;
>     
>     /**
>      * The count is the number of characters used.
>      */
> 		int count;
>   
>     AbstractStringBuilder(int capacity) {
>         value = new char[capacity];
>     }
> }
> ```



## 线程安全

- String 可以理解为常量，所以是线程安全的。
- StringBuilder 没有对方法进行同步操作，所以是非线程安全的。
- StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。



## 性能

- String 类型每次进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。所以，性能一般。
- StringBuilder 每次都会对 StringBuilder 对象本身进行操作，而不是生成新的对象并改变对象引用。所以，性能最好。
- StringBuffer 跟 StringBuilder 情况类似，但由于 StringBuffer 对方法内进行同步操作，效率比 StringBuilder 低 10%~15% 左右。所以，性能比 StringBuilder 低，比 String 高。



## 三者详细区别

|   区别点   |   String   | StringBuilder |          StringBuffer        |
| :-------: | :--------: | :-----------: | :--------------------------: |
|   可变性   |   不可变    |     可变       |            可变              |
|  线程安全性 |  线程安全   |  非线程安全     |           线程安全            |
|    性能    | 产生新对象  | 操作对象本身     |  操作对象本身，方法里有同步操作  |



## JVM 对 String 的优化

String 在使用 '+'  和  '+='  操作符时，Java 编译器（即 JVM ）会隐式创建 StringBuilder 对象，调用 append() 方法来拼接，然后将拼接后的 StringBuilder 对象用 toString() 方法处理成 String 对象。

所以，如果在循环体中用 String 做拼接，实际上循环一次就创建一个 StringBuilder 对象，会占用较多内存。因此，推荐在循环体外部创建一个 StringBuilder 变量代替 String。



## 使用建议

- 操作少量的数据: 适用 String。
- 单线程操作字符串缓冲区下操作大量数据: 适用 StringBuilder。
- 多线程操作字符串缓冲区下操作大量数据: 适用 StringBuffer。