---
title: 数据库事物&并发脚本
author: 
  name: nancyxia
  link: https://github.com/Spongiabob
date: 2021-10-12 18:32:00 +8
categories: [Blogging, Code]
tags: [db事物并发]
---

了解数据库事物，以及写一个并发脚本测试事物


## db事物

### 事物简介

数据库事物的定义：把多条语句作为一个整体进行操作的功能，被称为数据库**事务**。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

#### 事物特性 
数据库事务具有ACID这4个特性：
A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。


#### 事物隔离性详解

对于两个并发执行的事务，如果涉及到操作同一条记录的时候，可能会发生问题。因为并发操作会带来数据的不一致性，包括脏读、不可重复读、幻读等。数据库系统提供了隔离级别来让我们有针对性地选择事务的隔离级别，避免数据不一致的问题。

1.Read Uncommitted是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）。

2.在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。
不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

3.Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。
幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

4.Serializable是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。
虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

# 并发

## 并发简介

![并发]({{"/assets/img/blog/Concurrency.jpeg"|absolute_url}}){: width="800" height="800"}

Concurrency，是并发的意思。并发的实质是一个物理CPU(也可以多个物理CPU) 在若干道程序（或线程）之间多路复用，并发性是对有限物理资源强制行使多用户共享以提高效率。
微观角度：所有的并发处理都有排队等候，唤醒，执行等这样的步骤，在微观上他们都是序列被处理的，如果是同一时刻到达的请求（或线程）也会根据优先级的不同，而先后进入队列排队等候执行。
宏观角度：多个几乎同时到达的请求（或线程）在宏观上看就像是同时在被处理。
通俗点讲，并发就是只有一个CPU资源，程序（或线程）之间要竞争得到执行机会。图中的第一个阶段，在A执行的过程中B，C不会执行，因为这段时间内这个CPU资源被A竞争到了，同理，第二个阶段只有B在执行，第三个阶段只有C在执行。其实，并发过程中，A，B，C并不是同时在进行的（微观角度）。但又是同时进行的（宏观角度）。

版权声明：摘抄自CSDN博主「黄复贵」的原创文章，遵循CC 4.0 BY-SA版权协议
原文链接：https://blog.csdn.net/qq_33290787/article/details/51790605


## 场景讲解
场景如下：春节期间，购买流量给用户发送红包，发送红包两个操作，
1.新增一条红包记录：insert into t_redpackage_deal values(****);
2.扣减红包的总金额库存:update t_redpackage set amount=amount-hbnumber where id=1 and amount>0;

这两个操作组成了一个db事物,为了保证红包库存扣减的正确性，因此，需要验证高并发情况下，红包库存情况


## 使用python实现并发脚本
code 如下

```py

# -*- coding: utf-8 -*-
import threading
import time
from concurrent.futures import ThreadPoolExecutor
from concurrent.futures import ProcessPoolExecutor
from threading import current_thread
import datetime

import pytest
import allure

def fun(dealid)
    #调用db 
    resualt=db.XXX
    print("新增红包函数db调用",dealid)
    return resualt
def thread(dealid):
    global error_number
    global result
    #确认服务是否是一个并发                                     )
    print("start:",dealid,datetime.datetime.now())
    time1=time.perf_counter()
    rsp = fun(dealid)
    print("end:", dealid,datetime.datetime.now())
    time2 = time.perf_counter()
    tim=time2-time1

    if rsp==bytes("succ",encoding='utf-8'):
        print("dealid ",dealid," scuess")

    else:
        error_number += 1
        result[dealid] = msg_no
        print("wrong 结果:", dealid, rsp.get_raw_response(),tim)

if __name__ == '__main__':
    error_number = 0
    result = {}
   #方法一 线程池方式
    executor = ThreadPoolExecutor(max_workers=100)
    urls=[]
    for i in range(0,20):
       urls.append(i)
   # 通过submit函数提交执行的函数到线程池中，submit函数立即返回，不阻塞
    time1 = time.perf_counter()
    all_task = [executor.submit(thread, (str(url))) for url in urls]
    time2 = time.perf_counter()
    tim=time2-time1
   

   #方法二，threading库方式
    urls=[]
    for i in range(0,20):
       urls.append(i)
    threads = []
    time1 = time.perf_counter()
    for url in urls:
        t = threading.Thread(target=thread,args=(url,))
        t.start()
        threads.append(t)
    for th in threads:
        th.join()

    time2 = time.perf_counter()
    tim=time2-time1
    print("错误数量：", error_number, "总数：100", "通过率:", (100 - error_number) / 100,"请求时间：",tim)

```
{: file="thread.py"}





