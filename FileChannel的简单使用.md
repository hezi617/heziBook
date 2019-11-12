## FileChannel的简单使用
### FileChannel用于文件的数据读写。
在D盘根目录创建一个文件“nio.txt”，并在在里面写有字符串“hello_hezi”
```java
package nIO;

import java.io.IOException;
import java.io.RandomAccessFile;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class TestFileChannel {
	public static void main(String[] args) throws IOException {
		
		RandomAccessFile raf = new RandomAccessFile("D://nio.txt","rw");
		FileChannel fileChannel = raf.getChannel();
		ByteBuffer byteBuffer = ByteBuffer.allocate(48);
		int read = fileChannel.read(byteBuffer);
		
		while(read!=-1) {
			System.out.println("read值为："+read);
			//改为读模式
			byteBuffer.flip();
			//byteBuffer.hasRemaining()：Tells whether there are any elements between the current position and the limit.
			while(byteBuffer.hasRemaining()) {
				System.out.print((char)byteBuffer.get());
			}
			byteBuffer.clear();
			read = fileChannel.read(byteBuffer);
		}
		raf.close();
	}
}
```

输出结果为：read值为：10

​						hello_hezi