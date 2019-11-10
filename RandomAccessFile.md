##RandomAccessFile:测试利用多线程进行文件的写操作
```java
package randomAccessFile;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.RandomAccessFile;
/**
 * RandomAccessFile:测试利用多线程进行文件的写操作
 * @author HeZi
 *
 */
public class Test {
	public static void main(String[] args) throws IOException {
		//预分配文件所占的磁盘空间，磁盘中会创建一个指定大小的文件
		RandomAccessFile raf = new RandomAccessFile("D://useless.txt", "rw");
		raf.setLength(1024);
		raf.close();
		//所要写入的文件内容
		String s1 = "Hello，";
		String s2 = "Welcone to my github";
		String s3 = "My name is hezi!";
		//利用多线程同时写入一个文件
		new FileWriteThread(0,s1.getBytes()).start();
		new FileWriteThread(50*2,s2.getBytes()).start();
		new FileWriteThread(50*3,s3.getBytes()).start();
	}
	
	//利用多线程利用线程在文件的指定位置写入指定数据	
	 static class FileWriteThread extends Thread{
		private int skip;
		private byte[] content;
		public FileWriteThread(int skip,byte[] content) {
			this.skip = skip;
			this.content = content;
		}
		//重写run()方法
		public void run() {
			RandomAccessFile raf = null;
			try {
				 raf = new RandomAccessFile("D://useless.txt", "rw");
				 raf.seek(skip);
				 raf.write(content);
			} catch (FileNotFoundException e) {
				e.printStackTrace();
			} catch (IOException e) {
				e.printStackTrace();
			}finally {
				try {
					raf.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}
}

```