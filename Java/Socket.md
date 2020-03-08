## Socket
- **TCP通信**  
1. TCP通信的客户端：向服务器发送连接请求，给服务器发送数据，读取服务器回写的数据  
2. 表示客户端的类：`java.net.Socket`，此类实现客户端套接字。套接字是两台机器间通信的端点，是包含了IP地址和端口号的网络单位  
构造方法：`Socket(String host, int port)`：创建一个流套接字并将其连接到指定主机上的指定端口号。参数：`String host`，服务器主机的名称/服务器的IP地址。`int port`：服务器的端口号  
成员方法：`OutputStream getOutputStream()`：返回此套接字的输出流。`InputStream getInputStream()`：返回此套接字的输入流。`void close()`：关闭此套接字
3. 实现步骤：  
a. 创建一个客户端对象`Socket`，构造方法绑定服务器的IP地址和端口号  
b. 使用`Socket`对象中的方法`getOutputStream()`获取网络字节输出流`OutputStream`对象  
c. 使用网络字节输出流`OutputStream`对象中的方法`write`，给服务器发送数据  
d. 使用`Socket`对象中的方法`getInputStream()`获取网络字节输入流`InputStream`对象  
e. 使用网络字节输入流`InputStream`对象中的方法`read`，读取服务器回写的数据  
4. 注意：  
a. 客户端和服务端进行交互，必须使用`Socket`中提供的网络流，不能使用自己创建的流对象  
b. 创建客户端对象`Socket`的时候，请求服务器和服务器经过3次握手建立连接通路。如果此时服务器没有启动，就会抛出异常。如果服务器已经启动，就可以进行交互
5. `java.net.ServerSocket`：此类实现服务器套接字  
构造方法：`ServerSocket(int port)`：创建绑定到特定端口的服务器套接字。  
成员方法：`Socket accept()`：倾听并接收套接字的连接。服务器端必须明确一件事情，必须知道是哪个客户端请求的服务器。可以使用`accept`方法获取到请求的客户端对象`Socket`。
6. 服务器端的实现步骤：    
a. 创建服务器`ServerSocket`对象和系统要指定的端口号  
b. 使用`ServerSocket`对象中的方法`accept`，获取到请求的客户端对象`Socket`  
c. 使用`Socket`对象中的方法`getInputStream()`获取网络字节输入流`InputStream`对象  
d. 使用网络字节输入流`InputStream`对象中的方法`read`，读取客户端发送的数据  
e. 使用`Socket`对象中的方法`getOutputStream()`获取网络字节输出流`OutputStream`对象  
f. 使用网络字节输出流`OutputStream`对象中的方法`write`，给客户端回写数据  
d. 释放资源（`Socket`，`ServerSocket`）
