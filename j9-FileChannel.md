## FileChannel

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