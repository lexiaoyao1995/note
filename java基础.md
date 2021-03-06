# JavaSE

### 序列化

Java 串行化技术把一个对象的状态写入一个Byte 流里，并且可以从其它地方把该Byte 流里的数据读出来，重新构造一个相同的对象。

这种机制允许你将对象通过网络进行传播，并可以随时把对象持久化到数据库、文件等系统里。

Java的串行化机制是RMI、EJB等技术的技术基础。

用途：利用对象的串行化实现保存应用程序的当前工作状态，下次再启动的时候将自动地恢复到上次执行的状态。

**序列化的实现**

​	将需要被序列化的类实现Serializable接口，然后使用一个输出流(如：FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)，要恢复的话则用输入流。

 **相关注意事项**
    a）序列化时，只对对象的状态进行保存，而不管对象的方法；
    b）当一个父类实现序列化，子类自动实现序列化，不需要显式实现Serializable接口；
    c）当一个对象的实例变量引用其他对象，序列化该对象时也把引用对象进行序列化；
    d）并非所有的对象都可以序列化，至于为什么不可以，有很多原因了,比如：

​        1.安全方面的原因，比如一个对象拥有private，public等field，对于一个要传输的对象，比如写到文件，或者进行rmi传输等等，在序列化进行传输的过程中，这个对象的private等域是不受保护的。

       2. 资源分配方面的原因，比如socket，thread类，如果可以序列化，进行传输或者保存，也无法对他们进行重新的资源分配，而且，也是没有必要这样实现。

### 注解

元注解即作用在注解上的注解，用来描述注解。

Java中内置了5中元注解，分别是： @Retention、@Documented、@Target、@Inherited、@Repeatable 。

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD,ElementType.TYPE, ElementType.METHOD})
public @interface Yeonon {
}
```

### transient关键字

transient关键字可修饰于类成员变量，作用是当类的对象发生序列化的时候，最终的序列化内容不包括被修饰的成员变量。

### Static关键字

static变量放在jvm内存的方法区

1. **修饰方法**

static方法一般称作静态方法，由于静态方法不依赖于任何对象就可以进行访问；

在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用；

可以直接通过类名.直接调用。

2. **修饰变量**

static变量也称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。

static成员变量的初始化顺序按照定义的顺序进行初始化。

3. **静态代码块**

static关键字还有一个比较关键的作用就是 用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

为什么说static块可以用来优化程序性能，是因为它的特性:只会在类加载的时候执行一次。

> 静态代码块会优先于构造方法加载，不论是否是继承关系
>
> **顺序：**
>
> 父类静态代码
>
> 子类静态代码
>
> 父类构造函数
>
> 子类构造函数

### 反射

Java反射机制是在运行状态中，对于任意一个类，都能知道这个类的所以属性和方法；对于任何一个对象，都能够调用它的任何一个方法和属性；这样动态获取新的以及动态调用对象方法的功能就叫做反射。

#### 缺点

* 无法得到编译期的安全保证
* 性能是一个问题，反射相当于一系列解释操作，通知jvm要做的事情，性能比直接的java代码要慢很多。

### 泛型

在没有泛型的情况的下，通过对类型 Object 的引用来实现参数的“任意化”，“任意化”带来的缺点是要做显式的强制类型转换，而这种转换是要求开发者对实际参数类型可以预知的情况下进行的。对于强制类型转换错误的情况，编译器可能不提示错误，在运行的时候才出现异常，这是本身就是一个安全隐患。

那么泛型的好处就是在编译的时候能够检查类型安全，并且所有的强制转换都是自动和隐式的。

**上界通配符 < ? extends E>**

> 上届：用 extends 关键字声明，表示参数化的类型可能是所指定的类型，或者是此类型的子类。

**下界通配符 < ? super E>**

> 下界: 用 super 进行声明，表示参数化的类型可能是所指定的类型，或者是此类型的父类型，直至 Object

**？ 和 T 的区别**

？和 T 都表示不确定的类型，区别在于我们可以对 T 进行操作，但是对 ？ 不行，比如如下这种 ：

```java
// 可以
T t = operate();

// 不可以
？ car = operate();
```

简单总结下：

T 是一个 确定的 类型，通常用于泛型类和泛型方法的定义，？是一个 不确定 的类型，通常用于泛型方法的调用代码和形参，不能用于定义类和泛型方法。

#### 泛型擦除

经常在论坛、社区里听到Java的泛型实现是伪泛型，而C#、C++的泛型实现才是真正的泛型。这么说是有原因的，因为Java源码编译后的字节码里不存在什么类型参数。

### final关键字

1. 修饰类

   类不能被继承

2. 修饰方法

   方法不能被重写

3. 修饰变量

   变量不能被修改，并且必须要**初始化**

### 基本类型以及所占字节

| byte    | 1            |
| ------- | ------------ |
| short   | 2            |
| int     | 4            |
| long    | 8            |
| char    | 2 可以存汉字 |
| float   | 4            |
| double  | 8            |
| boolean | 2            |

##### 汉字

```java
public class Main {

	public static void main(String[] args) throws Exception {

		char a = '汉';
		String aa = "汉";
		System.out.println(a);
		System.out.println("utf-8 :" + aa.getBytes("UTF-8").length);
		System.out.println("GBK :" + aa.getBytes("GBK").length);
		System.out.println("unicode :" + aa.getBytes("unicode").length);

	}

}
```

汉 utf-8 :3 GBK :2 unicode :4

一个char可以存放汉字

### 多态

它是指在父类中定义的属性和方法被子类继承之后，可以具有不同的数据类型或表现出不同的行为，这使得同一个属性或方法在父类及其各个子类中具有不同的含义。

对面向对象来说，多态分为编译时多态和运行时多态。其中**编译时多态是静态的，主要是指方法的重载**，它是根据参数列表的不同来区分不同的方法。通过编译之后会变成两个不同的方法，在运行时谈不上多态。而运行时多态是动态的，它是通过动态绑定来实现的，也就是大家通常所说的多态性。

#### 多态的实现机制

多态的底层实现是动态绑定，即在运行时才把方法调用与方法实现关联起来。

在Java语言中，基类的引用变量不仅可以指向基类的实例对象，也可以指向其子类中的实例对象。同样，接口中的引用变量也可以指向其实现类的实例对象。而程序调用的方法在运行时期才动态绑定（绑定是指将一个方法调用和一个方法主体联系在一起），绑定的是引用变量所指向的具体实例对象的方法，也就是内存中正在运行的那个对象的方法，而不是引用变量的类型中定义的方法。通过这种动态绑定实现了多态。由于只有在运行时才能确定调用哪个方法，因此通过方法覆盖实现的多态也可以被称为运行时多态。

##### 静态绑定与动态绑定

静态绑定在编译期就已经确定，这是因为静态方法、构造器方法、私有方法和父类方法可以唯一确定。这些方法的符号引用在类加载的解析阶段就会解析成直接引用。

动态绑定方法的方法调用与方法实现的关联（也就是分派）有两种，一种是在编译期确定，被称为静态分派，比如方法的重载；一种是在运行时确定，被称为动态分派，比如方法的覆盖。对象方法基本上都是虚方法。

### java创建对象的方式

1、new关键字

可以调用任意的构造函数

2、Class类的newInstance方法

newInstance方法调用无参的构造函数创建对象

3、Constructor类的newInstance方法

 java.lang.reflect.Constructor类里也有一个newInstance方法可以创建对象。我们可以通过这个newInstance方法调用有参数的和私有的构造函数。

4、clone

无论何时我们调用一个对象的clone方法，jvm就会创建一个新的对象，将前面对象的内容全部拷贝进去。用clone方法创建对象并不会调用任何构造函数。

要使用clone方法，我们需要先实现Cloneable接口并实现其定义的clone方法。

5、使用反序列化

当我们序列化和反序列化一个对象，jvm会给我们创建一个单独的对象。在反序列化时，jvm创建对象并不会调用任何构造函数。
为了反序列化一个对象，我们需要让我们的类实现Serializable接口

### 抽象类和接口

接口比抽象类更加抽象，因为抽象类中可以定义构造器，可以有抽象方法和具体方法，而接口中不能定义构造器而且其中的方法全部都是抽象方法。抽象类中的成员可以是private、默认、protected、public的，而接口中的成员全都是public的。抽象类中可以定义成员变量，而接口中定义的成员变量实际上都是常量。有抽象方法的类必须被声明为抽象类，而抽象类未必要有抽象方法。

### 编译型语言和解释型语言

**编译型语言**

只须编译一次就可以把源代码编译成机器语言，后面的执行无须重新编译，直接使用之前的编译结果就可以；因此其执行的效率比较高；

源代码—>机器语言

编译性语言代表：C、C++、Pascal/Object Pascal（Delphi）；

**解释型语言**

源代码不能直接翻译成机器语言，而是先翻译成中间代码，再由解释器对中间代码进行解释运行；

源代码—>中间代码—>机器语言

解释性语言代表：Python、JavaScript、Shell、Ruby、MATLAB等；

> java语言应是编译性-解释性语言，因为其同时具备编译性和解释性两种特性；
>
> java文件先编译成与平台无关的.class的字节码文件，然后.class的字节码文件既可以在Windows平台上的java虚拟机（JVM）上进行解释运行，也可以在Linux平台上的JVM上解释运行；而JVM的翻译过程时解释性的，JVM从.class的字节码文件中读出一条指令，翻译一条指令，然后执行一条指令，这个过程就称为java的解释执行；

### clone一个对象

有两种方式：

1). 实现Cloneable接口并重写Object类中的clone()方法;

2). 实现Serializable接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆，代码如下。

#### 浅拷贝和深拷贝

浅拷贝：被复制对象的所有变量都含有与原来的对象相同的值，而所有的对其他对象的引用仍然指向原来的对象。换言之，浅复制仅仅复制所考虑的对象，而不复制它所引用的对象。

深拷贝：被复制对象的所有变量都含有与原来的对象相同的值，除去那些引用其他对象的变量。那些引用其他对象的变量将指向被复制过的新对象，而不再是原有的那些被引用的对象。换言之，深复制把要复制的对象所引用的对象都复制了一遍。

### 内部类

- 内部类方法可以访问该类定义所在作用域中的数据，包括被 private 修饰的私有数据
- 内部类可以对同一包中的其他类隐藏起来
- 内部类可以实现 java 单继承的缺陷
- 当我们想要定义一个回调函数却不想写大量代码的时候我们可以选择使用匿名内部类来实现

### String

#### 不可变性

**字符串常量池的实现**

在Java中，我们通常有两种方式创建字符串对象，一种是通过字符串字面量方式创建，就如上文的代码，另外一种就是通过 new 方式去创建，如 `String c = new String("string 3");` 而两者区别就在于通过字符串字面量的方式创建时，JVM 会现在字符串池中检查字符串内容是否已经存在，如果存在就会直接返回对应的引用，而不是再次分配内存进行创建，如果不存在就会分配在内存中创建的同时将字符串数据缓存在字符串池中，便于重用。正是是由于字符串的不可变，同样的字符串内容可以让 JVM 可以减少额外的内存分配操作，直接使用在字符串池中字符串对象即可，对性能提升和内存节省都大有好处。

**Hashcode 缓存**

字符串作为基础的数据结构，大量地应用在一些集合容器之中，尤其是一些散列集合，在散列集合中，存放元素都要根据对象的 `hashCode()` 方法来确定元素的位置。由于字符串  `hashcode`  属性不会变更，保证了唯一性，使得类似 HashMap，HashSet 等容器才能实现相应的缓存功能。由于 String 的不可变，**避免重复计算** `hashcode`，只有使用缓存的 `hashcode` 即可，这样一来大大提高了在散列集合中使用 String 对象的性能。

**线程安全**

在多线程中，只有不变的对象和值是线程安全的，可以在多个线程中共享数据。由于 String 天然的不可变，当一个线程”修改“了字符串的值，只会产生一个新的字符串对象，不会对其他线程的访问产生副作用，访问的都是同样的字符串数据，不需要任何同步操作。

**安全性**

由于字符串无论在任何 Java 系统中都广泛使用，会用来存储敏感信息，如账号，密码，网络路径，文件处理等场景里，保证字符串 String 类的安全性就尤为重要了，如果字符串是可变的，容易被篡改，那我们就无法保证使用字符串进行操作时，它是安全的，很有可能出现 SQL 注入，访问危险文件等操作。

### HashCode 和 equals

HashMap的很多函数要基于hashCode()方法和equals()方法，hashCode()用来定位要存放的位置，equals()用来判断是否相等。

如果没有正确的实现这两个方法，两个不同的键可能会有相同的hash值，因此可能会被集合认为是相等的。
而且，这两个方法也用来发现重复元素，所以这两个方法的实现对**HashMap**的精确性和正确性是至关重要的。

**总结**

* 同一个对象（没有发生过修改）无论何时调用hashCode()，得到的返回值必须一样。
* hashCode()返回值相等，对象不一定相等，通过hashCode()和equals()必须能唯一确定一个对象。
* 一旦重写了equals()，就必须重写hashCode()。而且hashCode()生成哈希值的依据应该是equals()中用来比较是否相等的字段。如果两个由equals()规定相等的对象生成的hashCode不等，对于HashMap来说，他们可能分别映射到不同位置，没有调用equals()比较是否相等的机会，两个实际上相等的对象可能被插入到不同位置，出现错误。其他一些基于哈希方法的集合类可能也会有这个问题。

# java IO

### 同步，异步，阻塞，非阻塞

同步	指的是用户进程触发IO操作并等待或者轮询的去查看IO操作是否就绪 

异步	异步是指用户进程触发IO操作以后便开始做自己的事情，而当IO操作已经完成的时候会得到IO完成的通知（异步的特点就是通知） 

**同步和异步的最大区别是，同步是用户方主动去查询IO操作是否完成，异步是IO操作主动通知用户方**

阻塞	所谓阻塞方式的意思是指, 当试图对该文件描述符进行读写时, 如果当时没有东西可读,或者暂时不可写, 程序就进入等待 状态, 直到有东西可读或者可写为止 

非阻塞	非阻塞状态下, 如果没有东西可读, 或者不可写, 读写函数马上返回, 而不会等待 

## Java 1.4 NIO

可以更高速进行io操作

速度的提高来自于所使用的结构更接近于操作系统执行I/O的方式：通道（Channel）和缓冲器(Buffer)

### 缓冲器（Buffer）

Buffer是一个对象，它包含着一些需要读取的数据或者是要传输的数据。

在面向流的I/O中我们直接通过流对象直接和数据进行交互的，但是在NIO中我们和数据的交互必须通过Buffer了。

![](pic/爱奇艺20190905150653.png)

### 通道（Channel）

`Channel`是一个对象，缓冲器可以通过它进行读取和写入数据。和原来的I/O做个比较，通道就像个流。正如前面所提到的，`Channel`是不和数据进行交互。但是它和流有一点不同，就是通道是双向的，而流只能是单向的（只能是InputStream或者OutputStream），但是通道可以用于读、写或者是同时用于读写。

1、fileChannel

note：

transferTo：把fileChannel中的数据拷贝到另外一个channel

transferFrom：把另外一个channel中的数据拷贝到fileChannel

避免两次用户态和内核态间的上下文切换，即零靠别，效率高

2、DatagramChannel

3、SocketChannel

4、ServerSocketChannel

![](pic/爱奇艺20190905150811.png)

### 选择器 Selector

Selector 是 Java NIO 的一个组件，它用于监听多个 Channel 的各种状态，用于管理多个 Channel。但本质上由于 FileChannel 不支持注册选择器，所以 Selector 一般被认为是服务于网络套接字通道的。

而大家口中的「NIO 是非阻塞的」，准确来说，指的是网络编程中客户端与服务端连接交换数据的过程是非阻塞的。普通的文件读写依然是阻塞的。

![](pic/爱奇艺20190905150931.png)

## Linux IO 模型

Java的NIO映射到Linux操作系统就是如上图所示的非阻塞I/O模型，就是io多路复用

### Nio下的三种轮询方式

 NIO的最重要的地方是当一个连接创建后，不需要对应一个线程，这个连接会被注册到多路复用器上面，所以所有的连接只需要一个线程就可以搞定，当这个线程中的多路复用器进行轮询的时候，发现连接上有请求的话，才开启一个线程进行处理，也就是一个请求一个线程模式。 

#### 忙查询

不停的把所有流从头到尾问一遍，又从头开始。这样就可以处理多个流了，但这样的做法显然不好，因为如果所有的流都没有数据，那么只会白白浪费CPU。 

#### 无差别轮询（poll/select） 

>   select==>时间复杂度O(n)
>
> 它仅仅知道了，有I/O事件发生了，却并不知道是哪那几个流（可能有一个，多个，甚至全部），我们只能无差别轮询所有流，找出能读出数据，或者写入数据的流，对他们进行操作。所以select具有O(n)的无差别轮询复杂度，同时处理的流越多，无差别轮询时间就越长。  

为了避免CPU空转，可以引进了一个代理（一开始有一位叫做select的代理，后来又有一位叫做poll的代理，不过两者的本质是一样的）。这个代理比较厉害，可以同时观察许多流的I/O事件，在空闲的时候，会把当前线程阻塞掉，当有一个或多个流有I/O事件时，就从阻塞态中醒来，于是我们的程序就会轮询一遍所有的流。

于是，如果没有I/O事件产生，我们的程序就会阻塞在select处。但是依然有个问题，我们从select那里仅仅知道了，有I/O事件发生了，但却并不知道是那几个流（可能有一个，多个，甚至全部），我们只能无差别轮询所有流，找出能读出数据，或者写入数据的流，对他们进行操作。

但是使用select，我们有O(n)的无差别轮询复杂度，同时处理的流越多，没一次无差别轮询时间就越长。

#### epoll

> epoll可以理解为event poll，不同于忙轮询和无差别轮询，epoll会把哪个流发生了怎样的I/O事件通知我们。所以我们说epoll实际上是事件驱动（每个事件关联上fd）的，此时我们对这些流的操作都是有意义的。（复杂度降低到了O(1)）

epoll可以理解为event poll，不同于忙轮询和无差别轮询，**epoll之会把哪个流发生了怎样的I/O事件通知我们**。此时我们对这些流的操作都是有意义的。（复杂度降低到了O(1)） 

> epoll_create 创建一个epoll对象，一般epollfd = epoll_create()
>
> epoll_ctl （epoll_add/epoll_del的合体），往epoll对象中增加/删除某一个流的某一个事件
>
> epoll_wait(epollfd,…)等待直到注册的事件发生

#### 三种方式区别

##### select的几大缺点：

（1）每次调用select，都需要把fd集合从用户态拷贝到内核态，这个开销在fd很多时会很大

（2）同时每次调用select都需要在内核遍历传递进来的所有fd，这个开销在fd很多时也很大

（3）select支持的文件描述符数量太小了，默认是1024

##### poll实现

poll的实现和select非常相似，只是描述fd集合的方式不同，poll使用pollfd结构而不是select的fd_set结构.

##### epoll（reference Link）

　　epoll既然是对select和poll的改进，就应该能避免上述的三个缺点。那epoll都是怎么解决的呢？在此之前，我们先看一下epoll和select和poll的调用接口上的不同，select和poll都只提供了一个函数——select或者poll函数。而epoll提供了三个函数，epoll_create,epoll_ctl和epoll_wait，epoll_create是创建一个epoll句柄；epoll_ctl是注册要监听的事件类型；epoll_wait则是等待事件的产生。

　　对于第一个缺点，epoll的解决方案在epoll_ctl函数中。每次注册新的事件到epoll句柄中时（在epoll_ctl中指定EPOLL_CTL_ADD），会把所有的fd拷贝进内核，而不是在epoll_wait的时候重复拷贝。epoll保证了每个fd在整个过程中只会拷贝一次。

　　对于第二个缺点，epoll的解决方案不像select或poll一样每次都把current轮流加入fd对应的设备等待队列中，而只在epoll_ctl时把current挂一遍（这一遍必不可少）并为每个fd指定一个回调函数，当设备就绪，唤醒等待队列上的等待者时，就会调用这个回调函数，而这个回调函数会把就绪的fd加入一个就绪链表）。epoll_wait的工作实际上就是在这个就绪链表中查看有没有就绪的fd（利用schedule_timeout()实现睡一会，判断一会的效果，和select实现中的第7步是类似的）。

　　对于第三个缺点，epoll没有这个限制，它所支持的FD上限是最大可以打开文件的数目，这个数字一般远大于2048,举个例子,在1GB内存的机器上大约是10万左右，具体数目可以cat /proc/sys/fs/file-max察看,一般来说这个数目和系统内存关系很大。 

#### Epoll具体实现

在内核的最底层是中断，类似系统回调的机制。网卡设备对应一个中断号, 当网卡收到网络端的消息的时候会向CPU发起中断请求, 然后CPU处理该请求. 通过驱动程序进而操作系统得到通知, 系统然后通知epoll, epoll通知用户代码。它能显著提高程序在大量并发连接中只有少量活跃的情况下的系统CPU利用率，因为它会复用文件描述符集合来传递结果而不用迫使开发者每次等待事件之前都必须重新准备要被侦听的文件描述符集合，另一点原因就是获取事件的时候，它无须遍历整个被侦听的描述符集，只要遍历那些被内核IO事件异步唤醒而加入Ready队列的描述符集合就行了。

epoll在被内核初始化时（操作系统启动），同时会开辟出epoll自己的内核高速cache区，用于安置每一个我们想监控的socket，这些socket会以红黑树的形式保存在内核cache里，以支持快速的查找、插入、删除。这个内核高速cache区，就是建立连续的物理内存页，然后在之上建立slab层，简单的说，就是物理上分配好你想要的size的内存对象，每次使用时都是使用空闲的已分配好的对象。

#### epoll和select的区别：

进程通过将一个或多个fd传递给select或poll系统调用，阻塞在select;这样select/poll可以帮我们侦测许多fd是否就绪；但是select/poll是顺序扫描fd是否就绪，而且支持的fd数量有限。linux还提供了一个epoll系统调用，epoll是基于事件驱动方式，而不是顺序扫描,当有fd就绪时，立即回调函数rollback

### AIO 异步io  基于事件和回调

##### AIO如何进一步加工处理结果

1、基于回调，实现CompletionHandle接口，调用时触发回调函数

2、返回future，通过isDone查看是否准备好，通过get等待返回数据



### Java对BIO、NIO、AIO的支持

Java BIO ： 同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。

Java NIO ： 同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。

Java AIO(NIO.2) ： 异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理，

### BIO、NIO、AIO的使用场景

BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。

NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。

AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。

# Collection

![](pic/爱奇艺20190726144548.png)



## 快速失败和安全失败

> ```
> fast-fail事件产生的条件：当多个线程对Collection进行操作时，若其中某一个线程通过iterator去遍历集合时，该集合的内容被其他线程所改变；则会抛出ConcurrentModificationException异常。
> fast-fail解决办法：通过util.concurrent集合包下的相应类去处理，则不会产生fast-fail事件。
> ```

Iterator的安全失败是基于对底层集合做拷贝，因此，它不受源集合上修改的影响。**java.util包下面的所有的集合类都是快速失败的，而java.util.concurrent包下面的所有的类都是安全失败的**。**快速失败的迭代器会抛出ConcurrentModificationException异常**，而安全失败的迭代器永远不会抛出这样的异常。

##### fail-fast原理

fail-fast机制，是一种错误检测机制。它只能被用来检测错误，因为JDK并不保证fail-fast机制一定会发生**。**

ConcurrentModificationException是在操作Iterator时抛出的异常。

从中，我们可以发现在调用 next() 和 remove()时，都会执行 **checkForComodification()**。若 “**modCount 不等于 expectedModCount**”，则抛出ConcurrentModificationException异常，产生fail-fast事件。

要搞明白 fail-fast机制，我们就要需要理解什么时候“modCount 不等于 expectedModCount”！
从Itr类中，我们知道 expectedModCount 在创建Itr对象时，被赋值为 modCount。通过Itr，我们知道：expectedModCount不可能被修改为不等于 modCount。所以，需要考证的就是modCount何时会被修改。

我们发现：无论是add()、remove()，还是clear()，只要涉及到修改集合中的元素个数时，都会改变modCount的值。

##### 在foreach中修改元素会抛出ConcurrentModificationException异常，属于快速失败

## List

### ArrayList

#### 构造方法

​	ArrayList提供了三种方式的构造器，可以构造一个默认初始容量为10的空列表、构造一个指定初始容量的空列表以及构造一个包含指定collection的元素的列表，这些元素按照该collection的迭代器返回它们的顺序排列的。

#### 添加

先调用了ensureCapacity(size+1)方法，判断数组容量是否足够，如果不够就扩容。

~~~java
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
~~~

#### 删除

~~~java
    public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
~~~

### CopyOnWriteList

Copy-On-Write简称COW，是一种用于程序设计中的优化策略。其基本思路是，从一开始大家都在共享同一个内容，当某个人想要修改这个内容的时候，才会真正把内容Copy出去形成一个新的内容然后再改，这是一种延时懒惰策略。

CopyOnWrite容器即写时复制的容器。通俗的理解是当我们往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行Copy，复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器。

**add**

~~~java
  private transient volatile Object[] array;

  public boolean add(E e) {

      final ReentrantLock lock = this.lock;
      lock.lock();

      try {
          Object[] elements = getArray();
          int len = elements.length;

          // 对数组拷贝一个副本出来
          Object[] newElements = Arrays.copyOf(elements, len + 1);

          // 对副本数组进行修改，比如在里面加入一个元素
          newElements[len] = e;

          // 然后把副本数组赋值给volatile修饰的变量
          setArray(newElements);
          return true;


      } finally {
          lock.unlock();
      }
  }
~~~



调用add方法的线程会首先获取锁，然后调用lock方法对list进行加锁（了解ReentrantLock的知道这是个独占锁，所以多线程下只有一个线程会获取到锁）

只有线程会获取到锁，所以只有一个线程会去更新这个数组，此过程中别的调用add方法的线程被阻塞等待

获取到锁的线程继续执行 

- 首先获取原数组以及其长度，然后将其中的元素复制到一个新数组中(newArray的长度是原长度+1)
- 给定数组下标为len+1处赋值
- 将新数组替换掉原有的数组

**get**

get方法这里也体现了`copy-on-write-list`的弱一致性问题。我们用下面的图示简略说明一下。图中给的假设情况是：threadA访问index=1处的元素

- ①获取array数组
- ②访问传入参数下标的元素

​	因为我们看到get过程是没有加锁的（假设array中有三个元素如图所示）。假设threadA执行①之后②之前，threadB执行remove(1)操作，threadB或获取独占锁，然后执行写时复制操作，即`复制一个新的数组neArray`，然后在newArray中执行remove操作(1)，更新array。threadB执行完毕array中index=1的元素已经是item3了。

​	然后threadA继续执行，但是因为threadA操作的是原数组中的元素，这个时候的index=1还是item2。所以最终现象就是虽然threadB删除了位置为1处的元素，但是threadA还是访问的原数组的元素。这就是`弱一致性问题`

![](pic/爱奇艺20190820135430.png)

**小节**

CopyOnWriteArrayList是如何保证`写`时线程安全的：使用ReentrantLock独占锁，保证同时只有一个线程对集合进行`写`操作

数据是存储在CopyOnWriteArrayList中的array数组中的，并且array长度是动态变化的（`写`操作会更新array）

在修改数组的时候，并不是直接操作array，而是复制出来了一个新的数组，修改完毕，再把旧的数组替换成新的数组

使用迭代器进行遍历的时候不用加锁，不会抛出ConcurrentModificationException异常，因为使用迭代器遍历操作的是数组的副本（当然，这是在别的线程修改list的情况）

## Map

### HashMap

1、数组加链表的实现方式

2、**容量大小**是2的幂次方的原因

可以用按位与的操作来计算余数，比求模快

3、并发读写会有什么风险

在容量超过填充因子时，进行rehash，因为hashMap为了避免尾部遍历，在链表的插入时，使用的是头插法，多线程场景下，可能会产生死循环

#### put方法的逻辑

1、如果HashMap未被初始化过，则初始化

2、对Key求Hash值，然后再计算下标

3、如果没有碰撞，直接放入桶中

4、如果碰撞了，以链表的方式链接到后面

5、如果链表长度超过阈值，就把链表转成红黑树

6、如果链表长度低于6，就把红黑树转回链表

7、如果节点已经存在就替换旧值

8、如果桶满了（容量16*负载因子0.75），就需要resize（扩容2倍后重排）

#### 如何有效减少碰撞

1、扰动函数：促使元素位置分布均匀，减少碰撞机会

2、使用final对象，并采用合适的equals和hashCode方法

#### 为什么hashMap的长度是2的指数倍

HashMap存取时，都需要计算当前key应该对应Entry[]数组哪个元素，即计算数组下标；算法如下：

**static** **int** indexFor(**int** h, **int** length) {

​        **return** h & (length-1);

​    }

hash%length==hash&(length-1)的前提是length是2的n次方； 

让取模运算更高效

#### HashMap扩容的问题

1、多线程环境下，调整大小会存在条件竞争，容易造成死锁

2、rehashing是一个比较耗时的过程

hashTable

早期java类库提供的哈希表的实现

线程安全：涉及到修改HashTable的方法，使用synchronized修饰

串行化的方式运行，性能较差

**note**

使用Collections.sychronizedMap可以将hashmap转化为线程安全的map，不过效率也很低

#### 解决hash碰撞

​     1.开放地址法：容易产生堆积问题；不适于大规模的数据存储；散列函数的设计对冲突会有很大的影响；插入时可能会出现多次冲突的现象，删除的元素是多个冲突元素中的一个，需要对后面的元素作处理，实现较复杂；结点规模很大时会浪费很多空间；

​     2.链地址法：处理冲突简单，且无堆积现象，平均查找长度短；链表中的结点是动态申请的，适合构造表不能确定长度的情况；相对而言，拉链法的指针域可以忽略不计，因此较开放地址法更加节省空间。插入结点应该在链首，删除结点比较方便，只需调整指针而不需要对其他冲突元素作调整。

> 开放地址法 
>
> 这个方法的基本思想是：当发生地址冲突时，按照某种方法继续探测哈希表中的其他存储单元，直到找到空位置为止。这个过程可用下式描述： 
> H i ( key ) = ( H ( key )+ d i ) mod m ( i = 1,2,…… ， k ( k ≤ m – 1)) 
> 其中： H ( key ) 为关键字 key 的直接哈希地址， m 为哈希表的长度， di 为每次再探测时的地址增量。 
> 采用这种方法时，首先计算出元素的直接哈希地址 H ( key ) ，如果该存储单元已被其他元素占用，则继续查看地址为 H ( key ) + d 2 的存储单元，如此重复直至找到某个存储单元为空时，将关键字为 key 的数据元素存放到该单元。 

### TreeMap

TreeMap 是一个有序的key-value集合，它是通过红黑树实现的。

~~~java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable
~~~

实现了NavigableMap接口，可以有一些HashMap不具有的方法

> **tailMap(K fromKey)** 方法用于返回此映射，其键大于或等于fromKey的部分视图。返回的映射受此映射支持，因此改变返回映射反映在此映射中，反之亦然。
>
> **headMap(K fromKey)** 与之相反
>
> **firstKey** 返回第一个key

### ConcurrentHashMap

1、并发控制与分段锁思想

2、1.8中的CAS自旋锁

3、红黑树的启用条件

#### 1.7

ConCurrentHashMap其实就是一个segment数组，而每一个segment又类似一个hashMap，因此ConCurrentHashMap可以说是一个二级hash表，在一个总的hash表下面，有若干个子hash表。

优势就是，采用了分段锁的技术，segment之间相互不影响。

每个segment中都有一把锁，相当于降低了锁的粒度，让并发效率更高。

##### get方法：

1、为输入的key做hash运算，得到hash值

2、通过hash值，定位到segment对象

3、再次通过hash值，定位到segment当中数组的具体位置。

##### put方法：

1、为输入的key做hash运算，得到hash值

2、通过hash值，定位到segment对象

3、获得可重入锁

4、再次通过hash值，定位到segment当中数组的具体位置。

5、插入数据

6、释放锁

##### size方法如何保证一致性

类似于乐观锁转悲观锁

先假定size过程中不会有修改，当尝试一定次数，才转为悲观锁，锁住所有segment保证一致性。

#### 1.8

使用CAS+synchronized使锁更细化

数据结构  数组+链表+红黑树

ConcurrentHashMap的JDK8与JDK7版本的并发实现相比，最大的区别在于JDK8的锁粒度更细，理想情况下talbe数组元素的大小就是其支持并发的最大个数，在JDK7里面最大并发个数就是Segment的个数，默认值是16

##### put方法的逻辑

1、判断node[]数组是否初始化，没有则进行初始化操作

2、通过hash定位数组的索引坐标，是否有node节点，如果没有则使用cas进行添加，添加失败则进入下一次循环

3、检测到内部正在扩容，就帮助它一块扩容

4、如果头节点不为空 f!=null，则使用synchronized锁住f元素（链表/红黑树的头元素）

5、判断链表长度是否达到8，做树化

##### get方法

1. 首先计算hash值，定位到该table索引位置，如果是首节点符合就返回
2. 如果遇到扩容的时候，会调用标志正在扩容节点ForwardingNode的find方法，查找该节点，匹配就返回
3. 以上都不符合的话，就往下遍历节点，匹配就返回，否则最后就返回null

**不需要加锁**

- get操作全程不需要加锁是因为Node的成员val是用volatile修饰的和数组用volatile修饰没有关系，数组用volatile其实就是为了使得Node数组在扩容的时候对其他线程具有可见性而加的
- 数组用volatile修饰主要是保证在数组扩容的时候保证可见性。

##### 比起1.7的优化：

比起segment ，锁拆得更细

1、首先使用无锁操作cas插入头节点，失败则循环重试

2、若头节点已存在，则尝试获取头节点的同步锁，再进行操作

## hashMap/table concurrentHashMap三者区别

1、hashMap线程不安全，数组+链表+红黑树

2、HashTable线程安全，锁住整个对象，数组+链表

3、concurrentHashMap线程安全，CAS+同步锁，数组+链表+红黑树

4、HashMap的key，value均可以为null，而其他的两个类不支持

## 遍历Map

```java
HashMap<String, String> map = new HashMap<>();
for (Map.Entry<String, String> entry : map.entrySet()) {
    entry.getKey();
    entry.getValue();
}
```
# JavaEE

## Tomcat

开源的 Java Web 应用服务器，实现了 Java EE(Java Platform Enterprise Edition)的部 分技术规范，比如 Java Servlet、Java Server Page、JSTL、Java WebSocket。Java EE 是 Sun 公 司为企业级应用推出的标准平台，定义了一系列用于企业级开发的技术规范，除了上述的之外，还有 EJB、Java Mail、JPA、JTA、JMS 等，而这些都依赖具体容器的实现。

### 组件

![](pic/爱奇艺20190820113117.png)

**Connector**：Tomcat 与外部世界的连接器，监听固定端口接收外部请求，传递给 Container，并 将 Container 处理的结果返回给外部；

**Container**：Catalina，Servlet 容器，内部有多层容器组成，用于管理 Servlet 生命周期，调用 servlet 相关方法。

**Server**：指的就是整个 Tomcat 服 务器，包含多组服务，负责管理和 启动各个 Service，同时监听 8005 端口发过来的 shutdown 命令，用 于关闭整个容器 ；

**Service**：Tomcat 封装的、对外提 供完整的、基于组件的 web 服务， 包含 Connectors、Container 两个 核心组件，以及多个功能组件，各 个 Service 之间是独立的，但是共享 同一 JVM 的资源 ；

**Loader**：封装了 Java ClassLoader，用于 Container 加载类文件； Realm：Tomcat 中为 web 应用程序提供访问认证和角色管理的机制；

**JMX**：Java SE 中定义技术规范，是一个为应用程序、设备、系统等植入管理功能的框架，通过 JMX 可以远程监控 Tomcat 的运行状态；

**Jasper**：Tomcat 的 Jsp 解析引擎，用于将 Jsp 转换成 Java 文件，并编译成 class 文件。 Session：负责管理和创建 session，以及 Session 的持久化(可自定义)，支持 session 的集
群。

**Pipeline**：在容器中充当管道的作用，管道中可以设置各种 valve(阀门)，请求和响应在经由管 道中各个阀门处理，提供了一种灵活可配置的处理请求和响应的机制。

**Naming**：命名服务，JNDI， Java 命名和目录接口，是一组在 Java 应用中访问命名和目录服务的 API。命名服务将名称和对象联系起来，使得我们可以用名称访问对象，目录服务也是一种命名 服务，对象不但有名称，还有属性。Tomcat 中可以使用 JNDI 定义数据源、配置信息，用于开发 与部署的分离。

### 生命周期

Tomcat 为了方便管理组件和容器的生命周期，定义了从创建、启动、到停止、销毁共 12 中状态，tomcat 生命周期管理了内部状态变化的规则控制，组件和容器只需实现相应的生命周期 方法即可完成各生命周期内的操作(initInternal、startInternal、stopInternal、 destroyInternal)；

![](pic/爱奇艺20190820113313.png)



### Tomcat的启动过程

1、初始化守护线程，其实就是初始化类加载器

先是 bootstrapClassLoader，CommonClassLoader，ServerClassLoader，SharedClassLoader

2、加载相关的配置文件，初始化几个主要的顶层接口实例，就是服务器的初始化

初始化一些重要的接口，server，engine，host，context，wrapper，servlet等

3、启动那些有生命周期的顶层实例，就是启动服务器

对ContainerBase容器初始化

### jsp九大内置对象和其作用详解

1、**request**

2、**response**

3、**session**

4、**application**

application对象可将信息保存在服务器中，直到服务器关闭。

类似于“全局变量”

5、**out**

6、**pageContext**

7、**config**

8、**page**

代表jsp本身

9、**exception**

### servlet生命周期

##### 1、加载和实例化

当servlet容器启动或客户端发送一个请求时，servlet容器会查找内存中是否存在该servlet实例，若存在，则直接读取该实例相应请求，如果不存在，就创建一个servlet实例

##### 2、初始化

实例化后，servlet容器将调用servlet的init方法进行初始化

##### 3、服务

初始化后，servlet处于能相应请求的就绪状态。当接收到客户端请求时，调用service方法处理客户端请求，HttpServlet的service方法会根据不同的请求，转调不同的doXXX的方法

##### 4、销毁

当servlet容器关闭时，servlet实例也会销毁，期间，servlet容器会调用servlet的destroy方法去判断servlet是否应当被释放。