## 为什么tcp建立链接需要三次握手

### TCP建立链接需要三次握手（three-way handshake）

### 连接的定义：  
  The reliability and flow control mechanisms described above require that TCs initialize and maintain certain status information for each data stream. The combination of this information,including sockets, sequence numbers, and window sizes, is called a connection.  
  [RFC 793 - Transmission Control Protocol](https://tools.ietf.org/html/rfc793)  
  用于保证可靠性和流控制机制的信息，包括socket、序列号以及窗口大小叫做连接。  
  建立TCP连接需要通信双方对socket、sequence numbers, window sizes达成共识。

### 为什么需要通过三次握手才可以初始化Sockets、窗口大小、初始序列号并建立TCP链接
- 通过三次握手才能阻止重复历史连接的初始化。
- 通过三次握手才能对通信双方的初始序列号进行初始化。

### 历史连接
TCP连接使用三次握手的首要原因 - 为了阻止历史的重复连接初始化造成的混乱问题，防止TCP协议通信双方建立了错误的连接。  
The priciple reason for the three-way handshake is to prevent old duplicate connection initiations from causing confusion.
