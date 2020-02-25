[原链接出处](https://mingyangshang.github.io/2016/03/06/RTMP协议/)

### RTMP( real time message protocol):实时信息传输协议

- 用来解决多媒体数据传输流的多路复用（MultiPlexing）和分包(Packetizing)的问题

- RTMP是应用层协议，是需要靠底层可靠的传输层协议（通常是TCP）来保证信息传输的可靠性的。
- RTMP协议也要客户端和服务器通过”握手“来建立基于传输层链接之上的RTMP Connection链接，在Connection链接上会传输一些控制信息
- RTMP协议传输时会对数据做自己的格式化，这种格式化的消息我们称之为RTMP Message，而实际传输的时候为了更好地实现多路复用、分包和信息的公平性，发送端会把Message划分为带有Message ID的Chunk，每个Chunk可能是一个单独的Message，也可能是Message的一部分，在接受端会根据chunk中包含的data的长度，message id和message的长度把chunk还原成完整的Message，从而实现信息的收发

### RTMP Chunk Stream

- Message:这里的Message是指满足该协议格式的、可以切分成Chunk发送的消息，消息包含的字段如下
  - timeStamp:消息的时间戳, 4个字节
  - length:Message Payload（消息负载）即音视频等信息的数据的长度，3个字节
  - typeId:消息的类型Id，1个字节
  - message Stream ID(消息的流ID):每个消息的唯一标识，划分成Chunk和还原Chunk为Message的时候都是根据这个ID来辨识是否是同一个消息的Chunk的，4个字节，并且以小端格式存储
- chunking(Message 分块)
  - RTMP在收发数据的时候并不是以Message为单位的，而是把Message拆分成Chunk发送，而且必须在一个Chunk发送完成之后才能开始发送下一个Chunk。每个Chunk中带有MessageID代表属于哪个Message，接受端也会按照这个id来将chunk组装成Message。
  - 为什么RTMP要将Message拆分成不同的Chunk呢？通过拆分，数据量较大的Message可以被拆分成较小的“Message”，这样就可以避免优先级低的消息持续发送阻塞优先级高的数据，比如在视频的传输过程中，会包括视频帧，音频帧和RTMP控制信息，如果持续发送音频数据或者控制数据的话可能就会造成视频帧的阻塞，然后就会造成看视频时最烦人的卡顿现象。同时对于数据量较小的Message，可以通过对Chunk Header的字段来压缩信息，从而减少信息的传输量。
  - Chunk的默认大小是128字节，在传输过程中，通过一个叫做Set Chunk Size的控制信息可以设置Chunk数据量的最大值，在发送端和接受端会各自维护一个Chunk Size，可以分别设置这个值来改变自己这一方发送的Chunk的最大大小。大一点的Chunk减少了计算每个chunk的时间从而减少了CPU的占用率，但是它会占用更多的时间在发送上，尤其是在低带宽的网络情况下，很可能会阻塞后面更重要信息的传输。小一点的Chunk可以减少这种阻塞问题，但小的Chunk会引入过多额外的信息（Chunk中的Header），少量多次的传输也可能会造成发送的间断导致不能充分利用高带宽的优势，因此并不适合在高比特率的流中传输。
  - 在实际发送时应对要发送的数据用不同的Chunk Size去尝试，通过抓包分析等手段得出合适的Chunk大小，并且在传输过程中可以根据当前的带宽信息和实际信息的大小动态调整Chunk的大小，从而尽量提高CPU的利用率并减少信息的阻塞机率。
- Chunk Format（块格式）
  - Base Header（基本的头信息）：包含了chunk stream ID（流通道Id）和chunk type（chunk的类型）包含了chunk stream ID（流通道Id）和chunk type（chunk的类型）
  - Message Header(消息的头信息)：包含了要发送的实际信息（可能是完整的，也可能是一部分）的描述信息。Message Header的格式和长度取决于Basic Header的chunk type，共有4种不同的格式
  - Extended Timestamp(扩展时间戳)
  - Chunk Data（块数据）：用户层面上真正想要发送的与协议无关的数据，长度在(0,chunkSize]之间。
- 协议控制消息（Protocol Control  Message）
  - 在RTMP的chunk流会用一些特殊的值来代表协议的控制消息，它们的Message Stream ID必须为0（代表控制流信息），CSID必须为2，Message Type ID可以为1，2，3，5，6，具体代表的消息会在下面依次说明

### 不同类型的RTMP Message

- Command Message(命令消息，Message Type ID＝17或20)：表示在客户端盒服务器间传递的在对端执行某些操作的命令消息
- Data Message（数据消息，Message Type ID＝15或18）：传递一些元数据（MetaData，比如视频名，分辨率等等）或者用户自定义的一些消息
- Shared Object Message(共享消息，Message Type ID＝16或19)：表示一个Flash类型的对象，由键值对的集合组成，用于多客户端，多实例时使用
- Audio Message（音频信息，Message Type ID＝8）：音频数据。
- Video Message（视频信息，Message Type ID＝9）：视频数据。
- Aggregate Message (聚集信息，Message Type ID＝22)：多个RTMP子消息的集合
- User Control Message Events(用户控制消息，Message Type ID=4):告知对方执行该信息中包含的用户控制事件，比如Stream Begin事件告知对方流信息开始传输。