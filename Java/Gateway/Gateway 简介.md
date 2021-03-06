

## 1.网关的作用

当前，微服务架构越来越流行，在微服务架构的原则下，根据业务内聚、康威法则、配合DevOps等等等等.......复杂庞大的单体应用逐渐被拆分为许多个体应用；

下图描述了这样一个简单的场景，商品销售网站系统被垂直拆分为诸如商品、库存等模块；

而对于一个系统外的用户，或者对于外部系统来说，当他们访问该网站提供的服务时，我们不希望对他们来暴露系统拆分这样的细节；

这种屏蔽功能就可以由网关来完成，当然，网关的功能远不止如此，这只是微服务场景下，网关的作用之一。

![image-20210122105104071](D:\Notes\Java\Gateway\Untitled.resource\image-20210122105104071.png)

## 2. 网关常见的用途

网关在具体的系统中，扮演的角色和用途均不太一样，通常情况下，会被设计来做如下事情：

a.统一日志管控。可以对访问后端服务的所有请求，打统一的访问日志，实现对系统访问的集中管控，日志收集到一起也便于做统一分析

b.反向路由。可以把具体的请求，按照预定的路由逻辑表，分发到目标服务上

c.统一安全认证。将爬虫、攻击等非法请求拦截在服务之外，对于正常请求的权限管控，也可以统一收拢在网关

d.限流熔断。面对流量洪峰时，可以执行限流、熔断逻辑，可以基于服务响应时间，维护服务器的权重，减少问题服务导致雪崩的可能性

e.简化接入逻辑。服务拆分之后，可能会存在大量的微服务，网关可以收敛后端微服务于一点，方便外界系统的对接

## 3. 网关在架构中的位置A

下面以一个抽象的例子，描述一下Gateway在系统架构中所处的位置：

1：端发送请求到系统，端种类可以是PC、APP、H5或者其他外部系统server

2：一般会有个负载均衡层，实现方式比较多，可以是硬件的F5，也可以是软件的LVS，因为LVS本身也存在单点，所以会配合DNS做高可用

3：负载均衡把请求转给Nginx，为了Nginx层的高可用，通常会用VIP来部署多个，同时通过Keepalived做心跳监控

4：对于前端的请求，Nginx可以转发到前端系统

5：Nginx作为Http Server，也可以直接响应对于前端资源的访问

6：对于后端的请求，例如一些API访问，Nginx统一转发到Gateway

7：Gateway通过路由信息，将具体请求转发到具体的微服务上

![image-20210122105116195](D:\Notes\Java\Gateway\Untitled.resource\image-20210122105116195.png)

## 4.网关在架构中的位置B

关于网关在系统架构中的位置，除了上述方案以外，网上流传有另一种使用网关的架构方式，据说是Netflix现行架构的高度抽象描述，感兴趣的同学也可以参考下。具体内容可以参考下图，对于其中的一些点，简单做一下说明：

a. 1层，是接入系统的多种多样的终端

b. 2层，是负载均衡服务，作用跟上文描述的类似，ELB应该是AWS上的一款服务，弹性负载均衡

c. 3层，网关Zuul

d. 4层，请求转发到Web站点；如果是API的请求，则转发至下游的Edge Service

e. 4.5层，Edge Service（边界服务），这一层可以看做是对外提供服务的层次，可以将请求拆分、服务逻辑动态裁剪、响应聚合等等事情

f. 5层，具体的微服务实现，小到一个RPC服务，大到一个相对复杂的Spring Boot项目

![image-20210122105126741](D:\Notes\Java\Gateway\Untitled.resource\image-20210122105126741.png)

 