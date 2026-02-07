# 开场白

面试官您好，我是sjs，毕业于哈尔滨理工大学，学习的是软件工程专业，现已收到香港理工大学录取，我在校期间，通过了英语四六级考试，并考得雅思6分。

在校期间，我学习了C/C++ 初阶的数据结构，数据库等相关知识，但我并不满足于此，因为我的个人兴趣是软件开发，为了加强我的软件开发水平，同时也是为了了解我们计算机岗位的前沿知识，于是我自学了网络编程，linux编程，qt编程，以及更深层次的数据结构与算法。

在此基础上，为了检验我的学习成果，我完成了了两个项目，一个是趣聊星球，另一个是五子智弈，面试官我可以向您介绍这两个项目吗？

趣聊星球，是一款基于C/S架构的轻型聊天系统，他的服务端和客户端都是基于Windows平台开发，利用了QT的信号与槽机制实现了用户的注册登录添加好友等功能，使用TCP协议实现了客户端于服务端之间的双向通信。

在这个过程中，我发现了很多问题，也获得了很多灵感，在我进行更多的学习以及知识积累后，我开始了我的第二个项目——五子智弈

五子智弈是一款基于C/S架构的游戏大厅，客户端部署在Windows上，服务端部署在Linux，用户可以通过注册登录进入游戏大厅，在进入五子棋专区后，游玩内置的五子棋游戏，这款游戏支持双人联机游玩，同时使用了极大极小值搜索算法配合α-β剪枝来支持人机托管的功能。这个项目我使用的技术栈有：信号与槽、TCP网络通信、epoll线程池、md5信息摘要算法、中介者设计模式

English:

Thank you for this opportunity, it is an great honor for me to join this job interview today.

My name is SUN, Jiashi. I graduated from Harbin University of Science and Technology with a bachelor degree in Software Engineering. And I have accepted a postgraduate offer from The Hong Kong Polytechnic University for the Metaverse Technology. 

During my time at school, I studied basic C/C plus plus, data structures, databases, and related knowledge, but I was not satisfied with this, because my personal interest is in software development. To improve my software development skills and also to understand the cutting-edge knowledge of our computer positions, I self-studied network programming, Linux programming, Qt programming, as well as more advanced data structures and algorithms.

I'm really excited to be here to learn more about XXX, discuss how I can contribute to this role, and work effectively with your team.

---
# 遇到了哪些问题？

## 趣聊星球

我这个项目是基于 C/S架构开发的即时聊天系统，核心实现了TCP的网络通信、MySQL 数据持久化，以及注册、登录、添加好友、实时聊天、用户上下线等核心业务逻辑。开发中遇到了几个核心难点，我从**网络层、架构设计、数据层、业务层、资源管理**这四个方向来说：

1. 网络层：TCP粘包问题的解决
	- TCP和UDP的区别，为什么选择TCP；[[4、网络协议#2. TCP和UDP的区别（连接性、可靠性、适用场景）？]]
	- TCP有什么样的问题，TCP怎么解决粘包问题：[[4、网络协议#26. ⾼频 TCP粘包和拆包的原因及解决方法（固定长度、分隔符、包头+包长）？]]
	- 解决粘包问题里为什么选择了先发包大小后发包内容这种方式？
	- 【学到了什么】了解了TCP和UDP的区别，他们各自有什么样的应用场景
2. 架构设计：中介者模式的选择
	- 中介者模式的核心解耦多个对象之间的直接交互，通过中介者统一协调
	- 在我的项目中，所有的业务逻辑交给核心处理类处理，中介者只负责网络层面的数据收发，业务层无需关注网络部分细节
	- 【学到了什么】中介者模式的一些优缺点[[Cpp9、C++复习4-2设计模式#中介者模式]]
3. ==**数据层**==：MySQL数据一致性的解决，字符集的统一和转码
	- 手机号或者昵称可能会存在重复注册的问题
	- 
	- 给手机号、昵称添加唯一索引
	- 【学到了什么】
4. 业务层：自定义协议的解析和分发
	- 对于自定义协议的解析和分发，我原来想到的到是用if-else或者switch来实现，但经过我的学习，我了解到了有关==设计原则==的知识，其中有一项**开闭原则**，就是我的项目应该保证对扩展开放，对修改关闭。[[Cpp8、C++复习4-2设计原则#设计原则SOLID CD]]
	- 如果我全都用switch来进行协议的解析和分发的话，不仅会让处理协议的函数变得臃肿，对后期的扩展也会更加的麻烦
	- 于是为了让协议的分发更加灵活，更方便扩展。我设计了**函数指针数组**，让每个协议对应的处理函数都映射到数组下标（协议类型宏），这样处理函数只需要解析出协议的类型，就可以通过下标找到函数指针并进行调用。新增协议的话只需要扩展数组和处理函数就好了
	- 【学到了什么】==设计原则==，也理解到了“设计原则是设计模式的根基，设计模式是设计原则的践行”；
5. ==**多线程管理**==：
	- 服务端怎么实现的多线程管理：
	- 客户端接收线程
6. 测试的问题：
	- socket压力测试的过程中：连接只能达到100-150左右
	- 原因：
	- 【学到了什么】



7. 为什么选择中介者模式？解决了什么问题？不用有哪些弊端
	- 首先我了解到中介者模式的核心在于：解耦多个对象之间的直接交互，通过中介者统一协调
	- 在我的项目中，所有的业务逻辑交给核心处理类处理，中介者只负责网络层面的数据收发，业务层无需关注网络部分细节
8. 怎么处理TCP粘包？
9. 在服务端采用了多线程处理客户端连接和数据接收，这种设计的优缺点？
10. 在代码中如何管理线程资源？
11. 有没有考虑线程安全问题？
12. 数据库模块是否有潜在的问题
13. 协议是怎么设计的？考虑了哪些因素？
14. 有没有处理字节序的问题？
15. 传输大字节数据，有没有潜在的性能或稳定性问题？
16. 在核心处理类中使用函数指针数组来分发不同的协议请求，优势是什么？索引计算可能有什么风险？
17. 客户端和服务端发送数据的实现中，先发长度再发内容，在实际的传输中如果出现半包发送，怎么处理？
18. 怎么处理用户id和socket的映射关系
19. 用户状态（在线 / 离线）的持久化逻辑，服务端重启后用户状态丢失，你会如何设计状态持久化方案？
20. 你的数据库连接密码是硬编码的（H800-S081157），注册 / 登录的密码传输是明文，这两个问题如何解决？
21. 如果要给项目增加 “文件传输” 功能，你会如何扩展协议结构体和网络层代码？
22. 为什么压力测试是100-150？

## 五子智弈



