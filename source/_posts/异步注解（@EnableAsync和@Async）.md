---
title: 异步注解（@EnableAsync和@Async）
tags:
  - 异步注解
  - EnableAsync
  - Async
categories:
  - 异步注解
  - Async
  - EnableAsync和
top_img: /img/yibu.jpg
cover: /img/yibu.jpg
keywords: 异步注解
description: 异步注解（@EnableAsync和@Async）
post_meta:
  page:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
  post:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
abbrlink: ce3886d7
date: 2023-10-14 22:50:18
---
# 为什么要使用？
1.在我们的日常开发中，我们偶尔会遇到在业务层中我们需要同时修改多张表的数据并且需要有序的执行，如果我们用往常的同步的方式，也就是单线程的方式来执行的话，可能会出现执行超时等异常造成请求结果失败，即使成功，前端也需要等待较长时间来获取响应结果，这样不但造成了用户体验差，而且会经常出现请求执行失败的问题。

2.在解释异步调用之前，我们先来看同步调用的定义；
1. 同步就是整个处理过程顺序执行，当各个过程都执行完毕，并返回结果。
2. 异步调用则是只是发送了调用的指令，调用者无需等待被调用的方法完全执行完毕；而是继续执行下面的流程。

# 什么时候使用
1.在某个调用中，需要顺序调用 A, B, C三个过程方法；如他们都是同步调用，则需要将他们都顺序执行完毕之后，才算作过程执行完毕； 如B为一个异步的调用方法，则在执行完A之后，调用B，并不等待B完成，而是执行开始调用C，待C执行完毕之后，就意味着这个过程执行完毕了。

2.在生成订单的时候给用户发送短信，生成订单的结果不应该被发送短信的成功与否所左右，也就是说生成订单这个主操作是不依赖于发送短信这个操作，所以我们就可以把发送短信这个操作置为异步操作。

# 一、基本介绍
@Async是spring为了方便开发人员进行异步调用的出现的，在方法上加入这个注解，spring会从线程池中获取一个新的线程来执行方法，实现异步调用

@EnableAsync表示开启对异步任务的支持，可以放在springboot的启动类上，也可以放在自定义线程池的配置类上，具体看下文

# 二.最简单的使用
在springboot项目中，直接在启动类上加上@EnableAsync，然后在service层的方法上对于需要异步调用的方法加上@Async，

那么当controller层调用这个方法的时候，就会在主线程外自动新建线程执行该方法。

1.springboot启动类开启异步支持
![图片](/img/async-1.png)

2.service层的方法加@Async，如果在类上加该注解表示整个类的方法都异步执行，建议加到具体的某个方法上
![图片](/img/async-2.png)

3.controller层调用service层的异步方法，这里用主线程在异步方法前后执行了2次打印输出
![图片](/img/async-3.png)

4.调用的结果

首先看看没有异步执行，正常的顺序执行的结果

可以看到，按顺序执行，全部是main线程http-nio-8181-exec-124执行，并且service方法的执行结果在中间，如下所示
![图片](/img/async-4.png)

由于我们的方法使用了@Async注解，所以主线程http-nio-8181-exec-124不等异步方法完成，先结束了，异步线程task-1继续执行.
![图片](/img/async-5.png)

**tips：没有自定义线程池@Async默认的线程池是SimpleAsyncTaskExecutor**

# 三.自定义线程池来使用@Async
1.新建一个线程池配置类，@EnableAsync在配置类上加，不用在启动类上加也行，可以配置不同的线程池，用bean的name做区分
![图片](/img/async-6.png)

2.@Async的使用一样是在service层的方法上加，如果配置了多个线程池，可以用@Async("name")，那么表示线程池的@Bean的name，来指定用哪个线程池处理.

假如只配置了一个线程池，直接用@Async就会用自定义的线程池执行.

假如配置了多个线程池，用@Async没指定用哪个线程池，会用默认的SimpleAsyncTaskExecutor来处理.
![图片](/img/async-7.png)

假如配置了多个线程池，用@Async("name")，会用指定的线程池处理

比如service层方法上指定pool1线程池
![图片](/img/async-8.png)

执行结果，异步线程名是pool配置的fzhThread
![图片](/img/async-9.png)

# 四.注解没生效的原因
1.异步方法使用static修饰

2.异步方法类没有使用@Service注解（或其他注解）导致spring无法扫描到异步类

3.controller中需要使用@Autowired或@Resource等注解自动注入service类，不能自己手动new对象