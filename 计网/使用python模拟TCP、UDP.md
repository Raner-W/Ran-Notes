## socket模块
socket又称“套接字”，是应用程序与底层协议栈的桥梁。socket上连应用程序，下连TCP/UDP协议，使开发者无需直接操作底层协议即可实现网络通信<br />
`socket的结构`:`IP地址+端口号`，确保数据能够准确的传输到目标进程<br />
socket分为三类:<br />
- `SOCK_STREAM`:流式套接字,基于TCP<br />
- `SOCK_DGRAM`:数据报套接字，基于UDP<br />
- `SOCK_RAW`:原始套接字，允许应用程序直接访问协议层，用于协议开发，网络嗅探等底层操作<br />
<br />`socket的通信流程`：<br />
- 1. 服务端监听
- 2. 客户端请求
- 3. 连接确认<br />
## 通过TCP收发报文
1. 新建一个 `tcp_server.py`模拟server端
```python
import socket
tcpServer = socket.socket(socket.AF_INET,socket.SOCK_STREAM)  #创建socket对象，AF_INET表示使用IPV4通信
host = socket.gethostname()   #获取本地主机名
port = 12345   #端口号
addr = (host,port)  #设置地址
tcpServer.bind(addr)  #绑定地址
tcpServer.listen(5)  #设置tcp监听，表示最大连接5个，超过需要排队

while True:
    conn,addr = tcpServer.accept()  #建立客户端连接
    print("连接地址：",addr)
    data = conn.recv(1024)  #接收数据
    print(data)
    msg = "收到".encode('utf-8')
    conn.send(msg)
    conn.close()
    
```
2. 新建一个 `tcp_client.py` 模拟client端
```
import socket               
 
tcpClient = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # 创建socket对象
host = socket.gethostname() 
port = 12345 
addr = (host, port)
tcpClient.connect(addr) # 连接服务，指定主机和端口号
data = b'\x01\x64\xff' # 报文数据，bytes类型
tcpClient.send(data) # 发送数据给服务端
msg = tcpClient.recv(1024) # 接收来自服务端的数据，小于1024字节
print(msg.decode('utf-8'))
tcpClient.close()

```
3. 打开两个cmd ， 先运行server端，然后运行client端
4. 模拟udp只需要更改SOCK_STREAM为SOCK_DGRAM，其他基本上没有变动