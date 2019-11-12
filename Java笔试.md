1、下面哪些是Thread类的方法（） A start()       B run()       C exit()       D getPriority() 

答案：ABD   	解析：看Java API docs吧：http://docs.oracle.com/javase/7/docs/api/，exit()是System类的方法， 如System.exit(0)。

2、下列说法正确的有（）

 A． class中的constructor不可省略 

 B． constructor必须与class同名，但方法不能与class同名

 C． constructor在一个对象被new时执行

 D．一个class只能定义一个constructor 

答案：C  解析：这里可能会有误区，其实普通的类方法是可以和类名同名的，和构造方法唯一的区分就是，构造 方法没有返回值。

3、 具体选项不记得，但用到的知识如下：

 	String []a = new String[10]; 

​	 则：a[0]~a[9] = null a.length = 10

 	如果是int []a = new int[10]; 

​	则：a[0]~a[9] = 0 a.length = 10

4、下列属于关系型数据库的是（）

  A. Oracle 

   B MySql   

  C IMS  

   D MongoDB 

​	答案：AB 解答：IMS（Information Management System ）数据库是IBM公司开发的两种数据库类型之一;  一种是关系数据库，典型代表产品：DB2； 另一种则是层次数据库，代表产品：IMS层次数据库。 非关系型数据库有MongoDB、memcachedb、Redis等。

5、 下面程序的运行结果：（）
```java
    public static void main(String args[]) {
        Thread t = new Thread() { 
			public void run() {
        	pong();
       	    } 
        }; 
        t.run(); 
        System.out.print("ping"); 
    } 
	static void pong() { 
        System.out.print("pong"); 
    }
```
A pingpong        B pongping       C pingpong和pongping都有可能       D 都不输出 

​	答案：B 解析：这里考的是Thread类中start()和run()方法的区别了。start()用来启动一个线程，当调用start方法 后，系统才会开启一个新的线程，进而调用run()方法来执行任务，而单独的调用run()就跟调用普通方法 是一样的，已经失去线程的特性了。因此在启动一个线程的时候一定要使用start()而不是run()。

6、GC线程是否为守护线程？（） 

答案：是

 解析：线程分为守护线程和非守护线程（即用户线程）。 只要当前JVM实例中尚存在任何一个非守护线程没有结束，守护线程就全部工作；只有当最后一个非守 护线程结束时，守护线程随着JVM一同结束工作。 守护线程最典型的应用就是 GC (垃圾回收器)

7、 存在使i + 1 < i的数吗（） 

答案：存在 

解析：如果i为int型，那么当i为int能表示的最大整数时，i+1就溢出变成负数了，此时不就<i了吗。 

扩展：存在使i > j || i <= j不成立的数吗（） 

答案：存在

解析：比如Double.NaN或Float.NaN

8、 0.6332的数据类型是（） 

A float     B double     C Float      D Double 

答案：B 解析：默认为double型，如果为float型需要加上f显示说明，即0.6332f

9、不通过构造函数也能创建对象吗（） 

​	A 是     B 否 

答案：A 

解析：Java创建对象的几种方式（重要）：

 (1) 用new语句创建对象，这是最常见的创建对象的方法。

 (2) 运用反射手段,调用java.lang.Class或者java.lang.reflect.Constructor类的newInstance()实例方法。 

 (3) 调用对象的clone()方法。 

 (4) 运用反序列化手段，调用java.io.ObjectInputStream对象的 readObject()方法。 

​		(1)和(2)都会明确的显式的调用构造函数 ；(3)是在内存上对已有对象的影印，所以不会调用构造函数 ； (4)是从文件中还原类的对象，也不会调用构造函数。

10、 ArrayList list = new ArrayList(20);中的list扩充几次（）

 	 A 0     B 1     C 2      D 3 

 答案：A 	解析：这里有点迷惑人，大家都知道默认ArrayList的长度是10个，所以如果你要往list里添加20个元素 肯定要扩充一次（扩充为原来的1.5倍），但是这里显示指明了需要多少空间，所以就一次性为你分配这 么多空间，也就是不需要扩充了。

11、下面哪些是对称加密算法（） 

​	A DES   B AES   C DSA   D RSA 

​	答案：AB 	解析：常用的对称加密算法有：DES、3DES、RC2、RC4、AES 

​									 常用的非对称加密算法有：RSA、DSA、ECC 

​									 使用单向散列函数的加密算法：MD5、SHA



12、下面程序能正常运行吗（）
```java
public class NULL { 
	public static void haha(){ 

​        System.out.println("haha");

​     } 

​	public static void main(String[] args) { 

​        ((NULL)null).haha(); 

​    } 
}
```
答案：能正常运行 解析：输出为haha，因为null值可以强制转换为任何java类类型,(String)null也是合法的。但null强制转 换后是无效对象，其返回值还是为null，而static方法的调用是和类名绑定的，不借助对象进行访问所以 能正确输出。反过来，没有static修饰就只能用对象进行访问，使用null调用对象肯定会报空指针错了。 这里和C++很类似。

13、 下面程序的运行结果是什么（）

```java
class HelloA { 
	public HelloA() {
    	System.out.println("HelloA");
    } 
    { System.out.println("I'm A class"); } 
	static { System.out.println("static A"); } 
} 
public class HelloB extends HelloA {
	public HelloB() {
    	System.out.println("HelloB");
    } 
    { System.out.println("I'm B class"); } 
	static { System.out.println("static B");} 
	public static void main(String[] args) {
   		 new HelloB();
    } 
}
/*答案
static A 
static B 
I'm A class 
HelloA 
I'm B class 
HelloB
*/
```

解析：考查静态语句块、构造语句块（就是只有大括号的那块）以及构造函数 的执行顺序。 

对象的初始化顺序：（1）类加载之后，按从上到下（从父类到子类）执行被static修饰的语句；（2）当 static语句执行完之后,再执行main方法；（3）如果有语句new了自身的对象，将从上到下执行构造代码 块、构造器（两者可以说绑定在一起）。 

14、下面代码的运行结果为：（）
```java
import java.io.*; import java.util.*; 
public class foo{ 
	public static void main (String[] args){ 
        String s; 
        System.out.println("s=" + s); 
    } 
}
```
​	A 代码得到编译，并输出“s=” 

​	B 代码得到编译，并输出“s=null” 

​	C 由于String s没有初始化，代码不能编译通过 

​	D 代码得到编译，但捕获到 NullPointException异常 

​	答案：C 解析：Java中所有定义的基本类型或对象都必须初始化才能输出值。

15、指出下列程序运行的结果 （）
```java
public class Example { 
    String str = new String("good"); 
	char[] ch = { 'a', 'b', 'c' }; 
	public static void main(String args[]) { 
        Example ex = new Example(); 
        ex.change(ex.str, ex.ch); 
        System.out.print(ex.str + " and "); 
        System.out.print(ex.ch); 
    } 
	public void change(String str, char ch[]) { 
        str = "test ok"; 
        ch[0] = 'g'; 
    }
}
```
A、 good and abc        B、 good and gbc       C、 test ok and abc       D、 test ok and gbc  	答案：B   解析：大家可能以为Java中String和数组都是对象所以肯定是对象引用，然后就会选D，其实这是个很大 的误区：因为在java里没有引用传递，只有值传递 这个值指的是实参的地址的拷贝，得到这个拷贝地址后，你可以通过它修改这个地址的内容（引用不 变），因为此时这个内容的地址和原地址是同一地址， 但是你不能改变这个地址本身使其重新引用其它的对象，也就是值传递。
说明：不管是对象、基本类型还是对象数组、基本类型数组，在函数中都不能改变其实际地址但能改变其中的内容。
16、下面的方法，当输入为2的时候返回值是多少?（）
```java
public static int getValue(int i) { 
	int result = 0; 
	switch (i) { 
		case 1:
        	result = result + i; 
		case 2:
        	result = result + i * 2; 
		case 3:
        	result = result + i * 3;
    } 
    return result;
    }
```
A0                    B2                    C4                     D10 
 答案：D	 解析：注意这里case后面没有加break，所以从case 2开始一直往下运行。
 17、选项中哪一行代码可以替换题目中//add code here而不产生编译错误？（）
```java
public abstract class MyClass {
	public int constInt = 5; 
	//add code here 
	public void method() { 
    }
}
```
A public abstract void method(int a);  	B constInt = constInt + 5;
C public int method();			        D public abstract void anotherMethod() {} 
答案： A 
18、下面是People和Child类的定义和构造方法，每个构造方法都输出编号。在执行**new Child("mike")**的 时候都有哪些构造方法被顺序调用？请选择输出结果 ( )

```java
class People {
	String name; 
	public People() { 
		System.out.print(1);
    } 
	public People(String name) {
    	System.out.print(2); 
    	this.name = name;
        }
} 
class Child extends People { 
	People father; 
	public Child(String name) { 
    	System.out.print(3); 
        this.name = name; 
        father = new People(name + ":F");
    } 
	public Child() {
    	System.out.print(4);
    } 
}
```
A	312              B 	32               C 	432              D 	132 
答案：D 	解析：考察的又是父类与子类的构造函数调用次序。在Java中，子类的构造过程中必须调用其父类的构 造函数，是因为有继承关系存在时，子类要把父类的内容继承下来。但如果父类有多个构造函数时，该 如何选择调用呢？ 第一个规则：子类的构造过程中，必须调用其父类的构造方法。一个类，如果我们不写构造方法，那么 编译器会帮我们加上一个默认的构造方法（就是没有参数的构造方法），但是如果你自己写了构造方 法，那么编译器就不会给你添加了，所以有时候当你new一个子类对象的时候，肯定调用了子类的构造 方法，但是如果在子类构造方法中我们并没有显示的调用基类的构造方法，如：super();  这样就会调用 父类没有参数的构造方法。     第二个规则：如果子类的构造方法中既没有显示的调用基类构造方法，而基类中又没有无参的构造方 法，则编译出错，所以，通常我们需要显示的：super(参数列表)，来调用父类有参数的构造函数，此时 无参的构造函数就不会被调用。 
总之，一句话：子类没有显示调用父类构造函数，不管子类构造函数是否带参数都默认调用父类无参的 构造函数，若父类没有则编译出错。