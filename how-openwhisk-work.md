## 揭秘openwhisk的机理（详细版）

### **Nginx Kafka Consul Docker CouchDB**共同组成了这个无服务事件驱动的服务平台。

在激活一个action的时候究竟发生了什么？

1 通过CLI写的指令 其实都是向openwhisk系统的http请求 

POST /api/v1/namespaces/$userNamespace/actions/Actionname  

Host: $openwhiskEndpoint

通过post方法向apihost发送请求。

通过**Nginx**生成的上述POST,发送给 **controller**

2 controller接收到这个http请求后，会根据这个http请求搞清楚用户要干什么，（例如上述的 post请求就是对一个已存在的action的请求， 翻译成对一个action的invocate）

3 **CouchDB** 用来身份验证和身份授权 当controller验证了身份和是否授权，同时会在一个主题数据库 CouchDB中验证请求。如果用户在数据库中，CouchDB有效地赋予了用户调用该操作的特权。

4 Controller判断权限好了，就会从whisk database中load这个action。

5 当Controller获取了所有需要执行该action的东西之后，要看一下哪个服务器可以执行这个，这个过程由一个交consul的服务器discovery做，看哪个invoker可以做这件事。

6 当controller知道哪个invoker可以执行操作了， controller和invoker之间需要通信，这里就用到了kafka（一个高吞吐 分布式的消息系统）
" the Controller publishes a message to Kafka, which contains the action to invoke and the parameters to pass to that action. This message is addressed to the Invoker which the Controller chose above from the list it got from Consul. "

7 为了执行在一个孤立的和安全的方式行动，它使用的另一个巨头：docker ，也就是上述的 invoker。简而言之，对于每个动作调用，都会产生一个Docker容器，该action代码被注入，并使用传递给它的参数执行该操作，获得结果，该容器被销毁。
