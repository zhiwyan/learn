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

如果通信双方的通信次数只有两次，发送方一旦发出建立连接的请求之后就没有办法撤回请求，在网络状况复杂或者较差的网络中，发送方连续发送多次建立连接请求，接收方只能选择接收或者拒绝发送方发起的请求，接收方并不清楚这一次请求是不是由于网络拥堵而早早过期的连接。　　

TCP选择使用三次握手来建立连接并在连接引入RST这一控制信息，接收方当收到请求时会将发送方发来的SEQ+1发送给对方，这时由发送方来判断当前连接是否是历史连接：
- 如果当前连接是历史连接，即SEQ过期或者超时，那么发送方就会直接发送RST控制消息中止这一次连接。
- 如果当前连接不是历史连接，发送方就会发送ACK控制信息，通信双方就会成功建立连接

使用三次握手和RST控制消息将是否建立来接的最终控制权交给发送方，因为只有发送方有足够的上下文来判断当前连接是否是错误或者过期的，这也是TCP使用三次握手建立连接的最主要的原因

### 初始序列号
