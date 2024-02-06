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
			- [比较 Web缓存、Cookies和Conditional GET：](#比较-web缓存cookies和conditional-get)
			- [HTTP/2 和 HTTP/3](#http2-和-http3)
	- [Section 3. Email, SMTP, IMAP](#section-3-email-smtp-imap)
		- [Email](#email)
		- [SMTP (simple mail transfer protocol)](#smtp-simple-mail-transfer-protocol)
	- [Section 4. The Domain Name Service: DNS](#section-4-the-domain-name-service-dns)
		- [DNS (Domain Name System)](#dns-domain-name-system)
			- [DNS structure, function](#dns-structure-function)
			- [resolving DNS queries](#resolving-dns-queries)
			- [DNS record format](#dns-record-format)
			- [DNS protocol messages](#dns-protocol-messages)
			- [Getting your info into the DNS](#getting-your-info-into-the-dns)
			- [DNS security](#dns-security)
	- [Section 7. Socket Programming-Creating Network Applications](#section-7-socket-programming-creating-network-applications)
		- [Notes](#notes-2)
	- [Section 8. Supplemental Topics](#section-8-supplemental-topics)
		- [Notes](#notes-3)
   
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

#### 比较 Web缓存、Cookies和Conditional GET：

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

### Email

**3 major components**
1. user agents (aka "mail reader")
   - composing, editing, reading mail messages (e.g. Outlook)
   - outgoing, incoming messages stored on server
2. mail servers
   - mailbox (contains incoming messages for user)
   - message queue of outgoing (to be sent) mail messages
3. simple mail transfer protocol (SMTP)
   - between mail servers to send mail messages
   - client: (sender side) sending mail server
   - server: (receiver side) receiving mail server

<img width="1034" alt="Screenshot 2024-01-22 at 1 34 04 PM" src="https://github.com/jhan125/Computer_Networking_JimKurose/assets/98071264/fff113cc-1105-492e-92ba-5ed637a9ea92">

### SMTP (simple mail transfer protocol)

1. protocol for exchanging email messages, defined in RFC 5321 
(like RFC 7231 defines HTTP)

2. RFC 2822 defines syntax for email message itself 
(like HTML defines syntax for web documents)

- header lines: e.g. To, From, Subject
- blank line
- body: the message (ASCII characters only)

**SMTP's comparison with HTTP**

1. HTTP: client pull; SMTP: client push
2. both have ASCII command/response interaction, status codes
3. HTTP: each object encapsulated in its own response message;
   SMTP: multiple objects sent in multipart message
4. SMTP uses persistent connections
5. SMTP requires message (header & body) to be in 7-bit ASCII
6. SMTP server uses CRLF.CRLF to determine end of message

SMTP和HTTP的不同：

1. HTTP是客户端拉取，SMTP是客户端推送：

想象HTTP就像你去餐厅点餐。你（客户端）告诉服务员（服务器）你想要什么，然后服务员把食物带给你。
SMTP更像是你给朋友寄信。你（客户端）把信件放在邮箱里，然后邮递员（服务器）来收信并送到你朋友那里。

2. 两者都使用ASCII命令/响应交互，状态码：

这就像在餐厅点餐或寄信时，服务员和邮递员都会用一些标准的话来回应你，比如“订单收到”或“信件已发送”。

3. HTTP: 每个对象都封装在它自己的响应消息中；SMTP: 多个对象可以在一个多部分消息中发送：

使用HTTP，就像每次只能从服务员那里拿到一个菜品。使用SMTP，就像你可以在一个大信封里放很多东西（比如几封信、照片）一起寄出。

4. SMTP使用持久连接：

这就像邮递员在你把所有信件都放入邮箱后再离开，而不是每放一封信就走一趟。

5. SMTP要求消息（头部和正文）必须是7位ASCII：

这就像写信时只能使用特定的字母和符号，不能用图片或特殊字符。

6. SMTP服务器用CRLF.CRLF来判断消息的结束：

这就像在信的最后写上“敬上”来表示信已经写完，邮递员看到这个就知道这是信件的结尾。

所以，总的来说，HTTP和SMTP都是用来在网络上交换信息的方法，但它们的工作方式不同。HTTP更像是一个请求-响应的过程，类似于在餐厅点餐；而SMTP是一个发送消息的过程，类似于寄信。

<img width="986" alt="Screenshot 2024-01-22 at 3 19 47 PM" src="https://github.com/geekahmed/Computer-Networking---A-Top-Down-Approach/assets/98071264/425aeef5-e28d-4448-a508-ff946569f0dd">

1. SMTP（简单邮件传输协议）就像你写了一封信然后把它放到你家附近的邮筒里。邮局的工作人员会来收信，然后把它传送到你朋友的邮筒。这里的SMTP就是邮局的传送系统，确保你的电子邮件从你的电脑安全地到达朋友的电子邮箱服务器。

2. 邮件访问协议 IMAP（互联网消息访问协议）：这就像是你去邮局拿信。你可以看信，决定留下来还是扔掉，或者把它分类放到不同的文件夹里。即使你回家了，邮局里还有信的副本。
   
3. HTTP：这个过程就像是你通过网页版邮局检查你的邮件。你可以在任何有互联网的地方登录一个网页，比如Gmail或Yahoo!Mail，来发送（通过SMTP）和接收（通过IMAP或POP）电子邮件。
   
所以，这张图就是在说，SMTP是用来“寄出”电子邮件的，而IMAP和HTTP是用来“取回”电子邮件的不同方式。SMTP确保电子邮件能从寄件人发送到收件人的服务器上，IMAP允许收件人查看和管理邮件，而HTTP则提供了一个网页界面来进行这些操作。

## Section 4. The Domain Name Service: DNS

### DNS (Domain Name System)

- distributed database implemented in the hierarchy of many name servers
- a protocol for application layer: hosts, DNS servers communicate to resolve names (address/name translation)
  - note: core internet function, implemented as application-layer protocol
  - complexity at network's 'EDGE'

DNS（域名系统）就像是互联网的电话簿。当你想要访问一个网站时，你通常会输入一个网址，比如 www.example.com。这个网址很容易被人记住，但是互联网上的计算机并不使用这些名字来定位和识别彼此，它们使用一串数字，称为IP地址，如 192.0.2.1。

DNS的工作就是将你容易记住的网址（域名）转换成计算机使用的IP地址。每当你输入一个网址，你的计算机就会向DNS服务器发出请求，DNS服务器会查找对应的IP地址，并将其返回给你的计算机，然后你的计算机才能加载你想访问的网站。

这个过程就像是当你想打电话给“小明”时，你不需要记住小明的电话号码，你只需要在电话簿里找到小明的名字，电话簿会告诉你小明的电话号码，然后你才能拨打电话。


**DNS services:**
1. hostname-to-IP-address translation
2. host aliasing: canonical, alias names
3. mail server aliasing
4. load distribution: replicated Web servers (many IP addresses correspond to one name)
   
DNS服务：

1. 域名转IP地址：就像学校里每个学生都有自己的学号。当你知道朋友的名字但不知道学号时，你可以查学校的名册来找到对应的学号。
2. 主机别名：有时候学校的活动或建筑有正式的名字和大家常用的别名。比如，图书馆可能正式叫“知识中心”，但大家都叫它“图书馆”。
3. 邮件服务器别名：老师们可能有正式的电子邮件地址，但也有更容易记的别名，比如“数学老师”可能会被映射到一个真实的电子邮件地址。
4. 负载分配：如果学校有很多个图书馆分布在校园的不同地方，你可以去任何一个图书馆借书。这样可以避免所有学生都挤在一个图书馆。

**Why not centralize DNS?**
1. single point of failure
1. traffic volume
2. distant centralized database
3. maintenance

**doesn‘t scale!**
§ Comcast DNS servers alone: 600B DNS queries/day
§ Akamai DNS servers alone: 2.2T DNS queries/day

就像学校不可能只有一个办公室来处理全校所有的事情。如果那样的话，那个办公室如果出问题了，那么全校的事情都会受影响。而且如果每个学生都要去那个办公室，那里将会非常拥挤。DNS服务就像是学校里帮你找到你需要信息的地方，而分布式的结构（不是集中在一个地方）是为了避免压力集中在一个点上，导致系统不稳定和难以维护。

如果DNS集中在一个地方：

1. 单点故障：如果那个中心出了问题，整个学校的通信都会停止。
2. 流量体积：那个中心会非常忙，处理不过来所有的请求。
3. 维护困难：只有一个地方负责所有事情，维护起来会非常困难。
4. Comcast和Akamai的例子，是说即使是一个办公室也会每天收到很多请求，全校只有一个办公室的话压力会更大。

**Summary** 

DNS can be thought as ...

1. a humongous distributed database:
   ~ billion records, each simple
2. handles many trillions of queries/day:
- many more reads than writes
- performance matters: almost every Internet transaction interacts with DNS - msecs count!
3. organizationally, physically decentralized:
- millions of different organizations responsible for their records
4. “bulletproof”: reliability, security

#### DNS structure, function

<img width="1053" alt="Screenshot 2024-01-22 at 3 38 20 PM" src="https://github.com/geekahmed/Computer-Networking---A-Top-Down-Approach/assets/98071264/37166a8f-9318-44f5-b5c4-3dd5a94bb38d">

DNS的结构就像是一个有多层管理的大公司。在这个公司里，有不同层级的管理者，每个人负责不同的工作，以确保整个公司运作顺畅。

**1. 根域名服务器（Root Name Servers）**
- official, contact-of-last-resort by name servers that cannot resolve name
- incredibly important Internet function
  - Internet couldn’t function without it!
  - DNSSEC – provides security (authentication, message integrity)
- ICANN (Internet Corporation for Assigned Names and Numbers) manages root DNS domain
- 13 logical root name "servers" worldwide each "server" replicated many times (~200 servers in US)
- 这就像公司的董事会。根域名服务器是最高层级，当DNS系统需要开始查找域名（比如你想访问的网站）对应的IP地址时，就从这里开始。有13组根域名服务器分布在世界各地，它们知道所有顶级域名服务器的位置，但它们不直接知道每个域名的IP地址。

**2. 顶级域名服务器（Top-Level Domain 'TLD' Servers）：**
- responsible for .com, .org, .net, .edu, .aero, .jobs, .museums, and all top-level country domains, e.g.: .cn, .uk, .fr, .ca, .jp
- Network Solutions: authoritative registry for .com, .net TLD
- Educause: .edu TLD
- 顶级域名服务器像是公司中的部门经理。每个顶级域名服务器负责管理域名的一个部分，比如.com、.org、.net或一个国家的域名，如.uk、.de等。当根服务器被问到特定的域名时，它会指向相关的顶级域名服务器。

**3. 权威域名服务器（Authoritative DNS Servers）：**
- organization’s own DNS server(s), providing authoritative hostname to IP mappings for organization’s named hosts
- can be maintained by organization or service provider
- 权威服务器就像是公司的项目经理。它负责知道具体的域名对应的IP地址。当顶级域名服务器被问到更具体的域名（比如www.amazon.com）时，它会指向权威服务器，权威服务器有权提供该域名的最终IP地址。

**4. 本地域名服务器（Local DNS Name Servers）：**
- when host makes DNS query, it is sent to its local DNS server
  - Local DNS server returns reply, answering:
    - from its local cache of recent name-to-address translation pairs (possibly out of date!) 
    - forwarding request into DNS hierarchy for resolution
  - each ISP has local DNS name server; to find yours:
    - `MacOS: % scutil --dns`
    - `Windows: >ipconfig /all`
- local DNS server doesn’t strictly belong to hierarchy
- 这就像是公司前台。它通常是你的互联网服务提供商（ISP）拥有的DNS服务器。当你想要访问一个网站时，你的电脑首先会问前台，即本地DNS服务器，它有没有这个网站的IP地址。如果本地服务器不知道，它会代表你去问更高层级的服务器，直到找到答案。

在这个过程中，每个域名服务器都有它的责任和权限范围。当一个域名查询发生时，会从本地DNS服务器开始，逐步向上到根服务器，再向下到顶级域名服务器和权威域名服务器，直到找到准确的IP地址，然后这个信息会沿着相同的路径返回给本地服务器，最后返回给你的电脑，这样你就可以访问你想去的网站了。

#### resolving DNS queries

DNS name resolutions: DNS的名称解析是将用户友好的域名（如 www.example.com）转换成计算机可理解的IP地址（如 192.0.2.1）的过程。在这个过程中，有两种主要的查询方法：迭代查询（Iterated Query）和递归查询（Recursive Query）。

**Iterated Query**

<img width="1040" alt="Screenshot 2024-01-22 at 3 54 18 PM" src="https://github.com/geekahmed/Computer-Networking---A-Top-Down-Approach/assets/98071264/793debd5-59b8-444b-b79b-94ccfdf3fdd1">

这就像你自己去找某个学生。首先，你去学校办公室问，办公室告诉你应该去哪栋楼。然后你去那栋楼的管理处，管理处告诉你具体在哪个楼层。最后，你去那个楼层的辅导员那里，辅导员告诉你具体在哪个教室。在整个过程中，你自己负责从一个地方走到另一个地方，每个人只告诉你下一步应该去哪里。

在DNS中，迭代查询是当一个DNS服务器接收到查询请求时，它可能会返回下一个要查询的DNS服务器的地址给客户端，客户端然后向这个新服务器发起新的查询，如此重复直到找到答案。

**Recursive Query**

<img width="1021" alt="Screenshot 2024-01-22 at 3 57 01 PM" src="https://github.com/geekahmed/Computer-Networking---A-Top-Down-Approach/assets/98071264/5228561a-b4e9-4f8d-a7ad-5fc97a86aed7">

这就像你直接让学校办公室的人帮你找到那个学生。你告诉办公室你要找的人，然后办公室的工作人员自己去找，他们可能先去楼层辅导员那里问，然后再去具体的教室，最后把找到的结果带回来告诉你。

在DNS中，递归查询是当一个DNS服务器接收到查询请求时，如果它没有答案，它就会代表请求者去其他服务器查询，并负责完成整个查找过程，最后只返回最终的结果给客户端。

在实际的DNS查询中，通常会使用这两种方法的组合。一个本地DNS服务器可能对客户端进行递归查询，然后它自己对其他DNS服务器进行迭代查询。这种组合方法可以减少客户端的查询负担，同时允许DNS服务器有效地管理名称解析过程。

**Caching DNS Information**
- once (any) name server learns mapping, it caches mapping, and immediately returns a cached mapping in response to a query
  - caching improves response time
  - cache entries timeout (disappear) after some time (TTL)
  - TLD servers typically cached in local name servers
- cached entries may be out-of-date
  - if named host changes IP address, may not be known Internet-wide until all TTLs expire!
  - best-effort name-to-address translation!

**缓存DNS信息的好处是：**

1. 提高响应时间：就像图书管理员用记录本直接告诉你书的位置，而不是每次都去查找书架，这样可以更快地回应下一个想借这本书的人。
2. 缓存有超时时间（TTL，Time-To-Live）：记录本上的标记不会永远存在。过了一段时间（TTL），标记就会被清除，这样可以保证信息的更新。这就像管理员定期检查记录本，确保书的位置信息是最新的。

**缓存可能的问题：**

1. 如果书的位置变了（比如被移到了另一个地方），但记录本上的标记还没更新，那么管理员可能会告诉你错误的位置。在DNS中，如果一个网站的IP地址变了，但是DNS的缓存还没更新，你的电脑可能会被告知旧的IP地址，直到缓存过期并获取到最新信息。
2. 这个过程是尽力而为的：就像图书管理员尽最大努力确保书的位置信息是准确的，但有时候可能还是会出错一样，DNS服务器也尽力确保它提供的IP地址是最新的，但有时候也可能会有延迟。这就是为什么有时候你可能会遇到一个网站无法访问，然后过一会儿又可以访问了——这可能是因为DNS信息刚刚更新了缓存。

#### DNS record format

<img width="1026" alt="Screenshot 2024-01-22 at 4 05 40 PM" src="https://github.com/geekahmed/Computer-Networking---A-Top-Down-Approach/assets/98071264/fe53b3be-2656-4212-ba05-f1a284d2b557">

DNS记录就像是学校的详细名册，里面不只有学生的名字和学号，还包括他们的班级、课表、校车路线等信息。每个学生或老师在名册中都有不同类型的记录，告诉大家如何找到他们或他们负责的服务。在DNS中，这些记录称为资源记录（Resource Record，RR），每个记录都有类型，告诉你这个记录是用来干什么的。

一些常见的DNS记录类型和它们的类比：

1. NS记录（Name Server Record）：
   
类型：NS
就像是告诉你哪个老师负责哪个班级。例如，foo.com的NS记录会告诉你负责foo.com这个域名的服务器的名字。

2. A记录（Address Record）：

类型：A
就像是学生的名字和学号。它告诉你一个具体的电脑名字（如server1.foo.com）对应的IP地址是多少。

3. CNAME记录（Canonical Name Record）：

类型：CNAME
就像是别名。如果一个学生有一个常用名字和一个正式名字，CNAME记录会告诉你www.example.com这个名字实际上是servereast.backup2.example.com。

4. MX记录（Mail Exchange Record）：

类型：MX
就像是告诉你学校的邮件服务负责人是谁。如果你要发邮件给example.com中的某人，MX记录会告诉你应该把邮件发送到哪个邮件服务器。

这些记录都有一个TTL（生存时间），就像是名册里记录的信息每学期都要更新一次。这样确保了信息的准确性，因为学生可能换了班级，老师可能换了教学科目，等等。

#### DNS protocol messages

<img width="882" alt="Screenshot 2024-01-22 at 4 07 00 PM" src="https://github.com/geekahmed/Computer-Networking---A-Top-Down-Approach/assets/98071264/45352834-ce9c-4636-93f5-231f7b32e023">

<img width="879" alt="Screenshot 2024-01-22 at 4 07 35 PM" src="https://github.com/geekahmed/Computer-Networking---A-Top-Down-Approach/assets/98071264/9f34fc7e-a37e-45f5-9e23-ca26d3461b29">


#### Getting your info into the DNS

example: new startup “Network Utopia”
- register name networkuptopia.com at DNS registrar (e.g., Network
Solutions)
  - provide names, IP addresses of authoritative name server (primary and secondary)
  - registrar inserts NS, A RRs into .com TLD server:
  - `(networkutopia.com, dns1.networkutopia.com, NS)`
  - `(dns1.networkutopia.com, 212.212.212.1, A)`
- create authoritative server locally with IP address 212.212.212.1
  - type A record for www.networkuptopia.com
  - type MX record for networkutopia.com

**流程：**

1. 注册域名：

首先，你需要给你的公司取一个名字，并且在一个叫做域名注册机构（比如Network Solutions）的地方登记这个名字。就像你要在市政府登记一个新的商业名称一样，这样别人就知道那个名字属于你了。

2. 提供服务器名称和IP地址：

当你在域名注册机构登记你的网站名字，比如networkutopia.com后，你需要告诉他们你的服务器的名字和数字地址（IP地址）。这就像你告诉市政府你的公司地址和电话号码，这样人们就知道怎么联系你了。

3. 注册机构更新DNS记录：

注册机构会在互联网的地址簿里（.com顶级域名服务器）加上你的信息。他们会创建两条记录：一条NS记录，告诉大家你的域名networkutopia.com是由哪个服务器（比如dns1.networkutopia.com）管理的；还有一条A记录，告诉大家这个服务器的数字地址（IP地址）是212.212.212.1。

4. 设置你自己的权威服务器：

你需要在一个电脑上设置一个DNS服务器，让它成为你公司域名信息的权威来源。你可以在这个服务器上设置更多的DNS记录，比如一个A记录，让人们通过www.networkutopia.com就能访问你的网站，还有一个MX记录，告诉大家要把发给networkutopia.com的邮件送到哪个邮件服务器。

#### DNS security

**DDoS attacks**
- bombard root servers with traffic
  - not successful to date
  - traffic filtering
  - local DNS servers cache IPs of TLD servers, allowing root server bypass
- bombard TLD servers
  - potentially more dangerous

**Spoofing attacks**
- intercept DNS queries, returning bogus replies
- DNS cache poisoning
- RFC 4033: DNSSEC
- authentication services

## Section 7. Socket Programming-Creating Network Applications

### Notes


## Section 8. Supplemental Topics

### Notes

