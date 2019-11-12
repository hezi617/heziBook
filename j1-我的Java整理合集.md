感谢 swiftieslizheng 的分享！     *[点击进入他的GitHub！](http://www.lizheng0928.top/)*

### Java基础篇

#### Java基础知识

##### 0x01 Java集合

+ 数组是定长的，集合是变长的

+ 数组可以是基本数据类型也可以是类，集合只能是类，对于基本数据类型需要转为包装类

+ Java的集合类主要由两个接口派生而出：Collection和Map,Collection和Map是Java集合框架的根接口


+ ArrayList，HashSet，LinkedList，TreeSet，HashMap，TreeMap是我们经常会有用到的已实现的集合类
+ Iterator接口经常被称作迭代器，它是Collection接口的父接口。但Iterator主要用于遍历集合中的元素。 Iterator接口中主要定义了2个方法**hasNext()**和**next()**，当使用Iterator对集合元素进行迭代时，把集合元素的值传给了迭代变量（***就如同参数传递是值传递，基本数据类型传递的是值，引用类型传递的仅仅是对象的引用变量***）。
+ 值传递与引用传递？

>1. 基本数据类型传值，对形参的修改不会影响实参；
>
>2. 引用类型传引用，形参和实参指向同一个内存地址，所以对参数的修改会影响到实际的对象。
>
>3. String, Integer, Double等immutable的类型特殊处理，可以理解为传值，最后的操作不会修改实参。

##### Java 为什么要有包装类
+ 我们都知道在Java语言中，new一个对象存储在堆里，我们通过栈中的引用来使用这些对象；但是对于经常用到的一系列类型如int，如果我们用new将其存储在堆里就不是很有效——特别是简单的小的变量。所以就出现了基本类型，同C++一样，Java采用了相似的做法，对于这些类型不是用new关键字来创建，而是直接将变量的值存储在栈中，因此更加高效。

+ Java是一个面向对象的编程语言，基本类型并不具有对象的性质，为了让基本类型也具有对象的特征，就出现了包装类型（如我们在使用集合类型Collection时就一定要使用包装类型而非基本类型），它相当于将基本类型“包装起来”，使得它具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。

1. 声明方式不同：
基本类型不使用new关键字，而包装类型需要使用new关键字来在堆中分配存储空间；

2. 存储方式及位置不同：
基本类型是直接将变量值存储在栈中，而包装类型是将对象放在堆中，然后通过引用来使用；

3. 初始值不同：
基本类型的初始值如int为0，boolean为false，而包装类型的初始值为null；

4. 使用方式不同：
基本类型直接赋值直接使用就好，而包装类型在集合如Collection、Map时会使用到。

[https://blog.csdn.net/min996358312/article/details/62894674](https://blog.csdn.net/min996358312/article/details/62894674)


##### 0x02 ArrayList

+ 以数组实现。节约空间，但数组有容量限制。超出限制时会增加50%容量，用System.arraycopy()复制到新的数组，因此最好能给出数组大小的预估值。默认第一次插入元素时创建大小为10的数组。按数组下标访问元素—get(i)/set(i,e) 的性能很高，这是数组的基本优势。直接在数组末尾加入元素—add(e)的性能也高，但如果按下标插入、删除元素—add(i,e), remove(i), remove(e)，则要用System.arraycopy()来移动部分受影响的元素，性能就变差了，这是基本劣势。

+ 当增加数据的时候，如果ArrayList的大小已经不满足需求时，那么就将数组变为原长度的1.5倍，之后的操作就是把老的数组拷到新的数组里面。例如，默认的数组大小是10，也就是说当我们`add`10个元素之后，再进行一次add时，就会发生自动扩容，数组长度由10变为了15

##### 多线程高并发情况下ArrayList集合不安全问题
[https://www.jianshu.com/p/c34c39590620](https://www.jianshu.com/p/c34c39590620)

##### 0x03 LinkedList

+ 以双向链表实现。链表无容量限制，但双向链表本身使用了更多空间，也需要额外的链表指针操作。按下标访问元素—get(i)/set(i,e) 要悲剧的遍历链表将指针移动到位(如果i>数组大小的一半，会从末尾移起)。插入、删除元素时修改前后节点的指针即可，但还是要遍历部分链表的指针才能移动到下标所指的位置，只有在链表两头的操作—add()，addFirst()，removeLast()或用iterator()上的remove()能省掉指针的移动。

+ 如果在前半区间就从head搜索，而在后半区间就从tail搜索。而不是一直从头到尾的搜索。如此设计，将节点访问的复杂度由O(n)变为O(n/2)

##### 0x04 HashMap

+ 基于Map接口实现、**允许null键/值**、**非同步**、不保证有序(比如插入的顺序)、也不保证序不随时间变化。

+ 在HashMap中有两个很重要的参数，容量(Capacity)和负载因子(Load factor)

+ Capacity就是bucket的大小，Load factor就是bucket填满程度的最大比例。如果对迭代性能要求很高的话，不要把`capacity`设置过大，也不要把`load factor`设置过小。当bucket中的entries的数目大于`capacity*load factor`时就需要调整bucket的大小为当前的**2**倍

+ put函数大致的思路为：

  1. 对key的hashCode()做hash，然后再计算index;
  2. 如果没碰撞直接放到bucket里；
  3. 如果碰撞了，以链表的形式存在buckets后；
  4. 如果碰撞导致链表过长(大于等于`TREEIFY_THRESHOLD`)，就把链表转换成红黑树；
  5. 如果节点已经存在就替换old value(保证key的唯一性)
  6. 如果bucket满了(超过`load factor*current capacity`)，就要resize。

+ get函数大致思路如下：

  1. bucket里的第一个节点，直接命中；

  2. 如果有冲突，则通过key.equals(k)去查找对应的entry

     若为树，则在树中通过key.equals(k)查找，O(logn)；

     若为链表，则在链表中通过key.equals(k)查找，O(n)。

+ 在get和put的过程中，计算下标时，先对hashCode进行hash操作，然后再通过hash值进一步计算下标，如下图所示：


在n - 1为15(0x1111)时，其实散列真正生效的只是低4bit的有效位，当然容易碰撞了。因此，设计者想了一个顾全大局的方法(综合考虑了速度、作用、质量)，就是把高16bit和低16bit异或了一下。设计者还解释到因为现在大多数的hashCode的分布已经很不错了，就算是发生了碰撞也用`O(logn)`的tree去做了。仅仅异或一下，既减少了系统的开销，也不会造成的因为高位没有参与下标的计算(table长度比较小时)，从而引起的碰撞。

+ 在get和put的过程中，计算下标时，先对hashCode进行hash操作，然后再通过hash值进一步计算下标，在对hashCode()计算hash时具体实现是这样的：

```java
//高16bit不变，低16bit和高16bit做了一个异或
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

+ 当put时，如果发现目前的bucket占用程度已经超过了Load Factor所希望的比例，那么就会发生resize。在resize的过程，简单的说就是把bucket扩充为2倍，之后重新计算index，把节点再放到新的bucket中。
  
+ **resize之后元素的位置要么是在原位置，要么是在原位置再移动2次幂的位置**：resize的时候由于长度变为原来的2的n次幂，所以可以查看Entry的第5位，如果是0则不需要移动，如果是1则位置变为原索引+oldCap
  
+ **1. 什么时候会使用HashMap？他有什么特点？**
  是基于Map接口的实现，存储键值对时，它可以接收null的键值，是非同步的，HashMap存储着**Entry(hash, key, value, next)**对象。

  **2. 你知道HashMap的工作原理吗？**
  通过hash的方法，通过put和get存储和获取对象。存储对象时，我们将K/V传给put方法时，它调用hashCode计算hash从而得到bucket【由(n-1)&hash得到】位置，进一步存储，HashMap会根据当前bucket的占用情况自动调整容量(超过`Load Facotr`则resize为原来的2倍)。获取对象时，我们将K传给get，它调用hashCode计算hash从而得到bucket位置，并进一步调用equals()方法确定键值对。如果发生碰撞的时候，Hashmap通过链表将产生碰撞冲突的元素组织起来，在Java 8中，如果一个bucket中碰撞冲突的元素超过某个限制(默认是8)，则使用红黑树来替换链表，从而提高速度。

  **3. 你知道get和put的原理吗？equals()和hashCode()的都有什么作用？**
  通过对key的hashCode()进行hashing，并计算下标( `(n-1) & hash`)，从而获得buckets的位置。如果产生碰撞，则利用key.equals()方法去链表或树中去查找对应的节点。

  **4. 你知道hash的实现吗？为什么要这样实现？**
  在Java 1.8的实现中，是通过hashCode()的高16位异或低16位实现的：`(h = k.hashCode()) ^ (h >>> 16)`，主要是从速度、功效、质量来考虑的，这么做可以在bucket的n比较小的时候，也能保证考虑到高低bit都参与到hash的计算中，同时不会有太大的开销。

  **5. 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？**
  如果超过了负载因子(默认**0.75**)，则会重新resize一个原来长度两倍的HashMap，并且重新调用hash方法。
  
  **6.为什么HashMap是线程不安全的？**
  
  在多个线程同时操作HashMap并且同时到达临界条件进行扩容的时候，可能由于A线程已经完成了扩容，并且改变了其中Entry的指向，B线程执行中途被挂起，在A线程完成之后由于Entry的指针已经改变了指向，造成扩容HashMap某个bucket中链表成环，如果get命中了这个bucket的话，会导致死循环。

##### 0x05 LinkedHashMap

+ LinkedHashMap是Hash表和双向链表的实现，依靠双向链表保证了迭代顺序是插入的顺序。
+ **保证双向链表中的节点次序或者双向链表容量**，目的就是保持双向链表中节点的顺序要从eldest到youngest

##### 0x06 TreeMap

+ 如果我们希望Map可以**保持key的大小顺序**的时候，我们就需要利用TreeMap了
+ put：如果存在的话，old value被替换；如果不存在的话，则新添一个节点，然后对做红黑树的平衡操作
+ get：以log(n)的复杂度进行get
+ TreeMap是如何保证其迭代输出是有序的呢？其实从宏观上来讲，就相当于树的中序遍历(LDR)

##### 0x07 Java泛型

+ **泛型的目的：** Java 泛型就是把一种语法糖，通过泛型使得在编译阶段完成一些类型转换的工作，避免在运行时强制类型转换而出现`ClassCastException`，即类型转换异常。

+ 泛型的好处

>①类型安全。类型错误现在在编译期间就被捕获到了，而不是在运行时当作java.lang.ClassCastException展示出来，将类型检查从运行时挪到编译时有助于开发者更容易找到错误，并提高程序的可靠性。
>
>②消除了代码中许多的强制类型转换，增强了代码的可读性。
>
>③为较大的优化带来了可能。

+ 泛型的实质：允许在定义接口、类时声明类型形参，类型形参在整个接口、类体内可当成类型使用，几乎所有可使用普通类型的地方都可以使用这种类型形参
+ 泛型类派生类：**使用这些接口、父类派生子类时不能再包含类型形参，需要传入具体的类型。**

```
public class A extends Container<K, V>{}//错误
public class A extends Container<Integer, String>{}//正确
public class A extends Container{}//当成Object处理
```

+ 泛型构造器

```
public <T> Person(T t)//定义
new Person(22);//隐式
new<String> Person("hello");//显式
```

+ **在静态方法、静态初始化块或者静态变量的声明和初始化中不允许使用类型形参。由于系统中并不会真正生成泛型类，所以instanceof运算符后不能使用泛型类。**
+ 类型通配符(?)：可以读取出Object类，不能插入
+ 上限通配符：如果想限制使用泛型类别时，只能用某个特定类型或者是其子类型才能实例化该类型时，可以在定义类型时，**使用extends关键字指定这个类型必须是继承某个类，或者实现某个接口，也可以是这个类或接口本身。**
+ 下限通配符：如果想限制使用泛型类别时，只能用某个特定类型或者是其父类型才能实例化该类型时，可以在定义类型时，**使用super关键字指定这个类型必须是是某个类的父类，或者是某个接口的父接口，也可以是这个类或接口本身。**

+ 不管为泛型的类型形参传入哪一种类型实参，对于Java来说，它们依然被当成同一类处理，在内存中也只占用一块内存空间。从Java泛型这一概念提出的目的来看，其只是作用于代码编译阶段，在编译过程中，对于正确检验泛型结果后，会将泛型的相关信息擦出，也就是说，成功编译过后的class文件中是不包含任何泛型信息的。泛型信息不会进入到运行时阶段。

##### 0x08 Java反射

+ Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类中的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。
+ 在Java程序中获得Class对象通常有如下三种方式：

```java
//第一种方式 通过Class类的静态方法——forName()来实现
class1 = Class.forName("com.lvr.reflection.Person");
//第二种方式 通过类的class属性
class1 = Person.class;
//第三种方式 通过对象getClass方法
Person person = new Person();
Class<?> class1 = person.getClass();
```

+ **生成类的实例对象**

  1.使用Class对象的newInstance()方法来创建该Class对象对应类的实例。这种方式要求该Class对象的对应类有默认构造器，而执行newInstance()方法时实际上是利用默认构造器来创建该类的实例。

  2.先使用Class对象获取指定的Constructor对象，再调用Constructor对象的newInstance()方法来创建该Class对象对应类的实例。通过这种方式可以选择使用指定的构造器来创建实例。

  ```java
  //第一种方式 Class对象调用newInstance()方法生成
  Object obj = class1.newInstance();
  //第二种方式 对象获得对应的Constructor对象，再通过该Constructor对象的newInstance()方法生成
  Constructor<?> constructor = class1.getDeclaredConstructor(String.class);//获取指定声明构造函数
  obj = constructor.newInstance("hello");
  ```

+ **调用类的方法**

  1.通过Class对象的getMethods()方法或者getMethod()方法获得指定方法，返回Method数组或对象。

  2.调用Method对象中的`Object invoke(Object obj, Object... args)`方法。第一个参数对应调用该方法的实例对象，第二个参数对应该方法的参数。

  ```java
   // 生成新的对象：用newInstance()方法
   Object obj = class1.newInstance();
  //首先需要获得与该方法对应的Method对象
  Method method = class1.getDeclaredMethod("setAge", int.class);
  //调用指定的函数并传递参数
  method.invoke(obj, 28);
  ```

**当通过Method的invoke()方法来调用对应的方法时，Java会要求程序必须有调用该方法的权限。如果程序确实需要调用某个对象的private方法，则可以先调用Method对象的如下方法。**
**setAccessible(boolean flag)：将Method对象的acessible设置为指定的布尔值。值为true，指示该Method在使用时应该取消Java语言的访问权限检查；值为false，则知识该Method在使用时要实施Java语言的访问权限检查。**

+ **访问成员变量值**

  1.通过Class对象的getFields()方法或者getField()方法获得指定方法，返回Field数组或对象。

  2.Field提供了两组方法来读取或设置成员变量的值：
  getXXX(Object obj):获取obj对象的该成员变量的值。此处的XXX对应8种基本类型。如果该成员变量的类型是引用类型，则取消get后面的XXX。
  setXXX(Object obj,XXX val)：将obj对象的该成员变量设置成val值。

  ```java
  //生成新的对象：用newInstance()方法 
  Object obj = class1.newInstance();
  //获取age成员变量
  Field field = class1.getField("age");
  //将obj对象的age的值设置为10
  field.setInt(obj, 10);
  //获取obj对象的age的值
  field.getInt(obj);
  ```

+ 代理模式的参与者：

  **主题接口：**Subject 是委托对象和代理对象都共同实现的接口，即代理类的所实现的行为接口。Request() 是委托对象和代理对象共同拥有的方法。
  **目标对象：**ReaSubject 是原对象，也就是被代理的对象。
  **代理对象：**Proxy 是代理对象，用来封装真是主题类的代理类。
  **客户端 ：**使用代理类和主题接口完成一些工作。

+ **静态代理：**代理类是在编译时就实现好的。也就是说 Java 编译完成后代理类是一个实际的 class 文件。
  **动态代理：**代理类是在运行时生成的。也就是说 Java 编译完之后并没有实际的 class 文件，而是在运行时动态生成的类字节码，并加载到JVM中。代理类的字节码将在运行时生成并载入当前代理的 ClassLoader。

+ 动态代理主要类

**java.lang.reflect.Proxy:**这是生成代理类的主类，通过 Proxy 类生成的代理类都继承了 Proxy 类。Proxy提供了用户创建动态代理类和代理对象的静态方法，它是所有动态代理类的父类。

**java.lang.reflect.InvocationHandler:**这里称他为"调用处理器"，它是一个接口。当调用动态代理类中的方法时，将会直接转接到执行自定义的InvocationHandler中的invoke()方法。即我们动态生成的代理类需要完成的具体内容需要自己定义一个类，而这个类必须实现 InvocationHandler 接口，通过重写invoke()方法来执行具体内容。

Proxy 类还有一些静态方法，比如：

`InvocationHandler getInvocationHandler(Object proxy):`获得代理对象对应的调用处理器对象。

`Class getProxyClass(ClassLoader loader, Class[] interfaces):`获得代理类。

InvocationHandler 接口中有方法：

`invoke(Object proxy, Method method, Object[] args)`
**这个函数是在代理对象调用任何一个方法时都会调用的，方法不同会导致第二个参数method不同，第一个参数是代理对象（表示哪个代理对象调用了method方法），第二个参数是 Method 对象（表示哪个方法被调用了），第三个参数是指定调用方法的参数。**

+ **newProxyInstance这个方法实际上做了两件事：第一，创建了一个新的类【代理类】，这个类实现了Class[] interfaces中的所有接口，并通过你指定的ClassLoader将生成的类的字节码加载到JVM中，创建Class对象；第二，以你传入的InvocationHandler作为参数创建一个代理类的实例并返回。**

```java
//创建一个InvocationHandler对象
InvocationHandler handler = new MyInvocationHandler(.args..);
//使用Proxy直接生成一个动态代理对象
RealSubject real = Proxy.newProxyInstance(RealSubject.class.getClassLoader(),
                                         RealSubject.class.getInterfaces(), handler);
```

```java
public class DynamicProxyDemo {
    public static void main(String[] args) {
        //1.创建目标对象
        RealSubject realSubject = new RealSubject();    
        //2.创建调用处理器对象
        ProxyHandler handler = new ProxyHandler(realSubject);    
        //3.动态生成代理对象
        Subject proxySubject = (Subject)Proxy.newProxyInstance(
            RealSubject.class.getClassLoader(),RealSubject.class.getInterfaces(), handler); 
        //4.通过代理对象调用方法
        proxySubject.request();    
    }
}

/**
 * 主题接口
 */
interface Subject{
    void request();
}

/**
 * 目标对象类
 */
class RealSubject implements Subject{
    public void request(){
        System.out.println("====RealSubject Request====");
    }
}
/**
 * 代理类的调用处理器
 */
class ProxyHandler implements InvocationHandler{
    private Subject subject;
    public ProxyHandler(Subject subject){
        this.subject = subject;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        //定义预处理的工作，当然你也可以根据 method 的不同进行不同的预处理工作
        System.out.println("====before====");
       //调用RealSubject中的方法
        Object result = method.invoke(subject, args);
        System.out.println("====after====");
        return result;
    }
}
```

+ 在下面程序的getInstance()方法中传入一个Class<T>参数，这是一个泛型化的Class对象，调用该Class对象的newInstance()方法将返回一个T对象。

```java
public class ObjectFactory {
    public static <T> T getInstance(Class<T> cls) {
        try {
            // 返回使用该Class对象创建的实例
            return cls.newInstance();
        } catch (InstantiationException | IllegalAccessException e) {
            e.printStackTrace();
            return null;
        }
    }
}

String instance = ObjectFactory.getInstance(String.class);

// 获取 Field 对象 f 的类型
Class<?> a = f.getType();
// 获得 Field 实例的泛型类型
Type type = f.getGenericType();
```

+ **getRawType()：**返回没有泛型信息的原始类型。

  **getActualTypeArguments()：**返回泛型参数的类型。

##### 0x09 Java-IO

+ Java采用unicode编码，2个字节来表示一个字符

+ Java中的String类是按照unicode进行编码的，当使用String(byte[] bytes, String encoding)构造字符串时，encoding所指的是bytes中的数据是按照那种方式编码的，而不是最后产生的String是什么编码方式，换句话说，是让系统把bytes中的数据由encoding编码方式转换成unicode编码

+ `getBytes(String charsetName)`使用指定的编码方式将此String编码为 byte 序列，并将结果存储到一个新的 byte 数组中

+ 在 GB 2312 编码或 GBK 编码中，一个英文字母字符存储需要1个字节，一个汉字字符存储需要2个字节。 在UTF-8编码中，一个英文字母字符存储需要1个字节，一个汉字字符储存需要3到4个字节。在UTF-16编码中，一个英文字母字符存储需要2个字节，一个汉字字符储存需要3到4个字节

+ 流是一组有顺序的，有起点和终点的字节集合，是对数据传输的总称或抽象。即数据在两设备间的传输称为流，流的本质是数据传输，根据数据传输特性将流抽象为各种类，方便更直观的进行数据操作。**流有输入和输出，输入时是流从数据源流向程序。输出时是流从程序传向数据源，而数据源可以是内存，文件，网络或程序等。**

+ 字节流和字符流和用法几乎完全一样，区别在于字节流和字符流所操作的数据单元不同。
  字符流的由来： 因为数据编码的不同，而有了对字符进行高效操作的流对象。本质其实就是基于字节流读取时，去查了指定的码表。字节流和字符流的区别：
  （1）读写单位不同：字节流以字节（8bit）为单位，字符流以字符为单位，根据码表映射字符，一次可能读多个字节。
  （2）处理对象不同：字节流能处理所有类型的数据（如图片、avi等），而字符流只能处理字符类型的数据。

  只要是处理纯文本数据，就优先考虑使用字符流。 除此之外都使用字节流。

+ 按照流的角色来分，可以分为节点流和处理流。
  可以从/向一个特定的IO设备（如磁盘、网络）读/写数据的流，称为节点流，节点流也被成为低级流。
  处理流是对一个已存在的流进行连接或封装，通过封装后的流来实现数据读/写功能，处理流也被称为高级流。

  当使用处理流进行输入/输出时，程序并不会直接连接到实际的数据源。使用处理流的一个明显好处是，只要使用相同的处理流，程序就可以采用完全相同的输入/输出代码来访问不同的数据源。

  ```java
  //节点流，直接传入的参数是IO设备
  FileInputStream fis = new FileInputStream("test.txt");
  //处理流，直接传入的参数是流对象
  BufferedInputStream bis = new BufferedInputStream(fis);
  ```

+ **在执行完流操作后，要调用**`close()`**方法来关系输入流，因为程序里打开的IO资源不属于内存资源，垃圾回收机制无法回收该资源，所以应该显式关闭文件IO资源。**
+ **使用Java的IO流执行输出时，不要忘记关闭输出流，关闭输出流除了可以保证流的物理资源被回收之外，还能将输出流缓冲区的数据flush到物理节点里（因为在执行close()方法之前，自动执行输出流的flush()方法）**

##### 0x10 Java-NIO

+ 标准的IO基于字节流和字符流进行操作的，而NIO是基于通道(Channel)和缓冲区(Buffer)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入通道也类似。
+ **NIO是一种新型的IO，但NIO不仅仅就是等于Non-blocking IO（非阻塞IO），NIO中有实现非阻塞IO的具体类，但不代表NIO就是Non-blocking IO（非阻塞IO）。**
+ Java NIO 由以下几个核心部分组成：Buffer、Channel、Selector
+ 传统的IO操作面向数据流，意味着每次从流中读一个或多个字节，直至完成，数据没有被缓存在任何地方。NIO操作面向缓冲区，数据从Channel读取到Buffer缓冲区，随后在Buffer中处理数据。
+ buffer缓冲区实质上就是一块内存，用于写入数据，也供后续再次读取数据。这块内存被NIO Buffer管理，并提供一系列的方法用于更简单的操作这块内存。
+ 一个Buffer有三个属性是必须掌握的，分别是：
  - capacity容量
  - position位置
  - limit限制
+ ![1566210660073](.\image\1566210660073.png)

+ **容量（Capacity）**

  作为一块内存，buffer有一个固定的大小，叫做capacity容量。也就是最多只能写入容量值得字节，整形等数据。一旦buffer写满了就需要清空已读数据以便下次继续写入新的数据。

  **位置（Position）**

  当写入数据到Buffer的时候需要中一个确定的位置开始，默认初始化时这个位置position为0，一旦写入了数据比如一个字节，整形数据，那么position的值就会指向数据之后的一个单元，position最大可以到capacity-1。
  当从Buffer读取数据时，也需要从一个确定的位置开始。buffer从写入模式变为读取模式时，position会归零，每次读取后，position向后移动。

  **上限（Limit）**
  在写模式，limit的含义是我们所能写入的最大数据量。它等同于buffer的容量。
  一旦切换到读模式，limit则代表我们所能读取的最大数据量，他的值等同于写模式下position的位置。
  数据读取的上限时buffer中已有的数据，也就是limit的位置（原position所指的位置）。

+ [[Java\][IO]JAVA NIO之浅谈内存映射文件原理与DirectMemory](http://blog.csdn.net/szwangdf/article/details/10588489)，[深入浅出MappedByteBuffer](http://www.jianshu.com/p/f90866dcbffc)。

+ Java NIO Channel通道和流非常相似，主要有以下几点区别：

  - 通道可以读也可以写，流一般来说是单向的（只能读或者写）。
  - 通道可以异步读写。
  - 通道总是基于缓冲区Buffer来读写。

+ 一个network IO (这里我们以read举例)，它会涉及到两个系统对象，一个是调用这个IO的process (or thread)，另一个就是系统内核(kernel)

+ 当一个read操作发生时，它会经历两个阶段：
  **1 等待数据准备 (Waiting for the data to be ready)**
  **2 将数据从内核拷贝到进程中 (Copying the data from the kernel to the process)**

+ **blocking IO**

![1566211434634](.\image\1566211434634.png)

当用户进程调用了recvfrom这个系统调用，kernel就开始了IO的第一个阶段：准备数据。对于network io来说，很多时候数据在一开始还没有到达（比如，还没有收到一个完整的UDP包），这个时候kernel就要等待足够的数据到来。而在用户进程这边，整个进程会被阻塞。当kernel一直等到数据准备好了，它就会将数据从kernel中拷贝到用户内存，然后kernel返回结果，用户进程才解除block的状态，重新运行起来。**所以，blocking IO的特点就是在IO执行的两个阶段都被block了。**

+ **non-blocking IO**

![1566211490361](.\image\1566211490361.png)

从图中可以看出，当用户进程发出read操作时，如果kernel中的数据还没有准备好，那么它并不会block用户进程，而是立刻返回一个error。从用户进程角度讲 ，它发起一个read操作后，并不需要等待，而是马上就得到了一个结果。用户进程判断结果是一个error时，它就知道数据还没有准备好，于是它可以再次发送read操作。**一旦kernel中的数据准备好了，并且又再次收到了用户进程的system call，那么它马上就将数据拷贝到了用户内存，然后返回。所以，用户进程其实是需要不断的主动询问kernel数据好了没有。**

+ **IO multiplexing**

select/epoll的好处就在于单个process就可以同时处理多个网络连接的IO。它的基本原理就是select/epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。

![1566211580786](.\image\1566211580786.png)

当用户进程调用了select，那么整个进程会被block，而同时，kernel会“监视”所有select负责的socket，当任何一个socket中的数据准备好了，select就会返回。这个时候用户进程再调用read操作，将数据从kernel拷贝到用户进程。

这个图和blocking IO的图其实并没有太大的不同，事实上，还更差一些。因为这里需要使用两个system call (select 和 recvfrom)，而blocking IO只调用了一个system call (recvfrom)。但是，用select的优势在于它可以同时处理多个connection。

在IO multiplexing Model中，**实际中，对于每一个socket，一般都设置成为non-blocking，**但是，如上图所示，整个用户的process其实是一直被block的。**只不过process是被select这个函数block，而不是被socket IO给block。**

+ **Asynchronous I/O**

![1566211793457](.\image\1566211793457.png)

**用户进程发起read操作之后，立刻就可以开始去做其它的事。** 而另一方面，从kernel的角度，当它受到一个asynchronous read之后，首先它会立刻返回，所以不会对用户进程产生任何block。**然后，kernel会等待数据准备完成，然后将数据拷贝到用户内存，当这一切都完成之后，kernel会给用户进程发送一个signal，告诉它read操作完成了。**

+ 有人可能会说，non-blocking IO并没有被block啊。这里有个非常“狡猾”的地方，定义中所指的”IO operation”是指真实的IO操作，就是例子中的recvfrom这个system call。non-blocking IO在执行recvfrom这个system call的时候，如果kernel的数据没有准备好，这时候不会block进程。但是，当kernel中数据准备好的时候，recvfrom会将数据从kernel拷贝到用户内存中，这个时候进程是被block了，在这段时间内，进程是被block的【**数据拷贝是个block动作**】。而asynchronous IO则不一样，当进程发起IO 操作之后，就直接返回再也不理睬了，直到kernel发送一个信号，告诉进程说IO完成。

  在这整个过程中，进程完全没有被block。经过上面的介绍，会发现non-blocking IO和asynchronous IO的区别还是很明显的。在non-blocking IO中，虽然进程大部分时间都不会被block，但是它仍然要求进程去主动的check，并且当数据准备完成以后，也需要进程主动的再次调用recvfrom来将数据拷贝到用户内存。而asynchronous IO则完全不同。它就像是用户进程将整个IO操作交给了他人（kernel）完成，然后他人做完后发信号通知。在此期间，用户进程不需要去检查IO操作的状态，也不需要主动的去拷贝数据。

+ 首先，标准的IO显然属于blocking IO。

##### 0x11 Java易混淆知识点

+ **注意：Annotation能被用来为程序元素（类、方法、成员变量等）设置元素据。Annotaion不影响程序代码的执行，无论增加、删除Annotation，代码都始终如一地执行。如果希望让程序中的Annotation起一定的作用，只有通过解析工具或编译工具对Annotation中的信息进行解析和处理。**
+ java异常

![1566207762092](.\image\1566207762092.png)

+ 编译器不会检查RuntimeException异常
+ Java将可抛出(Throwable)的结构分为三种类型：**被检查的异常(Checked Exception)，运行时异常(RuntimeException)和错误(Error)。**
+ 被检查的异常：Java编译器会检查它。此类异常，要么通过throws进行声明抛出，要么通过try-catch进行捕获处理，否则不能通过编译
+ 在abstract class方式中，Demo可以有自己的数据成员，也可以有非abstarct的成员方法，而在interface方式的实现中，Demo只能够有static final的数据成员，所有的成员方法都是abstract的
+ 抽象类和接口都不能直接实例化，如果要实例化，抽象类变量必须指向实现所有抽象方法的子类对象，接口变量必须指向实现所有接口方法的类对象。
+ 抽象类要被子类继承，接口要被类实现。
+ 接口里定义的变量只能是公共的静态的常量，抽象类中的变量是普通变量。
+ 抽象类里可以没有抽象方法。
+ 接口可以被类多实现（被其他接口多继承），抽象类只能被单继承。
+ 接口中没有 `this` 指针，没有构造函数，不能拥有实例字段（实例变量）或实例方法。
+ 抽象类不能在Java 8 的 lambda 表达式中使用。
+ Java中有三种类型的对象拷贝：浅拷贝(Shallow Copy)、深拷贝(Deep Copy)、延迟拷贝(Lazy Copy)
+ 浅拷贝是按位拷贝对象，它会创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值；如果属性是内存地址（引用类型），拷贝的就是内存地址

![1566208646481](.\image\1566208646481.png)

+ 如何实现浅拷贝，super.clone()
+ 深拷贝会拷贝所有的属性,并拷贝属性指向的动态分配的内存。当对象和它所引用的对象一起拷贝时即发生深拷贝。深拷贝相比于浅拷贝速度较慢并且花销较大。

![1566208706434](.\image\1566208706434.png)

+ 如何实现深拷贝，new对象出来并进行赋值
+ 延迟拷贝：延迟拷贝是浅拷贝和深拷贝的一个组合，实际上很少会使用。 当最开始拷贝一个对象时，会使用速度较快的浅拷贝，还会使用一个计数器来记录有多少对象共享这个数据。当程序想要修改原始的对象时，它会决定数据是否被共享（通过检查计数器）并根据需要进行深拷贝。
+ java 的transient关键字为我们提供了便利，你只需要实现Serilizable接口，将不需要序列化的属性前添加关键字transient，序列化对象的时候，这个属性就不会序列化到指定的目的地中
+ **finally语句是在try的return语句执行之后，return返回之前执行**
+ 如果try中return的是值传递的变量（基本数据类型+基本数据类型包装类），finally的return不会影响try的值
+ 如果try中return的是引用传递的变量，finally的return会影响try的return值



#### Java并发编程

##### 0x01 线程的生成方式对比

+ **采用实现Runnable、Callable接口的方式创见多线程时，优势是：**线程类只是实现了Runnable接口或Callable接口，还可以继承其他类。在这种方式下，多个线程可以共享同一个target对象，所以非常适合多个相同线程来处理同一份资源的情况，从而可以将CPU、代码和数据分开，形成清晰的模型，较好地体现了面向对象的思想。**劣势是：**编程稍微复杂，如果要访问当前线程，则必须使用Thread.currentThread()方法。

  **使用继承Thread类的方式创建多线程时优势是：**编写简单，如果需要访问当前线程，则无需使用Thread.currentThread()方法，直接使用this即可获得当前线程。**劣势是：**线程类已经继承了Thread类，所以不能再继承其他父类。

##### 0x02 线程池

+ 线程池中保存等待执行的任务的阻塞队列。通过线程池中的execute方法提交的Runable对象都会存储在该队列中。我们可以选择下面几个阻塞队列。

  | 阻塞队列              | 说明                                                         |
  | :-------------------- | :----------------------------------------------------------- |
  | ArrayBlockingQueue    | 基于数组实现的有界的阻塞队列,该队列按照FIFO（先进先出）原则对队列中的元素进行排序。 |
  | LinkedBlockingQueue   | 基于链表实现的阻塞队列，该队列按照FIFO（先进先出）原则对队列中的元素进行排序。 |
  | SynchronousQueue      | 内部没有任何容量的阻塞队列。在它内部没有任何的缓存空间。对于SynchronousQueue中的数据元素只有当我们试着取走的时候才可能存在。 |
  | PriorityBlockingQueue | 具有优先级的无限阻塞队列。                                   |

+ 下面是在ThreadPoolExecutor中提供**handler**的四个可选值。

  | 可选值              | 说明                                       |
| :------------------ | :----------------------------------------- |
  | CallerRunsPolicy    | 只用调用者所在线程来运行任务。             |
| AbortPolicy         | 直接抛出RejectedExecutionException异常。   |
  | DiscardPolicy       | 丢弃掉该任务，不进行处理。                 |
  | DiscardOldestPolicy | 丢弃队列里最近的一个任务，并执行当前任务。 |
  
+  **execute** 当我们使用execute来提交任务时，由于execute方法没有返回值，所以说我们也就无法判定任务是否被线程池执行成功。

+ 当我们使用**submit**来提交任务时,它会返回一个future,我们就可以通过这个future来判断任务是否执行成功，还可以通过future的get方法来获取返回值。如果子线程任务没有完成，get方法会阻塞住直到任务完成，而使用get(long timeout, TimeUnit unit)方法则会阻塞一段时间后立即返回，这时候有可能任务并没有执行完。

+ 调用线程池的`shutdown()`或`shutdownNow()`方法来关闭线程池

  shutdown原理：将线程池状态设置成SHUTDOWN状态，然后中断所有没有正在执行任务的线程。

  shutdownNow原理：将线程池的状态设置成STOP状态，然后中断所有任务(包括正在执行的)的线程，并返回等待执行任务的列表。

  **中断采用interrupt方法，所以无法响应中断的任务可能永远无法终止。** 但调用上述的两个关闭之一，isShutdown()方法返回值为true，当所有任务都已关闭，表示线程池关闭完成，则isTerminated()方法返回值为true。当需要立刻中断所有的线程，不一定需要执行完任务，可直接调用shutdownNow()方法。

+ Java中四种具有不同功能常见的线程池。他们都是直接或者间接配置ThreadPoolExecutor来实现他们各自的功能。这四种线程池分别是newFixedThreadPool,newCachedThreadPool,newScheduledThreadPool和newSingleThreadExecutor

+ newFixedThreadPool：该线程池是一种线程数量固定的线程池，在这个线程池中 **所容纳最大的线程数就是我们设置的核心线程数。** 如果线程池的线程处于空闲状态的话，它们并不会被回收，除非是这个线程池被关闭。如果所有的线程都处于活动状态的话，新任务就会处于等待状态，直到有线程空闲出来。不存在超时机制，采用**LinkedBlockingQueue**，所以对于任务队列的大小也是没有限制的

+ newCachedThreadPool：**核心线程数为0，** 线程池的最大线程数Integer.MAX_VALUE。而Integer.MAX_VALUE是一个很大的数，也差不多可以说 **这个线程池中的最大线程数可以任意大。**

  **当线程池中的线程都处于活动状态的时候，线程池就会创建一个新的线程来处理任务。该线程池中的线程超时时长为60秒，所以当线程处于闲置状态超过60秒的时候便会被回收。** 这也就意味着若是整个线程池的线程都处于闲置状态超过60秒以后，在newCachedThreadPool线程池中是不存在任何线程的，所以这时候它几乎不占用任何的系统资源。

  对于newCachedThreadPool他的任务队列采用的是**SynchronousQueue**，上面说到在SynchronousQueue内部没有任何容量的阻塞队列。SynchronousQueue内部相当于一个空集合，我们无法将一个任务插入到SynchronousQueue中。所以说在线程池中如果现有线程无法接收任务,将会创建新的线程来执行任务。

+ newScheduledThreadPool：它的核心线程数是固定的，对于非核心线程几乎可以说是没有限制的，并且当非核心线程处于限制状态的时候就会立即被回收。使用**DelayedWorkQueue**实现等待队列

+ newSingleThreadExecutor：通过Executors中的newSingleThreadExecutor方法来创建，**在这个线程池中只有一个核心线程**，对于任务队列没有大小限制，也就意味着**这一个任务处于活动状态时，其他任务都会在任务队列中排队等候依次执行**，使用**LinkedBlockingQueue**实现等待队列

##### 0x03 死锁

+ 一般来说，要出现死锁问题需要满足以下条件：
  1. 互斥条件：一个资源每次只能被一个线程使用。
  2. 保持与请求条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放。
  3. 不剥夺条件：线程已获得的资源，在未使用完之前，不能强行剥夺。
  4. 循环等待条件：若干线程之间形成一种头尾相接的循环等待资源关系。
+ 在JAVA编程中，有3种典型的死锁类型：静态的锁顺序死锁，动态的锁顺序死锁，协作对象之间发生的死锁。
+ **解决静态的锁顺序死锁的方法就是：所有需要多个锁的线程，都要以相同的顺序来获得锁。**
+ 动态的锁顺序死锁是指两个线程调用**同一个方法**时，传入的参数颠倒造成的死锁。
+  **我们在持有锁的情况下调用了外部的方法，这是非常危险的（可能发生死锁）**
+ **解决协作对象之间发生的死锁：需要使用开放调用，即避免在持有锁的情况下调用外部的方法。**

##### 0x04 锁

+ 一共有两种锁，来实现线程同步问题，分别是：`synchronized`和`ReentrantLock`

+ synchronized的原理：JVM基于进入和退出`Monitor`对象来实现 **代码块同步** 和 **方法同步** ，两者实现细节不同。

+ **代码块同步：** 在编译后通过将`monitorenter`指令插入到同步代码块的开始处，将`monitorexit`指令插入到方法结束处和异常处，通过反编译字节码可以观察到。任何一个对象都有一个`monitor`与之关联，线程执行`monitorenter`指令时，会尝试获取对象对应的`monitor`的所有权，即尝试获得对象的锁。

  **方法同步：** synchronized方法在`method_info结构`有`ACC_synchronized`标记，线程执行时会识别该标记，获取对应的锁，实现方法同步。

+ ReentrantLock提供了与`synchronized`关键字类似的同步功能，只是在使用时需要显式地获取和释放锁，缺点就是缺少像`synchronized`那样隐式获取释放锁的便捷性，但是却拥有了锁获取与释放的可操作性

+ 当一个线程得到一个对象后，再次请求该对象锁时是可以再次得到该对象的锁的。

  具体概念就是：自己可以再次获取自己的内部锁。

  Java里面内置锁(synchronized)和Lock(ReentrantLock)都是可重入的。

+ CPU在调度线程的时候是在等待队列里随机挑选一个线程，由于这种随机性所以是无法保证线程**先到先得**的（synchronized控制的锁就是这种非公平锁）。但这样就会产生饥饿现象，即有些线程（优先级较低的线程）可能永远也无法获取CPU的执行权，优先级高的线程会不断的强制它的资源。那么如何解决饥饿问题呢，这就需要公平锁了。公平锁可以保证线程**按照时间的先后顺序**执行，避免饥饿现象的产生。但公平锁的效率比较低，因为要实现顺序执行，需要维护一个有序队列。

  ReentrantLock便是一种公平锁，通过在构造方法中传入true就是公平锁，传入false，就是非公平锁。

+ synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；

+ Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。

+ 在Java中，**synchronized就不是可中断锁，而Lock是可中断锁**。如果某一线程A正在执行锁中的代码，另一线程B正在等待获取该锁，可能由于等待时间过长，线程B不想等待了，想先处理其他事情，我们可以让它中断自己或者在别的线程中中断它，这种就是可中断锁。

+ 读写锁将对一个资源（比如文件）的访问分成了2个锁，一个读锁和一个写锁。

  正因为有了读写锁，才使得多个线程之间的读操作可以并发进行，不需要同步，而写操作需要同步进行，提高了效率。

+ 一个ReentrantLock对象可以同时绑定多个Condition对象，而在synchronized中，锁对象的wait()和notify()或notifyAll()方法可以实现一个隐含的条件，如果要和多余一个条件关联的时候，就不得不额外地添加一个锁，而ReentrantLock则无须这么做，只需要多次调用new Condition()方法即可。

+ Thread.sleep()，它会导致线程睡眠指定的毫秒数，但线程在睡眠的过程中是不会释放掉对象的锁的。

+ **synchronized相当于整个ReentrantLock对象只有一个单一的Condition对象情况。而一个ReentrantLock却可以拥有多个Condition对象，来实现通知部分线程。**

+ 假设有两个Condition对象：ConditionA和ConditionB。那么由ConditionA.await()方法进入等待状态的线程，由ConditionA.signalAll()通知唤醒；由ConditionB.await()方法进入等待状态的线程，由ConditionB.signalAll()通知唤醒。

##### 0x05 volatile

+ Java内存模型规定了**所有的变量都存储在主内存中**。**每条线程中还有自己的工作内存，线程的工作内存中保存了被该线程所使用到的变量（这些变量是从主内存中拷贝而来）**。**线程对变量的所有操作（读取，赋值）都必须在工作内存中进行。不同线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递均需要通过主内存来完成**。

+ 原子性：即一个操作或者多个操作，要么全部执行，并且执行的过程不会被任何因素打断，要么就都不执行。在Java中，**对基本数据类型的变量的读取和赋值操作是原子性操作**，即这些操作是不可被中断的，要么执行，要么不执行。

  **只有简单的读取、赋值（而且必须是将数字赋值给某个变量，变量之间的相互赋值不是原子操作）才是原子操作。**

  从上面可以看出，Java内存模型只保证了基本读取和赋值是原子性操作，**如果要实现更大范围操作的原子性，可以通过synchronized和Lock来实现。由于synchronized和Lock能够保证任一时刻只有一个线程执行该代码块，那么自然就不存在原子性问题了，从而保证了原子性。**

+ 可见性：当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。

+ **当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值。**

+ 有序性：即程序执行的顺序按照代码的先后顺序执行。

+ 下面解释一下什么是指令重排序，**一般来说，处理器为了提高程序运行效率，可能会对输入代码进行优化，它不保证程序中各个语句的执行先后顺序同代码中的顺序一致，但是它会保证程序最终执行结果和代码顺序执行的结果是一致的。**

+ **处理器在进行重排序时是会考虑指令之间的数据依赖性，如果一个指令Instruction 2必须用到Instruction 1的结果，那么处理器会保证Instruction 1会在Instruction 2之前执行。**

+ 一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：

  1）保证了**不同线程对这个变量进行操作时的可见性**，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。

  2）**禁止进行指令重排序。**

+ volatile关键字禁止指令重排序有两层意思：

  1）当程序执行到volatile变量的读操作或者写操作时，**在其前面的操作的更改肯定全部已经进行，且结果已经对后面的操作可见；在其后面的操作肯定还没有进行**；

  2）在进行指令优化时，**不能将在对volatile变量的读操作或者写操作的语句放在其后面执行，也不能把volatile变量后面的语句放到其前面执行。**

+ **可以通过synchronized或lock，进行加锁，来保证操作的原子性。也可以通过AtomicInteger。**

+ 单例中的volatitle：

  主要在于instance = new Singleton()这句，这并非是一个原子操作，事实上在 JVM 中这句话大概做了下面 3 件事情:

  1.给 instance 分配内存

  2.调用 Singleton 的构造函数来初始化成员变量

  3.将instance对象指向分配的内存空间（执行完这步 instance 就为非 null 了）。

  但是在 JVM 的即时编译器中存在指令重排序的优化。也就是说上面的第二步和第三步的顺序是不能保证的，最终的执行顺序可能是 1-2-3 也可能是 1-3-2。如果是后者，则在 3 执行完毕、2 未执行之前，被线程二抢占了，这时 instance 已经是非 null 了（但却没有初始化），所以线程二会直接返回 instance，然后使用，然后顺理成章地报错。
  
+ 双重判定单例模式两个If的作用

  > class SingletonTwo{	
  > 	/* 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载 */ 
  > 	private static SingletonTwo instance = null;	
  > 	/* 私有构造方法，防止被实例化 */
  > 	private SingletonTwo(){}
  >
  > ​	public static SingletonTwo getInstance() {  
  >
  > ​    	//位置1
  > ​    	if (instance == null) {  
  > ​        	//位置2
  > ​        	synchronized (instance) {  
  > ​        		//位置3
  > ​           	 if (instance == null) {  
  > ​                //位置4
  > ​                instance = new SingletonTwo();  
  > ​            	}  
  > ​       	 }  
  > ​    	}  
  > ​    	return instance;  
  > ​	}  
  >
  > }

  假设线程A和B作为第一批调用者同时或几乎同时调用静态工厂方法getInstance。

  1）因为A和B是第一批调用者，当它们进入静态工厂方法时，instance变量是null。因此它们几乎同时到达 位置2。

  2)  假设A线程先进入 synchronized (instance)，到达位置3，这时由于同步机制，线程B无法到达位置3，只能在位置2等待。

  3）线程A执行instance = new SingletonTwo()语句，使得instance引用指向一个对象。此时线程B还在位置2上等待。

  4）线程A退出synchronized (instance)，返回SingletonTwo对象，退出静态工厂方法。

  5）线程B进入 synchronized (instance)块，达到位置3，此时instance已经不为null，因此线程B退出synchronized (instance)，返回SingletonTwo对象（线程A所创建的SingletonTwo对象），退出静态工厂方法。

  到此为止，线程A和B得到同一个SingletonTwo对象。


  第一个if判断的作用：是为了提高程序的 效率，当SingletonTwo对象被创建以后，再获取SingletonTwo对象时就不用去验证同步代码块的锁及后面的代码，直接返回SingletonTwo对象

  第二个if判断的作用：是为了解决多线程下的安全性问题，也就是保证对象的唯一。如果没有第二个if判断，在上面介绍的步骤5处，线程B进入synchronized (instance)块，不用去验证instance是否为null，就会直接创建一个SingletonTwo新对象，这样整个程序运行下来就有可能创建多个实例。

##### 0x06 CAS

+ **在某个资源不可用的时候，就将cpu让出，把当前等待线程切换为阻塞状态。等到资源(比如一个共享数据）可用了，那么就将线程唤醒，让他进入runnable状态等待cpu调度。这就是典型的悲观锁的实现。**
+ **在进程挂起和恢复执行过程中存在着很大的开销**。当一个线程正在等待锁时，它不能做任何事，所以悲观锁有很大的缺点。举个例子，如果一个线程需要某个资源，但是这个资源的占用时间很短，当线程第一次抢占这个资源时，可能这个资源被占用，如果此时挂起这个线程，可能立刻就发现资源可用，然后又需要花费很长的时间重新抢占锁，时间代价就会非常的高。
+ 乐观锁的概念，他的核心思路就是，**每次不加锁而是假设修改数据之前其他线程一定不会修改，如果因为修改过产生冲突就失败就重试，直到成功为止。** 在上面的例子中，某个线程可以不让出cpu，而是一直while循环，如果失败就重试，直到成功为止。
+ **CAS 操作包含三个操作数 —— 内存位置（V）、预期原值（A）和新值(B)。执行CAS操作的时候，将内存位置的值与预期原值比较，如果相匹配，那么处理器会自动将该位置值更新为新值。否则，处理器不做任何操作。**
+ compareAndSet方法内部是调用Java本地方法compareAndSwapInt来实现的，而compareAndSwapInt方法内部又是借助C来调用CPU的底层指令来保证在硬件层面上实现原子操作的。在intel处理器中，CAS是通过调用**cmpxchg**指令完成的。这就是我们常说的**CAS操作**
+ ABA问题。因为CAS需要在操作值的时候检查下值有没有发生变化，如果没有发生变化则更新，但是如果一个值原来是A，变成了B，又变成了A，那么使用CAS进行检查时会发现它的值没有发生变化，但是实际上却变化了。ABA问题的解决思路就是使用版本号。在变量前面追加上版本号，每次变量更新的时候把版本号加一，那么A－B－A 就会变成1A-2B－3A。从Java1.5开始JDK的atomic包里提供了一个类AtomicStampedReference来解决ABA问题。这个类的compareAndSet方法作用是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。
+ 只能保证一个共享变量的原子操作。当对一个共享变量执行操作时，我们可以使用循环CAS的方式来保证原子操作，但是对多个共享变量操作时，循环CAS就无法保证操作的原子性，这个时候就可以用锁，或者有一个取巧的办法，就是把多个共享变量合并成一个共享变量来操作。比如有两个共享变量i＝2,j=a，合并一下ij=2a，然后用CAS来操作ij。从Java1.5开始JDK提供了AtomicReference类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行CAS操作。

##### 0x06 ReentrantLock深入理解

+ 在**非公平锁**中，每当线程执行lock方法时，都尝试利用CAS把state从0设置为1。

  那么Doug lea是如何实现锁的非公平性呢？ 我们假设这样一个场景：

  1. 持有锁的线程A正在running，队列中有线程BCDEF被挂起并等待被唤醒；
  2. 在某一个时间点，线程A执行unlock，唤醒线程B；
  3. 同时线程G执行lock，这个时候会发生什么？线程B和G拥有相同的优先级，这里讲的优先级是指获取锁的优先级，同时执行CAS指令竞争锁。如果恰好线程G成功了，线程B就得重新挂起等待被唤醒。

  通过上述场景描述，我们可以看出，即使线程B等了很长时间也得和新来的线程G同时竞争锁。

+ 在**公平锁**中，每当线程执行lock方法时，如果同步器的队列中有线程在等待，则直接加入到队列中。 场景分析：

  1. 持有锁的线程A正在running，对列中有线程BCDEF被挂起并等待被唤醒；
  2. 线程G执行lock，队列中有线程BCDEF在等待，线程G直接加入到队列的对尾。

  所以每个线程获取锁的过程是公平的，等待时间最长的会最先被唤醒获取锁。

+ 条件变量很大一个程度上是为了解决Object.wait/notify/notifyAll难以使用的问题。

+ Synchronized中，所有的线程都在同一个object的条件队列上等待。而ReentrantLock中，每个condition都维护了一个条件队列。

+ 每一个**Lock**可以有任意数据的**Condition**对象，**Condition**是与**Lock**绑定的，所以就有**Lock**的公平性特性：如果是公平锁，线程为按照FIFO的顺序从*Condition.await*中释放，如果是非公平锁，那么后续的锁竞争就不保证FIFO顺序了。

+ Condition接口定义的方法，**await**对应于**Object.wait**，**signal**对应于**Object.notify**，**signalAll**对应于**Object.notifyAll**。特别说明的是Condition的接口改变名称就是为了避免与Object中的*wait/notify/notifyAll*的语义和使用上混淆。



#### Java虚拟机

+ 对象的创建

  1.虚拟机遇到一个new指令时，首先将去检查这个指令的参数是否能在常量池中定位到一个类的符号引用；

  2.检查这个符号引用代表的类是否已经被加载，解析和初始化过。如果没有，那必须先执行响应的类加载过程；

  3.在类加载检查功通过后，为新生对象分配内存。对象所需的内存大小在类加载完成后便可完全确定。

+ 对象的内存布局：对象头+实例数据+对齐部分

+ 对象的访问定位：Java程序需要通过栈上了reference数据来操作堆上的具体对象。

  1、Java堆中会划分出一块内存来作为句柄池，reference中存储的就是对象的句柄地址，而句柄中包含了对实例数据与类型数据的各自具体的地址信息。

  2、reference中存储的直接就是对象地址。

+ **方法区（公有）：** 用户存储已被虚拟机加载的类信息，常量，静态常量，即时编译器编译后的代码等数据。异常状态 OutOfMemoryError

  其中包含常量池：用户存放编译器生成的各种字面量和符号引用。

  **堆（公有）：** 是JVM所管理的内存中最大的一块。唯一目的就是存放实例对象，几乎所有的对象实例都在这里分配。Java堆是垃圾收集器管理的主要区域，因此很多时候也被称为“GC堆”。异常状态 OutOfMemoryError

  **虚拟机栈（线程私有）：** 描述的是java方法执行的内存模型：每个方法在执行时都会创建一个栈帧，用户存储局部变量表，操作数栈，动态连接，方法出口等信息。每一个方法从调用直至完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。 对这个区域定义了两种异常状态 OutOfMemoryError StackOverflowError

  **本地方法栈（线程私有）:** 与虚拟机栈所发挥的作用相似。它们之间的区别不过是虚拟机栈为虚拟机执行java方法，而本地方法栈为虚拟机使用到的Native方法服务。

  **程序计数器（线程私有）：** 一块较小的内存，当前线程所执行的字节码的行号指示器。字节码解释器工作时，就是通过改变这个计数器的值来选取下一条需要执行的字节码指令。

+ **Java内存模型的目的：** 屏蔽掉各种硬件和操作系统的内存访问差异，以实现让java程序在各种平台下都能达到一致的内存访问效果。

+ **Java内存模型规定了所有的变量都存储在主内存中。每条线程中还有自己的工作内存，线程的工作内存中保存了被该线程所使用到的变量（这些变量是从主内存中拷贝而来）。线程对变量的所有操作（读取，赋值）都必须在工作内存中进行。不同线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递均需要通过主内存来完成。**

  ![1566370230316](.\image\1566370230316.png)

+ 加载，验证，准备，解析，初始化，使用和卸载。其中验证，准备，解析3个部分统称为连接。

  这7个阶段发生顺序如下图：

  ![1566372576308](image/1566372576308.png)

  加载，验证，准备，初始化，卸载这5个阶段的顺序是确定的，而解析阶段则不一定：它在某些情况下可以在初始化完成后在开始，这是为了支持Java语言的运行时绑定。

  **其中加载，验证，准备，解析及初始化是属于类加载机制中的步骤。注意此处的加载不等同于类加载。**

+ ①.遇到new,getstatic,putstatic或invokestatic这4条字节码指令时，如果类没有进行过初始化，则需要先触发初始化。生成这4条指令的最常见的Java代码场景是：使用new关键字实例化对象的时候，读取或设置一个类的静态字段的时候（被final修饰，已在编译期把结果放入常量池的静态字段除外），以及调用一个类的静态方法的时候。

  ②.使用java.lang.reflect包的方法对类进行反射调用的时候。

  ③.当初始化一个类的时候，发现其父类还没有进行过初始化，则需要先出发父类的初始化。

  ④.当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先初始化这个主类。

  ⑤.当使用JDK1.7的动态语言支持时，如果一个`java.lang.invoke.MethodHandle`实例最后的解析结果`REF_getStatic,REF_putStatic,REF_invokeStatic`的方法句柄，并且这个方法句柄所对应的类没有进行初始化，则需要先出发初始化。

+ 类加载机制的大概流程及各个部分的功能。其中加载部分的功能是将类的class文件读入内存，并为之创建一个java.lang.Class对象。这部分功能就是由类加载器来实现的。

+ **启动类加载器（Bootstrap ClassLoader）：** 由C++语言实现（针对HotSpot）,负责将存放在\lib目录或-Xbootclasspath参数指定的路径中的类库加载到内存中，即负责加载Java的核心类。

  **其他类加载器：** 由Java语言实现，继承自抽象类ClassLoader。如：

  **扩展类加载器（Extension ClassLoader）：** 负责加载\lib\ext目录或java.ext.dirs系统变量指定的路径中的所有类库，即负责加载Java扩展的核心类之外的类。

  **应用程序类加载器（Application ClassLoader）：** 负责加载用户类路径（classpath）上的指定类库，我们可以直接使用这个类加载器，通过ClassLoader.getSystemClassLoader()方法直接获取。一般情况，如果我们没有自定义类加载器默认就是用这个加载器。

+ 双亲委派模型的工作流程是：如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把请求委托给父加载器去完成，依次向上，因此，所有的类加载请求最终都应该被传递到顶层的启动类加载器中，只有当父加载器在它的搜索范围中没有找到所需的类时，即无法完成该加载，子加载器才会尝试自己去加载该类。

+ 这样的好处是不同层次的类加载器具有不同优先级，比如所有Java对象的超级父类java.lang.Object，位于rt.jar，无论哪个类加载器加载该类，最终都是由启动类加载器进行加载，保证安全。即使用户自己编写一个java.lang.Object类并放入程序中，虽能正常编译，但不会被加载运行，保证不会出现混乱。

+ 标记清除法：最基础的收集算法是“标记-清除”（Mark-Sweep）算法，如同它的名字一样，算法分为“标记”和“清除”两个阶段。

  ①首先标记出所有需要回收的对象

  ②在标记完成后统一回收所有被标记的对象。

  **不足：**

  效率问题：标记和清除两个过程的效率都不高

  空间问题：标记清除之后产生大量不连续的内存碎片，空间碎片太多可能会导致以后程序运行过程中需要分配较大对象时，无法找到足够的连续内存而不得不提前触发另一次垃圾收集动作。

  ![1566373299292](image/1566373299292.png)

+ 复制清除法：**目的：** 为了解决效率问题。

  将可用内存按容量大小划分为大小相等的两块，每次只使用其中的一块。当一块内存使用完了，就将还存活着的对象复制到另一块上面，然后再把已使用过的内存空间一次清理掉。这样使得每次都是对整个半区进行内存回收，内存分配时也就不用考虑内存碎片等复杂情况。

  **缺点：** 将内存缩小为了原来的一半。

  现代的商业虚拟机都采用这种收集算法来回收新生代，IBM公司的专门研究表明，新生代中对象98%对象是“朝生夕死”的，所以不需要按照1：1的比例来划分内存空间，而是**将内存分为较大的Eden空间和两块较小的Survivor空间，每次使用Eden和其中一块Survivor。HotSpot虚拟机中默认Eden和Survivor的大小比例是8：1。**

  ![1566373311948](image/1566373311948.png)

+ 标记-整理法：复制收集算法在对象存活率较高时，就要进行较多的复制操作，效率就会变低。 根据老年代的特点，提出了“标记-整理”算法。

  标记过程仍然与”标记-清除“算法一样，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉边界以外的内存。

  ![1566373391820](image/1566373391820.png)

+ 分代收集法：一般是把Java堆分为新生代和老年代，这样就可以根据各个年代的特点采用最适当的收集算法。在新生代中，每次垃圾收集时都发现有大批对象死去，只有少量存活，那就选用复制算法。在老年代中，因为对象存活率高、没有额外空间对它进行分配担保，就必须采用“标记-清除”或“标记-整理”算法来进行回收。

+ **JVM中分为年轻代（Young generation）和老年代(Tenured generation)**。

  **HotSpot JVM把年轻代分为了三部分**：1个Eden区和2个Survivor区（分别叫from和to）。默认比例为8：1，为啥默认会是这个比例，接下来我们会聊到。

  **一般情况下，新创建的对象都会被分配到Eden区**(一些大对象特殊处理)，这些对象经过第一次Minor GC后，如果仍然存活，将会被移到Survivor区。对象在Survivor区中每熬过一次Minor GC，年龄就会增加1岁，当它的年龄增加到一定程度时，就会被移动到年老代中。 因为年轻代中的对象基本都是朝生夕死的(80%以上)，所以**在年轻代的垃圾回收算法使用的是复制算法**，复制算法的基本思想就是将内存分为两块，每次只用其中一块，当这一块内存用完，就将还活着的对象复制到另外一块上面。复制算法不会产生内存碎片。

  在GC开始的时候，对象只会存在于Eden区和名为“From”的Survivor区，Survivor区“To”是空的。紧接着进行GC，Eden区中所有存活的对象都会被复制到“To”，而在“From”区中，仍存活的对象会根据他们的年龄值来决定去向。年龄达到一定值(年龄阈值，可以通过-XX:MaxTenuringThreshold来设置)的对象会被移动到年老代中，没有达到阈值的对象会被复制到“To”区域。经过这次GC后，Eden区和From区已经被清空。这个时候，“From”和“To”会交换他们的角色，也就是新的“To”就是上次GC前的“From”，新的“From”就是上次GC前的“To”。不管怎样，都会保证名为To的Survivor区域是空的。Minor GC会一直重复这样的过程，直到“To”区被填满，“To”区被填满之后，会将所有对象移动到年老代中。

+ Minor GC:指发生在新生代的垃圾收集动作，该动作非常频繁。

  Full GC/Major GC:指发生在老年代的垃圾收集动作，出现了Major GC，经常会伴随至少一次的Minor GC。Major GC的速度一般会比Minor GC慢10倍以上。

+ 在发生Minor GC之前，虚拟机会**先检查老年代最大可用的连续空间是否大于新生代所有对象的总空间**，如果这个条件成立，那么Minor GC可以 确保是安全的。如果不成立，则虚拟机**会查看HandlePromotionFailure设置值是否允许担保失败**。如果允许，那会继续检查老年代最大可用的连续空间**是否大于历次晋升到老年代对象的平均大小，如果大于，则将尝试进行一次Minor GC**，尽管这个Minor GC是有风险的。

+ 给对象添加一个引用计数器，每当有一个地方引用它时，计数器值就加1；当引用失效时，计数器值就减1；任何时刻计数器为0的对象就是不可能被再使用的。

  **主流的JVM里面没有选用引用计数算法来管理内存，其中最主要的原因是它很难解决对象间的互循环引用的问题。**

+ 通过一些列的称为“GC Roots”的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链，当一个对象到GC Roots没有任何引用链相连时（就是从GC Roots 到这个对象是不可达），则证明此对象是不可用的。所以它们会被判定为可回收对象

+ 在Java语言中，可以作为GC Roots的对象包括下面几种：

  - 虚拟机栈（栈帧中的本地变量表）中引用的对象；
  - 方法区中类静态属性引用的对象；
  - 方法区中常量引用的对象；
  - 本地方法栈中JNI（即一般说的Native方法）引用的对象；

  总结就是，方法运行时，方法中引用的对象；类的静态变量引用的对象；类中常量引用的对象；Native方法中引用的对象

+ **在可达性分析算法中，要真正宣告一个对象死亡，至少要经历两次标记过程：**

  1.如果对象在进行可达性分析后发现没有与GC Roots相连接的引用链，那它将会被第一次标记并且进行一次筛选，筛选的条件是此对象是否有必要执行finalize()方法。当对象没有 覆盖finalize()方法，或者finalize()方法已经被虚拟机调用过，虚拟机将这两种情况都视为“没有必要执行”。

  2.如果这个对象被判定为有必要执行finalize()方法，那么这个对象将会放置在一个叫做F-Queue队列之中，并在稍后由一个由虚拟机自动建立的、低优先级的Finalizer线程去执行它。finalize()方法是对象逃脱死亡命运的最后一次机会，稍候GC将对F-Queue中的对象进行第二次小规模的标记，如果对象要在finalie()中成功拯救自己——只要重新与引用链上的任何一个对象建立关联即可，譬如把自己（this关键字）赋值给某个类变量或者对象的成员变量，那在第二次标记时它将会被移除出“即将回收”的集合；如果对象这时候还没有逃脱，那基本上它就真的被回收了。

+ **强引用：** 就是指在程序代码之中普遍存在的，类似“Object obj = new Object()”这类的引用，只要强引用还存在，垃圾收集器永远不会回收掉被引用的对象。

  **软引用：** 用来描述一些还有用但并非必须的对象。在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之中进行第二次回收。

  **弱引用：** 用户描述非必须对象的。被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。

  **虚引用：** 一个对象是否有虚引用存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象实例。为一个对象设置虚引用的唯一目的就是能在这个对象被收集器回收时刻得到一个系统通知。
  
  强（strong）、软（soft）、弱（weak）、虚（phantom）引用
  Phantom reference
  和soft，weak Reference区别较大，它的get()方法总是返回null。这意味着你只能用PhantomReference本身，而得不到它指向的对象。当WeakReference指向的对象变得弱可达(weakly reachable）时会立即被放到ReferenceQueue中，这在finalization、garbage collection之前发生。理论上，你可以在finalize()方法中使对象“复活”（使一个强引用指向它就行了，gc不会回收它）。但没法复活PhantomReference指向的对象。而PhantomReference是在garbage collection之后被放到ReferenceQueue中的，没法复活。

##### 感谢
感谢 swiftieslizheng 的分享！     *[点击进入他的GitHub！](http://www.lizheng0928.top/)*

感谢文中链接作者的分享！



（如有引用不当，请发邮件到：1678866742@qq.com  联系我删除！）