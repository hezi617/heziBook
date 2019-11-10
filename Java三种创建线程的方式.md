## 三种创建线程的方式

### 方法一：继承Thread类
```java
package hezi.thread;
/**
 * 方法一：继承Thread类，作为线程对象存在（继承Thread对象）
 * @author HeZi
 */
public class CreatThreadDemo1 extends Thread {
	/**
	 *  构造方法： 继承父类方法的Thread(String name)；
	 */
	public CreatThreadDemo1(String name) {
		super(name);
	}
	
	//重写run方法：该run方法的方法体就代表了 线程要完成的任务。
	//因此把run()方法称为执行体。
	@Override
	public void run() {
		while (!interrupted()) {
			//getName()方法返回调用该方法的线程的名字
			System.out.println(getName() + "线程执行了。。。");
			try {
				Thread.sleep(200);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
	public static void main(String[] args) {
		CreatThreadDemo1 d1 = new CreatThreadDemo1("first");
		CreatThreadDemo1 d2 = new CreatThreadDemo1("second");
		d1.start();
		d2.start();
		d1.interrupt(); // 中断第一个线程
	}
}

```
```java
package hezi.thread;

public class FirstThreadTest extends Thread{
	int i= 0;
	@Override
	public void run() {
		for(;i<100;i++) {
			System.out.println(getName()+"  "+i);
		}
	}
	public static void main(String[] args) {
		for(int i=0;i<100;i++) {
			System.out.println(Thread.currentThread().getName()+"  "+i);
			if(i==20) {
				new FirstThreadTest().start();
				new FirstThreadTest().start();
			}
		}
	}
}
```
### 方法二：实现runnable接口
```java
package hezi.thread;
/**
 * 方法二：实现runnable接口，作为线程任务存在
 * @author HeZi
 *
 */
public class CreatThreadRunnable implements Runnable{

	@Override
	public void run() {
			System.out.println("线程执行了。。。");
	}
	
	public static void main(String[] args) {
		//将线程任务传给线程对象
		Thread thread1 = new Thread(new CreatThreadRunnable());
		thread1.start();
		if(thread1.isAlive()) 
			System.out.println(thread1.getName());
		thread1.interrupt();
	}
}
```
```java
package hezi.thread;

public class RunnableThreadTest implements Runnable{
	private int i;
	public void run() {
		for (i = 0; i < 100; i++) {
			System.out.println(Thread.currentThread().getName() + "	" + i);
		}
	}
	public static void main(String[] args) {
		for (int i = 0; i < 100; i++) {
			System.out.println(Thread.currentThread().getName() + "	" + i);
			if (i == 20) {
				RunnableThreadTest rtt = new RunnableThreadTest();
				new Thread(rtt, "新线程1").start();
				new Thread(rtt, "新线程2").start();
			}
		}
	}
}
```
### 方法三：继承Callable接口
```java
package hezi.thread;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

/**
 * 方法三：创建带返回值的线程，继承Callable接口
 * 
 * @author HeZi
 */
public class CreatThreadCallable implements Callable {
	@Override
	public Object call() throws Exception {
		int result = 1;
		System.out.println("业务逻辑计算中...");
		Thread.sleep(2000);
		return result;
	}

	public static void main(String[] args) throws InterruptedException, ExecutionException {
		Callable demo4 = new CreatThreadCallable();
		FutureTask<Integer> task = new FutureTask<Integer>(demo4); // FutureTask最终实现的是runnable接口
		Thread thread = new Thread(task);
		thread.start();
		System.out.println("我可以在这里做点别的业务逻辑...因为FutureTask是提前完成任务");
		// 拿出线程执行的返回值
		Integer result = task.get();
		System.out.println("线程中运算的结果为:" + result);
	}
}
```
```java
package hezi.thread;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class TestCallableThread implements Callable<Integer> {
	
	public static void main(String[] args) {
		TestCallableThread ctt = new TestCallableThread();
		FutureTask<Integer> ft = new FutureTask<>(ctt);
		for (int i = 0; i < 100; i++) {
			System.out.println(Thread.currentThread().getName()
					+ "	的循环变量i的值" + i);
			if (i == 20) {
				new Thread(ft, "有返回值的线程").start();
			}
		}
		try {
			System.out.println("子线程的返回值：" + ft.get());
		} catch (InterruptedException e) {
			e.printStackTrace();
		} catch (ExecutionException e) {
			e.printStackTrace();
		}
	}
	
	@Override
	public Integer call() throws Exception {
		int i = 0;
		for (; i < 100; i++) {
			System.out.println(Thread.currentThread().getName() + "	" + i);
		}
		return i;
	}
}
```