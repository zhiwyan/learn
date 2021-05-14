## channel

### channel使用注意

- channel是无序的
- channel关闭后，不能再向channel写入数据 （会panic），但可以从channel中读数据
- 无缓冲的channel如果没有写入或者写入了但是没有接的会一直阻塞
- 有缓冲的channel缓冲写满了，或者读空了才会阻塞。

### make chan

```go
type hchan struct {
    qcount   uint           // total data in the queue
    dataqsiz uint           // size of the circular queue
    buf      unsafe.Pointer // points to an array of dataqsiz elements
    elemsize uint16
    closed   uint32
    elemtype *_type // element type
    sendx    uint   // send index
    recvx    uint   // receive index
    recvq    waitq  // list of recv waiters
    sendq    waitq  // list of send waiters

    // lock protects all fields in hchan, as well as several
    // fields in sudogs blocked on this channel.
    //
    // Do not change another G's status while holding this lock
    // (in particular, do not ready a G), as this can deadlock
    // with stack shrinking.
    lock mutex
}
```

make函数在创建channel的时候会在该进程的heap区申请一块内存，创建一个hchan结构体，返回该内存的指针。

### 发送和接收

向channel发送数据和从channel接收数据主要涉及hchan里的4个成员变量，buf、sendx、recvx、lock。

- 初始的时候hcan结构体重的buf为空，sendx和recvx都为0，
- 当向channel中发送数据的时候，首先会对buf加锁，然后将要发送的数据copy到buf里，并增加sendx的值，然后释放buf的锁。
- 从channel中取数据的时候，也首先会锁，然后将buf里的数据copy到变量对应的内存中，增加recvx的值，最后释放锁。

### 写入满channel的场景

- 当goroutine向buf已经满了的channel发送数据。
  - 通知调度器，将这个goroutine的状态设置为waiting，移除与线程M的联系，然后从P的本地队列中选择一个goroutine在线程M运行，此时之前的goroutine是阻塞状态，但不是操作系统的线程阻塞，所以这个时候只用消耗少量的资源。
  - 创建一个sudog然后保存进入sendq队列。sudog持有G代表goroutine对象引用，elem代表channel里面保存的元素
- 此时如果有goroutine从channel读数据。会发生什么
  - 从channel中的buf读取一个数据。
  - 从sendq队列中pop出一个sudog；
  - 将sudog中elem保存的元素复制到buf中已经读取了元素的位置，然后更新sendx和recvx索引值；
  - 让后通知调度器，将往channel写数据的goroutine设置为runable。

### 读取空channel的场景

- 当channel的buffer里面的数据为空时，这时候channel发生了读取操作。
  - 通知调度器，将这个goroutine的状态设置为waiting，让出os线程，进入阻塞状态
  - 创建一个sodog然后保存到recvq队列
- 此时如果有goroutine往channel写数据。会发生什么
  - 直接把数据写入到 读goroutine的sudog中的elem中

