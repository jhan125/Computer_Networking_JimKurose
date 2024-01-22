# Chapter 2. Application Layer

# Table of Contents
- [Chapter 2. Application Layer](#chapter-2-application-layer)
- [Table of Contents](#table-of-contents)
	- [Section 1. Principles of Network Applications](#section-1-principles-of-network-applications)
		- [Notes](#notes)
			- [Client-Server Paradigm](#client-server-paradigm)
			- [Peer-to-peer architecture](#peer-to-peer-architecture)
			- [Socket](#socket)
		- [Review Questions](#review-questions)
	- [Section 2. The Web and HTTP](#section-2-the-web-and-http)
		- [Notes](#notes-1)
			- [HTTP: hypertext transfer protocol](#http-hypertext-transfer-protocol)
			- [Cookies](#cookies)
			- [Web caches 缓存 (aka proxy server 代理服务器)](#web-caches-缓存-aka-proxy-server-代理服务器)
			- [Conditional GET](#conditional-get)
			- [总结](#总结)
			- [HTTP/2 和 HTTP/3](#http2-和-http3)
	- [Section 3. Email, SMTP, IMAP](#section-3-email-smtp-imap)
		- [Notes](#notes-2)
		- [Review Questions](#review-questions-1)
	- [Section 4. The Domain Name Service: DNS](#section-4-the-domain-name-service-dns)
		- [Notes](#notes-3)
		- [Review Questions](#review-questions-2)
	- [Section 5. Peer-to-Peer File Distribution](#section-5-peer-to-peer-file-distribution)
		- [Notes](#notes-4)
		- [Review Questions](#review-questions-3)
	- [Section 6. Video Streaming and Content Distribution Networks](#section-6-video-streaming-and-content-distribution-networks)
		- [Notes](#notes-5)
		- [Review Questions](#review-questions-4)
	- [Section 7. Socket Programming-Creating Network Applications](#section-7-socket-programming-creating-network-applications)
		- [Notes](#notes-6)
	- [Section 8. Supplemental Topics](#section-8-supplemental-topics)
		- [Notes](#notes-7)
		- [Review Questions](#review-questions-5)
   
## Section 1. Principles of Network Applications

### Notes
- There is a difference between network architecture and application architecture.
- The predominant architectural paradigms are:
	- Client-Server Architecture
	- Peer-to-Peer (P2P) Architecture
- In the client-server architecture:
	- An always-on host with fixed ip address which responding to client's requests.
	- There is no way to make two clients talk to each other.
- In P2P architecture:
	- There is not reliance on a dedicated server.
	- The users share the content in a highly decentralized network.
- Processes on two different end systems communicate with each other by exchanging messages across the computer network.
	- processes communicating with each other reside in the application layer of the five-layer protocol stack.
	- client process: process that initiates communication
	- server process: process that waits to be contacted
- A socket is the interface between the application layer and the transport layer within a host.
- The only control that the application developer has on the transport-layer side is:
	- the choice of transport protocol
	- perhaps the ability to fix a few transport-layer parameters such as maximum buffer and maximum segment sizes
- To identify the receiving process, two pieces of information need to be specified:
	- the address of the host
	- an identifier that specifies the receiving process in the destination host.
- An application-layer protocol defines:
    - types of messages exchanged,e.g., request, response
    - message syntax: what fields in messages & how fields are delineated
    - message semantics: meaning of information in fields
    - rules for when and how processes send & respond to messages
    - open protocols: defined in RFCs, everyone has access to protocol definition; allows for interoperability; e.g., HTTP, SMTP
    - proprietary protocols: e.g., Skype, Zoom

- The transport-layer protocol provides services that can be categorized into:
	- data integrity: reliable data transfer
	- throughput
	- timing
	- security
- The Internet (TCP/IP networks) makes two transport protocols available to applications, UDP and TCP.
- The TCP service model includes a connection-oriented service and a reliable data transfer service.
	- reliable transport between sending and receiving process
	- flow control: sender won’t overwhelm receiver
	- congestion control: throttle sender when network overloaded
	- connection-oriented: setup required between client and server processes
	- does not provide: timing, minimum throughput guarantee, security
- UDP is a lightweight transport protocol, connectless.
	- It provides unreliable data transfer service.
	- does not provide: reliability, flow control, congestion control, timing, throughput guarantee, security, or connection setup.
	- but can be used to build upon
- Transport Layer Security (TLS) is an application layer enhancement for the TCP layer which provides data integrity, encryption, and end-point authentication.
- An application-layer protocol defines how an application's processes, running on different end systems, pass messages to each other.
	- The type of messages.
	- The syntax of the various message types.
	- The semantics of the fields.
	- Rules for determining when and how a process sends messages and responds to messages.

#### Client-Server Paradigm
<img width="1012" alt="Screenshot 2024-01-22 at 8 43 11 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/e5a1228a-f8a4-4a7f-a87e-2fbdfe418b00">

#### Peer-to-peer architecture
<img width="1003" alt="Screenshot 2024-01-22 at 8 43 23 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/f7e62575-507b-4432-be95-380e87fa0d73">

#### Socket
![image](https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/fd38d94b-d9ba-47ce-84dc-b3b5229c99a1)
套接字就像两个通信的程序之间的门。我们用一些简单的比喻来理解：

想象你的计算机是一栋大房子，而里面的程序就像是住在房子里的人。这些程序有时候需要和外面的世界（也就是互联网上的其他程序）通信。他们是怎么做到的呢？他们用的是窗户或者门，也就是我们这里说的“套接字”。

当一个程序想要发送信息时，它会把信息放到这个“门”上，就像你把一封信丢进邮箱一样。这个“门”依赖于交通基础设施，也就是网络，确保信能够安全送达。就像邮递员确保你的信能够送到对方的家里。在对面，也就是信息接收方的房子上，也有一个这样的“门”来接收信息。

这个图里还有一些细节：
左边是发送信息的程序和它的“门”（套接字）。
右边是接收信息的程序和它的“门”（套接字）。
两个程序都在自己的层次结构内操作，这个结构包括应用层（由程序开发者控制）和更低层次（由操作系统控制）。

总之，套接字是计算机程序用来互相发送和接收信息的一种方式，就像房子的门一样，让信息可以从一个程序传送到另一个程序。

**应用：**
1. 网页浏览： 当你在浏览器中输入一个网址时，你的浏览器（客户端程序）会使用套接字与网站的服务器进行通信，请求网页的内容。
2. 在线游戏： 如果你在玩一个多人在线游戏，你的电脑上的游戏程序会通过套接字与游戏服务器或者其他玩家的电脑通信，以同步游戏状态。
3. 聊天应用： 像QQ或微信这样的聊天程序使用套接字来发送和接收即时消息。
4. 文件传输： 如果你需要通过网络向另一台电脑发送文件，如使用FTP程序，它会创建套接字来管理文件的上传或下载。
5. 流媒体： 观看在线视频或听在线音乐时，流媒体服务也是通过套接字来传输视频或音频数据。
6. 远程登录： 使用远程桌面或SSH客户端登录到另一台电脑时，也会用到套接字来建立连接并传输指令和数据。

**用一些日常生活中的比喻来解释为什么需要套接字：**

1. 统一的通信接口： 就像不同的电器需要插座来接通电源一样，不同的计算机程序需要套接字来进行通信。套接字提供了一个统一的接口，允许软件开发者不用深入底层网络细节就能够发送和接收数据。

2. 支持不同的网络协议： 套接字支持多种类型的网络协议，比如TCP和UDP，就像不同的电话可以通过不同的网络（如4G或Wi-Fi）来通话。这让程序可以根据需要选择最合适的通信方式。

3. 隔离复杂性： 套接字帮助隔离了网络通信的复杂性，使开发者不必关心如何在网络层面传输数据包，就像人们使用手机聊天时不需要了解背后的移动通信技术细节。

4. 支持并发连接： 套接字允许一台计算机上的多个程序同时使用网络进行通信，就像一个大楼中有多个窗户，每个窗户都可以用来与外界交流。

5. 端到端的连接： 套接字让两个程序能够建立一个点对点的连接，这意味着数据可以直接从一个程序传送到另一个程序，就像邮件直接从发件人寄到收件人，而不需要打开查看。

6. 抽象化硬件和操作系统差异： 不同操作系统和硬件之间可能有很多差异，但是套接字提供了一个通用的接口，使得在不同系统和硬件之间的通信变得容易，就像电源适配器允许不同插头的电器在世界各地使用。


### Review Questions
- List five nonproprietary Internet applications and the application-layer protocols that they use.
	- Web: HTTP
	- E-mail: SMTP
	- Multimedia Streaming: RTP
	- Remote access: TELNET
	- File transfer: FTP
- What is the difference between network architecture and application architecture?
	- From application's developer point of view, network architecture is fixed and provides certain and well defined services to the application. On the other hand, the application architecture is defined by the application developer based on the nature of the problem being solved and other considerations.
- For a communication session between a pair of processes, which process is the client and which is the server?
	- The client is considered the one who started the converstation first in this session. While the server is considered the one who has the role to respond to this conversation.
- For a P2P file-sharing application, do you agree with the statement, “There is no notion of client and server sides of a communication session”? Why or why not?
	- No. As stated in the text, all communication sessions have a client side and a server side. In a P2P file-sharing application, the peer that is receiving a file is typically the client and the peer that is sending the file is typically the server.
- What information is used by a process running on one host to identify a process running on another host?
	- It uses the information provided by the socket.
- Suppose you wanted to do a transaction from a remote client to a server as fast as possible. Would you use UDP or TCP? Why?
	- It depends on the type of the transaction as the speed is not the only limitation. If this transaction should be gruanteed to reach the other end, then the best choice is TCP. But if not, then UDP is good.
- Referring to Figure 2.4, we see that none of the applications listed in Figure 2.4 requires both no data loss and timing. Can you conceive of an application that requires no data loss and that is also highly time-sensitive?
	- network games
	- network multimedia applications with high quality requirements
	- controlling hight sensitive devices over a network
- List the four broad classes of services that a transport protocol can provide. For each of the service classes, indicate if either UDP or TCP (or both) provides such a service.
	- Reliable data transfer: TCP
	- Timing: None
	- Security: None
	- Throughput: None
- Recall that TCP can be enhanced with TLS to provide process-to-process security services, including encryption. Does TLS operate at the transport layer or the application layer? If the application developer wants TCP to be enhanced with TLS, what does the developer have to do?
	- It operated in the application layer.
	- The developer should include TSL code in order to use.
  
## Section 2. The Web and HTTP

### Notes

#### HTTP: hypertext transfer protocol

-  Web’s application-layer protocol
-  client/server model
   -  client: browser that requests, receives, (using HTTP protocol) and “displays” Web objects
   -  server: Web server sends (using HTTP protocol) objects in response to requests
- HTTP uses TCP:
  - client initiates TCP connection (creates socket) to server, port 80
  - server accepts TCP connection from client
  - HTTP messages (application-layer protocol messages) exchanged between browser (HTTP client) and Web server (HTTP server)
  - TCP connection closed
- HTTP is “stateless”
  - server maintains no information about past client requests

***HTTP connections: two types*** 
Non-persistent HTTP + Persistent HTTP(most common form used today)

- Non-persistent HTTP Issues:
  - requires 2 RTTs per object
  - OS overhead for each TCP connection
  - browsers often open multiple parallel TCP connections to fetch referenced objects in parallel
  - Response time: 2RTTs + file transmission time (RTT is the time for a small packet to travel from client to server and back)
  
- Persistent HTTP (HTTP1.1) Issues: 
  - server leaves connection open after sending response
  - subsequent HTTP messages between same client/server sent over open connection
  - client sends requests as soon as it encounters a referenced object
  - as little as one RTT for all the referenced objects (cutting response time in half)

***HTTP Request & Response***
<img width="860" alt="Screenshot 2024-01-22 at 8 58 57 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/9c40e18f-d778-4387-8aff-7ffa0ad90e1a">

HTTP Request Messages:
- ASCII (human-readable format)

1) request line (GET, POST, HEAD commands) 
   `e.g. GET/index.html`
2) header lines include 'Host' 'User-Agent' 'Accept' 'Accept-Language' 'Accept-Encoding' 'Connection'
3) carriage return, line feed at start of line indicates end of header lines 

<img width="1022" alt="Screenshot 2024-01-22 at 8 59 52 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/f496619a-a286-484d-b5f3-915751f69dab">

HTTP Response status code:

1) status line (protocol status code status phrase) 
   `e.g. HTTP/1.1 200 OK`
2) header lines include 'Date' 'Server' 'Last Modified' 'ETag' 'Accept-Ranges' 'Content-Length' 'Content-Type'
3) body: data, e.g. requested HTML file

<img width="954" alt="Screenshot 2024-01-22 at 9 00 54 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/3039c268-8ff5-406d-a750-70dfe8a0e4e8">

#### Cookies
Maintaining user/server state: Web sites and client browser use cookies to maintain some state between transactions

**four components**
1) cookie header line of HTTP response message
2) cookie header line in next HTTP request message
3) cookie file kept on user’s host, managed by user’s browser
4) back-end database at Web site

<img width="926" alt="Screenshot 2024-01-22 at 9 04 43 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/3da3e442-5284-4c77-bc3b-fd2acb2257ad">

What cookies can be used for:
- authorization
- shopping carts
- recommendations
- user session state (Web e-mail)

Challenge: How to keep state?
- at protocol endpoints: maintain state at sender/receiver over multiple transactions
- in messages: cookies in HTTP messages carry state

cookies and privacy:
- cookies permit sites to learn a lot about you on their site.
- third party persistent cookies (tracking cookies) allow common identity (cookie value) to be tracked across multiple web sites

#### Web caches 缓存 (aka proxy server 代理服务器)

satisfy client requests without involving origin server

1. user configures browser to point to a (local) Web cache
2.  browser sends all HTTP requests to cache
    - if object in cache: cache returns object to client
    - else cache requests object from origin server, caches received object, then returns object to client

Why Web caching?
- reduce response time for client request
  - cache is closer to client
- reduce traffic on an institution’s access link
- Internet is dense with caches
  - enables “poor” content providers to more effectively deliver content

Improve delay caused by large queueing at high utilization:
1) buy a faster access link (more cost)
2) install a web cache (cheaper! see quantify process in slides)

**什么是web缓存**

想象一下，你每天放学后都要到一个位于城市另一端的商店去买你最喜欢的漫画书。这趟旅程可能会很长，因为商店离你家很远。但如果在你家附近就有一个小书摊，而这个小书摊每天都从那个远处的商店里拿来几本最新的漫画书放在这里卖，那你就不用走那么远了，可以很快就买到漫画书。

Web缓存就像是那个离你家很近的小书摊。它保存了你经常访问的网站的信息，这样当你下次再访问这个网站时，你的电脑就可以直接从离你更近的缓存服务器上获取信息，而不是每次都从远处的主服务器上下载。这不仅节省了你的时间，也减轻了网络上的交通——就像减少了大家都到同一个远处商店买书造成的拥堵。

**Web缓存如何工作**

当你在浏览器里输入一个网址，你的电脑会向缓存服务器发出请求，看看它是否有这个网站最新的信息。如果有，缓存服务器就会直接把这些信息发送给你的电脑。如果没有，缓存服务器会代替你的电脑去向远处的主服务器请求信息。得到信息后，缓存服务器既会把它发送给你的电脑，也会把它保存下来，这样下次别人或者你再请求同样的网站时就能直接从缓存服务器上获取了。

缓存服务器也会记得每份信息应该保存多久。有些信息会经常更新，像新闻网站的首页，而有些信息不常变，像一些教学视频。缓存服务器会根据这些规则决定什么时候去主服务器上更新信息。

通过这样的方式，Web缓存帮助我们更快地获取信息，同时也让整个互联网的运行更加顺畅。

比喻：

“在高利用率时会有大的排队延迟”，这就像是当图书馆入口人太多时，学生们要排很长时间的队才能进入，这样就耽误了他们借书和学习的时间。当很多人都在用同一条网络链路时（比如很多学生都想通过图书馆的入口），如果网络链路（图书馆的入口）快要达到最大容量时，大家就可能要等待更长的时间来传输数据（或者进入图书馆）。所以，图书馆需要有更多/更快的入口（faster access link）或者采用更有效的管理借书流程(use web cache)一样。

#### Conditional GET

don't send object if cache has up-to-date cached version

“有条件的GET请求”的电脑网络技术，它和图书馆借书的规则有点相似：

想象一下，你上次去图书馆借了一本书，然后回家之后把它复印了一份。下次你想看这本书时，你不一定非得再去图书馆，你可以直接看你复印的那份。但是，如果书有了新的内容（比如新的章节），你复印的版本就不是最新的了。所以，你可能会对图书管理员说：“如果这本书在我上次借阅后有更新，请告诉我，我就需要一个新的副本。”

<img width="1004" alt="Screenshot 2024-01-22 at 9 49 13 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/47b5a2be-6f08-4c0e-9ed6-722237956ff1">

在这张图中：

1. 目标：不发送对象，如果缓存中已经有了最新版本。
- 没有对象传输延迟（或使用网络资源）意味着如果你的复印版是最新的，你就不用等待去图书馆借书了。
客户端：就是你的电脑，你告诉服务器（就像告诉图书管理员），如果自从某个日期后这本书有更新，你就想要下载新的内容。

2. 服务器：就是图书馆的管理员，它会告诉你的电脑书有没有更新。
- 如果没有更新（没有修改），它就会回复“304 Not Modified”（没有修改），这就像图书管理员说：“自从你上次来之后，书没有新的章节。”
- 如果有更新，服务器就会回复“200 OK”和更新的内容（数据），就好像图书管理员给了你一本更新后的书。

**跟Web缓存的关系**

就像你不需要每次都去图书馆检查书有没有更新一样，你的电脑也可以使用有条件的GET来检查网上的信息有没有更新。如果没有，它就可以使用之前保存的版本，这样就不用每次都下载相同的信息了。这节省了时间也减少了网络上的流量，因为减少了重复的下载。这就是缓存的作用——存储信息，只在需要的时候更新。

#### 总结

比较一下Web缓存、Cookies和Conditional GET：

**Web缓存**
想象你的学校有一个资料室，它保存了很多常用的学习资料。当你需要某些资料时，你先去资料室看看，如果有，就不用去图书馆找了，这样既快又省力。Web缓存就像这个资料室，它保存了你经常查看的网页，所以当你下次访问这些网页时，你的电脑可以直接从这个“资料室”中快速拿到信息，而不是每次都去网站上重新加载。

**Cookies**
Cookies则像是学校发给你的一个小本子。每次你去资料室，管理员会在本子上记录你借了什么资料，下次你再来的时候，管理员就能看到你之前的记录，知道你喜欢什么，就可以快速帮你找到类似的资料。在网上，Cookies帮助网站记住你的信息（比如你登录状态、偏好设置等），这样网站就可以记得你是谁，并给你个性化的服务。

**Conditional GET**
Conditional GET就像你带着上次借的书回到资料室，然后问管理员：“这本书最近有更新吗？”如果管理员说“没有”，你就可以继续使用你的旧书。如果管理员说“有更新”，他就会给你一本新的书。在网上，这个过程可以帮助你的电脑决定是否需要下载网页的新版本，如果网页自从你上次访问后没有改变，你的电脑就可以使用缓存中的老版本，这样可以节省时间和网络流量。

总的来说，Web缓存是用来保存你经常访问的网页，以便快速加载；Cookies是用来保存你的个人偏好和登录信息，以便网站可以个性化你的体验；Conditional GET是一种检查方法，用来看你的缓存中的网页是否还是最新的，如果是，就不用下载新的网页了。三者都是为了让你上网时更加方便快捷。

#### HTTP/2 和 HTTP/3

**HTTP1.1** 
introduced multiple, pipelined GETs over single TCP connection
-  server responds in-order (FCFS: first-come-first-served scheduling) to GET requests
-  with FCFS, small object may have to wait for transmission (head-of-line (HOL) blocking) behind large object(s)
-  loss recovery (retransmitting lost TCP segments) stalls object transmission

**HOL是什么**

HOL代表“Head-Of-Line Blocking”。

想象你在学校的食堂排队等着买午饭，每个学生都要点自己的饭菜。如果前面的第一个学生（队伍的头部）花了很长时间来决定吃什么，或者他点的饭菜需要很长时间准备，那么即使后面的学生都知道自己要什么，他们也得等那个学生完成点餐并且拿到饭菜之后才能轮到自己。这就是“排头阻塞”，因为排在最前面的人或事物导致了后面所有人或事物的延迟。

在计算机网络中，特别是在旧的HTTP/1.1协议里，HOL Blocking是指当多个请求通过单个TCP（传输控制协议）连接传输时，如果第一个请求被延迟（可能因为丢包或者长时间处理），那么即使后面的请求已经准备好了，它们也必须等待，因为TCP只能按顺序发送数据。这样就导致了效率低下，因为一次只能处理一个请求，后面的请求即使准备好了也必须等待。

**HTTP/2**
Key goal: decreased delay in multi-object HTTP requests

- increased flexibility at server in sending objects to client:
  - methods, status codes, most header fields unchanged from HTTP 1.1
  - transmission order of requested objects based on client-specified object priority (not necessarily FCFS)
  - push unrequested objects to client
  - divide objects into frames, schedule frames to mitigate HOL blocking
- HTTP/2 over single TCP connection means:
  - recovery from packet loss still stalls all object transmissions as in HTTP 1.1, browsers have incentive to open multiple parallel
  - TCP connections to reduce stalling, increase overall throughput no security over vanilla TCP connection

HTTP/2对此进行了优化，采用了多路复用技术。还是用排队买午饭的例子来说明：

如果说HTTP/1.1是每个学生排成一队等待点餐，HTTP/2就像是有一个超级能力的食堂管理员，他可以同时接受多个学生的订单，并且同时开始准备。这个管理员有很多手，可以同时做很多事情，不需要等一个学生的饭做完才开始做下一个的。

在HTTP/2中，数据被分割成许多小的“帧Frame”，这些帧分属于不同的“流”，每个流对应一个请求。所有的帧都在同一个TCP连接上传输，但是它们可以独立处理，所以不会像HTTP/1.1那样互相阻塞。如果一个流中的帧被延迟了，其他流中的帧仍然可以被发送和接收，这就减少了延迟。

这样的设计大大减少了HOL Blocking的问题，因为即使某个请求被阻塞，其他请求仍然可以继续进行，这就像那个超级能力的食堂管理员能确保所有学生的饭菜都在同时准备，没有人需要不必要的等待。

**HTTP/3**

adds security, per object error- and congestion control (more pipelining) over UDP
- more on HTTP/3 in transport layer

如果说HTTP/2是一个有多个手的超级食堂管理员，他可以同时接受和准备多个学生的订单，那么HTTP/3就像是食堂进行了一次技术革新，现在有了一个先进的点餐系统。这个新系统（HTTP/3）不再依赖一个食堂管理员，而是有很多个机器人管理员，每个都有自己的厨房。这样，即使一个机器人在制作一个复杂的菜肴时出了问题，其他机器人还可以继续他们的工作，不会影响整个食堂的效率。在HTTP/2中，如果超级食堂管理员在准备一个大订单时遇到问题，那么即使其他订单已经准备好了，也得等那个大订单完成。

HTTP/3采用了一种叫做QUIC的新网络协议，它在传输数据时使用了一种不同的方式，让数据传输更加高效和可靠。它不像之前的版本那样在传输过程中会遇到的一些问题，比如网络变化导致的连接中断，或者数据包丢失导致整个数据流停止。

回到食堂例子，这就好比每个机器人管理员都有自己的通讯设备，即使他们中的一个通讯设备出了问题，其他的机器人还是可以继续接收订单和制作食物，不会被影响。

所以，总的来说，HTTP/3通过使用多个独立的机器人管理员（类似于多个独立的连接），每个都能处理自己的任务，即使其中一个遇到问题，也不会影响到其他的，这样就减少了等待时间，提高了整个系统的效率。


## Section 3. Email, SMTP, IMAP

### Notes
 - To send a message from source end system to a destination end system, the source breaks long messages into smaller chunks of data known as packets.
 - Packet switches implement the store-and-forward mechanism.
	- That means the packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link.
 - Propagation delay is the time takes for the bits to travel across the wire near the speed of light.
 - End-to-end delay equals: N*(L/R)
	- N: number of links.
	- R: rate of each link
	- L: packet length
 - Each packet switch has multiple links attached to it.
	- For each attached link, the packet switch has an output buffer, which stores packets that the router is about to send into that link.
 - Packets suffer from delaying due to store-and-forward and queuing.
 - Packet loss occurs when there is a congestion in the network which results in the filling of the output buffer.
 - Each router has a forwarding table that maps destination addresses to the router's outbound links.
  
### Review Questions
 - Suppose there is exactly one packet switch between a sending host and a receiving host. The transmission rates between the sending host and the switch and between the switch and the receiving host are R1 and R2, respectively. Assuming that the switch uses store-and-forward packet switching, what is the total end-to-end delay to send a packet of length L? (Ignore queuing, propagation delay, and processing delay.)
	 - (L/R1)+(L/R2)
 - What advantage does a circuit-switched network have over a packet switched network? What advantages does TDM have over FDM in a circuit-switched network?
	 - Circuit-switched networks are well suitable for real time servicessuch as voice calls and video calls whereas packet-switched network are not suitable for real time services. They are suitable for handling data.
	 - In circuit-switched networks, the transmission link is pre-allocated without taking into consideration the demand whereas packet-switched network allocates transmission link on demand.
	 - In circuit-switched network, the bandwidth is reserved and so packets arrive within the bandwidth whereas in packet-switched network, the bandwidth is not reserved and so the packets may have to wait for their turn to be forwarded.
	 - In time division multiplexing, all connections operate with same frequency at different times where as in frequency division multiplexing, all connections operate with different frequencies at the same time.
	 - In TDM, when the network establishes a connection across a link, the network dedicates one time slot in every frame to the connection which is used only for that connection.
 -  Suppose users share a 2 Mbps link. Also suppose each user transmits continuously at 1 Mbps when transmitting, but each user transmits only 20 percent of the time.
	 - When circuit switching is used, how many users can be supported?
		 - 2 users can be supported because each user requires half of the link bandwidth.
	 - For the remainder of this problem, suppose packet switching is used. Why will there be essentially no queuing delay before the link if two or fewer users transmit at the same time? Why will there be a queuing delay if three users transmit at the same time?
		 - Since each user requires 1Mbps when transmitting, if two or fewer users transmit simultaneously, a maximum of 2Mbps will be required. Since the available bandwidth of the shared link is 2Mbps, there will be no queuing delay before the link. Whereas, if three users transmit simultaneously, the bandwidth required will be 3Mbps which is more than the available bandwidth of the shared link. In this case, there will be queuing delay before the link.
	 - Find the probability that a given user is transmitting.
		 - 0.008
	 - Suppose now there are three users. Find the probability that at any given time, all three users are transmitting simultaneously. Find the fraction of time during which the queue grows.
		 - Since the queue grows when all the users are transmitting, the fraction of time during which the queue grows (which is equal to the probability that all three users are transmitting simultaneously) is 0.008.
 - Why will two ISPs at the same level of the hierarchy often peer with each other? How does an IXP earn money?
	 - By peering with each other, two ISP’s can reduce their cost and avoid paying to the intermediate ISP provider.
	- An Internet Exchange Points (IXP) can earn money by charging each ISP that connects to it. The IXP charges each ISP based on the amount of traffic sent to or received from the IXP.
 -  Some content providers have created their own networks. Describe Google’s network. What motivates content providers to create these networks?
	- Google’s network: 
		- This network provides global data. 
		- It is used to transfer content within the Google servers. 
		- Its contains some Tier-1 ISP and interconnect with TCP/IP.
	- Motivates: 
		- It is used to save money by transfer data and less time to travel content. 
		- Content providers to control over the services.

## Section 4. The Domain Name Service: DNS

### Notes

- **How do packet delay and loss occur?**
  - packets queue in router buffers, waiting for turn for transmission
  - queue length grows when arrival rate to link (temporarily) exceeds output link
capacity
  - packet loss occurs when memory to hold queued packets fills up

**4 sources that cause packet delays:**
- Nodal Processing Delay
  - check bit errors, determine output link, typically < microsecs
  - The time required to examine the packet's header and determine where to direct the packet.
  - This delay is typically on the order of microseconds or less.
- Queuing Delay
  - time waiting at output link for transmission, depends on congestion level of router
  - At the queue, the packet experiences a delay as it waits to be transmitted onto the link.
  - This delay can be on the order of microseconds to milliseconds.
- Transmission Delay
  - L: packet length(bits), R: link transmission rate(bps), d(trans) = L/R
  - The amount of time required to push all of the packet's bits into the link.
  - This delay is on the order of microseconds to milliseconds.
- Propagation Delay
  - d: length of physical link, s: propagation speed, d(prop) = d/s
  - The time required to propagate from the beginning of the link to the next router.
  - The speed depends on the link physical medium.
  - In WAN, this delay is on the order of milliseconds.

d(nodal) = d(proc) + d(queue) + d(trans) + d(prop)

- **d(trans) and d(prop) very different**

  - Transmission（传输）指的是数据从源头（例如计算机A）发送到网络中的行为。这一过程涉及到将数据编码成信号（比如电信号、光信号），然后通过网络连接（如以太网线缆、光纤或无线电波）发送出去。在这个阶段，重点是数据的“输出”，即数据是如何从源头发送出去的。

  - Propagation（传播）则指的是这些编码过的信号在物理媒介（如网络电缆、光纤或空气）中移动的过程。在这个阶段，信号正在从一个地点传输到另一个地点，比如从一个路由器传输到另一个路由器。这个过程主要涉及到信号的移动速度，这通常由媒介的物理属性和信号的传播距离决定。

  - 简单来说，transmission是关于数据如何被发送的，而propagation是关于一旦数据被发送，它如何通过网络媒介移动。两者都是数据通信过程中的重要部分，但关注的重点不同。

**Traffic intensity**

Traffic intensity is the rate of (arrival rate of bits) and (service rate of bits)

	- let, a: average packet arrival rate, L: packet length (bits), R: link bandwidth (bit transmission rate).
	- Traffic Intensity = L.a/R
	- La/R ~ 0: avg. queueing delay small
	- La/R -> 1: avg. queueing delay large
	- La/R > 1: more “work” arriving  is more than can be serviced -  average delay infinite!
- The fraction of lost packets increases as the traffic intensity increases.
- Performance at a node is often measured not only in terms of delay, but also in terms of the probability of packet loss.

traceroute program: 

provides delay measurement from source to router along end-end Internet path towards destination.

**Packet Loss**
- queue (aka buffer) preceding link in buffer has finite capacity
- packet arriving to full queue dropped (aka lost)
- lost packet may be retransmitted by previous node, by source end system, or not at all

**Throughput（吞吐量）**

rate (bits/time unit) at which bits are being sent from sender to receiver.
	- instantaneous: rate at given point in time.
	- average: rate over longer period of time.

用一些简单的比喻来理解这个概念：

想象一下有一根水管，水管的一端连接着水桶（服务器），另一端连接着水杯（计算机）。水桶里装的不是水，而是数据，我们可以称它为“数据流”。

吞吐量（Throughput）：这就像是水桶向水杯里倒水的速度，用“多少升/每秒”来衡量，但在我们的例子中，我们用“多少比特/每秒”来衡量数据的传输速度。这个速度就是从水桶（服务器）到水杯（计算机）的数据传输速度。

吞吐量有两种不同的类型：

- 瞬时吞吐量（Instantaneous）：这就像你突然看了一下水桶倒水的速度，就在那一瞬间的速度，可能是因为水桶刚倒的时候水流特别快，或者水快倒完时变慢了。

- 平均吞吐量（Average）：这不是看一瞬间的速度，而是你看了一段时间，比如一分钟，然后计算这一分钟内平均的倒水速度是多少。这就像是我们不仅仅看水桶开始倒水的速度，还要看整个过程中的平均速度。

水管能够承载的数据流的速率有两个表示：
- R_s（server）：这是水桶（服务器）能够向水管里倒水的速率，或者说是服务器能发送数据的速率。
- R_c（client）：这是水管能够承载的最大速率，就是水管最多能允许多快的水流通过，不会溢出来。在计算机网络中，这可以理解为网络的最大传输能力。

bottleneck link: link on end-end path that constrains end-end throughput.

“瓶颈链路”意思是从一端传到另一端的平均吞吐量，要么是由服务器的输出速率决定的，要么是由网络中最慢的连接速率决定的，这取决于哪个速率更慢。这就像是水流从水桶流向水杯的过程，要么是水桶倒水的速度慢，要么是水管狭窄导致水流不畅。

### Review Questions
- Consider sending a packet from a source host to a destination host over a fixed route. List the delay components in the end-to-end delay. Which of these delays are constant and which are variable?
	- Nodal Processing Delay (Constant).
	- Queuing Delay (Variable).
	- Transmission Delay (Constant).
	- Propagation Delay (Constant).
  
-  How long does it take a packet of length 1,000 bytes to propagate over a link of distance 2,500 km, propagation speed 2.5 * 10^8 m/s, and transmission rate 2 Mbps? More generally, how long does it take a packet of length L to propagate over a link of distance d, propagation speed s, and transmission rate R bps? Does this delay depend on packet length? Does this delay depend on transmission rate?
	- The propagation delay is the ratio between the distance and the speed which is numerically equal to 0.01 seconds.
	- This numerical value doesn't depend on the length of the packet or the transmission rate.

- Suppose Host A wants to send a large file to Host B. The path from Host A to Host B has three links, of rates R1 = 500 kbps, R2 = 2 Mbps, and R3 = 1 Mbps. 
	- a. Assuming no other traffic in the network, what is the throughput for the file transfer?
		- The minimum value of the rates of the three links = 500 Kbps.
	- b. Suppose the file is 4 million bytes. Dividing the file size by the throughput, roughly how long will it take to transfer the file to Host B?
		- 64 seconds.

## Section 5. Peer-to-Peer File Distribution

### Notes
- A layered architecture allows us to discuss a well-defined, specific part of a large and complex system.
- The internet protocol stack consists of the following layers from top to down:
	- Application layer: HTTP, SMTP, FTP, DNS
	- Transport layer: TCP, UDP
	- Network layer: IP
	- Link layer: Ethernet, WIFI
	- Physical layer
  
<img width="934" alt="Screenshot 2024-01-19 at 9 59 12 AM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/53ffd428-bac0-45b6-888e-1465e17b3f9f">

### Review Questions
-  List five tasks that a layer can perform. Is it possible that one (or more) of these tasks could be performed by two (or more) layers?
	-  Five generic tasks are error control, flow control, segmentation and reassembly, multiplexing, and connection setup.
	- Yes. Each layer in the Internet protocal stack implement an error recovery on pre-link basis and end-to-end basis.
- What are the five layers in the Internet protocol stack? What are the principal responsibilities of each of these layers?
	- Application layer: 
	- Transport layer: transports application-layer messages between application endpoints.
	- Network layer: moves network-layer packets from one host to another.
	- Link layer: routes a datagram through a series of routers between the source and destination.
	- Physical layer: moves the individual bits within the frame from one node to the next.
- What is an application-layer message? A transport-layer segment? A network-layer datagram? A link-layer frame?
	- Application-layer message is the information at the application layer.
	- Transport-layer segment is the packet at the transport layer after adding headers.
- Which layers in the Internet protocol stack does a router process? Which layers does a link-layer switch process? Which layers does a host process?
	- Routers process network, link and physical layers (layers 1 through 3).
	- Link layer switches process link and physical layers (layers 1 through 2).
	- Hosts process all five layers.

## Section 6. Video Streaming and Content Distribution Networks

### Notes
- Possible attacks
  1. 数据包拦截（packet interception）好比是有人在邮局里偷看别人的信。在计算机网络中，这种行为叫做“数据包嗅探”（packet sniffing）。当数据（像信件一样）在网络（可以想象成一条公路）上传输时，有些不好的人（黑客）会使用特殊的工具来“嗅探”这些数据。特别是在像共享以太网或无线网络这样的广播媒体中，他们可以抓取经过的所有数据包，包括密码等敏感信息。
  2. 伪装身份（fake identity）就像是有人寄了一个假的地址标签的包裹。在网络中，这称为“IP欺骗”（IP spoofing）。黑客发送一个带有假冒源地址的数据包，让接收者以为数据包来自另一个合法的来源。
  3. 服务拒绝攻击（Denial of Service, DoS）就像一个坏人故意在商店门口堆满了垃圾，导致顾客无法进入商店。在计算机网络中，这种攻击是通过发送大量无意义的流量到某个服务器，使得合法的流量不能到达服务器，从而使服务器或网络带宽不可用。
    - 服务拒绝攻击通常有以下步骤：
      - 选择目标： 就是黑客选定要攻击的服务器。
      - 入侵周围的主机： 黑客会入侵网络中的其他电脑，这些被入侵的电脑被称为“僵尸网络”（botnet）的一部分。
      - 从受损的主机发送数据包到目标： 黑客通过这些被控制的电脑发送大量数据包给目标服务器，导致正常的流量无法到达。

**Lines of defense:**
- authentication: proving you are who you say you are
  - cellular networks provides hardware identity via SIM card; no such hardware assist in traditional Internet
- confidentiality: via encryption
- integrity checks: digital signatures prevent/detect tampering
- access restrictions: password-protected VPNs
- firewalls: specialized “middleboxes” in access and core networks:
  - off-by-default: filter incoming packets to restrict senders, receivers, applications
  - detecting/reacting to DOS attacks
- … lots more on security (throughout, Chapter 8)

### Review Questions
-  What is self-replicating malware?
- Describe how a botnet can be created and how it can be used for a DDoS attack.
- Suppose Alice and Bob are sending packets to each other over a computer network. Suppose Trudy positions herself in the network so that she can capture all the packets sent by Alice and send whatever she wants to Bob; she can also capture all the packets sent by Bob and send whatever she wants to Alice. List some of the malicious things Trudy can do from this position.


## Section 7. Socket Programming-Creating Network Applications

### Notes


## Section 8. Supplemental Topics

### Notes


- End systems are referred to as hosts because they host (run) application programs such as:
	- Web browser
	- Web server
	- E-mail client
	- E-mail server
- Access network is the network that physically connects an end system to the first router (edge router) on a path from the end system to any other distant end system.
- The two most prevalent types of broadband residential access are digital subscriber line (DSL) and cable.
- Telephone line for obtaining DSL is divided into:
	- A high-speed downstream channel (50 KHZ to 1 MHZ band)
	- A medium-speed upstream channel (4 KHZ to 50 KHZ band)
	- An ordinary two-way telephone channel (0 to 4 KHZ band)
- The DSL standards define multiple transmission rates, including downstream transmission rates 24 Mbs and 52 Mbs, and upstream rates of 3.5 Mbps and 16 Mbps.
- The access is asymmetric because the downstream and upstream rates are different.
- The actual rates are limited by:
	- The DSL provider due to tiered services.
	- The gauge if the twisted-pair line.
	- The degree of electrical interference.
- DSL is designed for short distances between 5 and 10 miles.
- Cable internet access makes use if the cable television company's existing cable television infrastructure.
- Physical media is categorized as
	- Guides media:
		- Fiber-optic cable
			- An optical fiber is a thin, flexible medium that conducts pulses of light, with each pulse representing a bit.
			- They are immune to electromagnetic interference, have very low signal attenuation up to 100 KMs.
			- The Optical Carrier (OC) standard link speeds range from 51.8 Mbps to 39.8 Gbps. These specifications are often referred to as OC-n, where the link speed equals n * 51.8 Mbps.
		- Twisted-pair copper wire
			- Consists of two insulated copper wires, each about 1 mm thick, arranged in a regular spiral pattern.
			- The wires are twisted together to reduce the electrical interference from similar pairs close by.
		- Coaxial cable
			- Consists of two copper conductors, but the two are cocentric rather than parallel.
			- It can be used as a guided shared medium.
	- Unguided media:
		- Wireless LAN
		- Digital satellite channel
- The actual cost of physical link is relatively minor compared with other networking costs.

### Review Questions
-  List four access technologies. Classify each one as home access, enterprise access, or wide-area wireless access.
	- Home access: Ethernet LAN, Digital Subscriber Line over telephone line, and Cable internet access.
	- Enterprise access: Ethernet, WI-FI.
	- Wide-are Wireless access: 4G, 5G.
- Is HFC transmission rate dedicated or shared among users? Are collisions possible in a downstream HFC channel? Why or why not?
	- Shared.
	- On the downstream channel, all packets emanate from a single source, namely, the head end. Thus, there are no collisions in the downstream channel.
-  List the available residential access technologies in your city. For each type of access, provide the advertised downstream rate, upstream rate, and monthly price.
- What is the transmission rate of Ethernet LANs?
	- Ethernet LANs have transmission rates of 10 Mbps, 100 Mbps, 1 Gbps and 10 Gbps. 
- What are some of the physical media that Ethernet can run over?
	- Twisted-pair copper cables.
	- Fiber-optics cables.
- HFC, DSL, and FTTH are all used for residential access. For each of these access technologies, provide a range of transmission rates and comment on whether the transmission rate is shared or dedicated.
	- HFC: up to 42.8 Mbps and upstream rates of up to 30.7 Mbps, bandwidth is shared
	- DSL: up to 24 Mbps downstream and 2.5 Mbps upstream, bandwidth is dedicated
	- FTTH: 2-10Mbps upload; 10-20 Mbps download; bandwidth is not shared.
- Describe the most popular wireless Internet access technologies today. Compare and contrast them.
	- Wifi (802.11) In a wireless LAN, wireless users transmit/receive packets to/from an base station (i.e., wireless access point) within a radius of few tens of meters. The base station is typically connected to the wired Internet and thus serves to connect wireless users to the wired network.
	- 3G and 4G wide-area wireless access networks. In these systems, packets are transmitted over the same wireless infrastructure used for cellular telephony, with the base station thus being managed by a telecommunications provider. This provides wireless access to users within a radius of tens of kilometers of the base station.