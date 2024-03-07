---
layout: post
title:  阻塞队列-双锁实现2-解决1的问题
categories: [java,数据结构与算法,算法,阻塞队列]
description: 阻塞队列-双锁实现2-解决1的问题介绍。
keywords: 算法
---

# 阻塞队列-双锁实现2-解决1的问题

我们在 阻塞队列-双锁实现1中最后 编程的 唤醒线程的操作虽然 解决了唤醒线程时的报错问题但是 还是存在着 死锁的问题

我们先抛开 代码逻辑 只看 加锁 和 解锁的 代码

我们考虑这样一个场景，比如offer这个线程它已经获得了锁

![image-20240121154139650](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211541710.png)

然后它准备去获得 offer 方法中的 headLock 锁，当然现在 还没有获取，而poll 这个线程现在已经拿到了 headLock 这把锁了，而这个poll 线程又正准备去拿到tailLock锁 暂时还没有获得

那么问题就来了 ，对于offer 线程来将 它想获取的 headLock 锁 被 poll 线程拿着呢 而poll线程还没有释放锁

而poll 线程想要获得的tailLock锁 又被 offer 线程锁 持有着，同样offer 这个线程也没有 释放锁，所以大家都在等待对方释放锁

![image-20240121154705363](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211547423.png)

这样谁都不会释放锁 那么这就会造成 死锁的问题

测试代码如下：

```java
@Test
public void test2() throws InterruptedException
{
   BlockingQueue2<String> queue = new BlockingQueue2<>(3);
   queue.offer("元素1");
   queue.offer("元素2");

   new Thread(()->{
      try {
         queue.offer("元素3");
      } catch (InterruptedException e) {
         throw new RuntimeException(e);
      }
   }, "offer").start();

   new Thread(()->{
      try {
         queue.poll();
      } catch (InterruptedException e) {
         throw new RuntimeException(e);
      }
   }, "poll").start();
}
```

在测试代码中，我们已经添加了 两个元素 而 队列 容量为 3 让队列 既不是空 也不是满，这样它就不会进入到 await 方法进行线程阻塞 ，它们就会走 后面的逻辑

我们打上断点进行调试查看：

![image-20240121155429655](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211554786.png)

我们单步进入 offer 线程，让它先拿到 tailLock 锁，此时 offer 已经锁住了 tailLock

![image-20240121155531393](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211555439.png)

我们直接跨步执行，直接执行到headLock这里

![image-20240121155635337](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401211556383.png)

它准备获得headLock这把锁 但是 它还没获得这把锁的时候 我们再切换到 poll 线程

