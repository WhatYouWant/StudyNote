#AFNetworking学习笔记
##AFSecurityPolicy 安全相关处理
相关名词
SSL(Secure Sockets Layer 安全套接层),及其继任者传输层安全(Transport Layer Security, TLS)是为网络通信提供安全及数据完整性的一种安全协议。TLS与SSL在传输层对网络连接进行加密。
SSL协议提供的安全通道有以下三个特性:
机密性: SSL协议使用密钥加密通信数据。
可靠性: 服务器和客户都会被认证，客户的认证是可选的。
完整性: SSL协议会对传送的数据进行完整性检查。

会话(Session)和连接(Connection)是SSL中两个重要的概念。
SSL连接: 用于提供某种类型的服务数据的传输，是一种点对点的关系。
SSL会话: 是指客户端和服务器之间的一个关联关系。会话通过握手协议来创建。

https是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，https的安全基础是SSL。

