#生产者与消费者问题


----------


##    *概念
  
   >    有一个生产者在生产产品，这些产品将提供给若干个消费者去消费，为了使生产者和消费者能并发执行，在两者之间设置一个具有多个缓冲区的缓冲池，生产者将它生产的产品放入一个缓冲区中，消费者可以从缓冲区中取走产品进行消费，显然生产者和消费者之间必须保持同步，是即不允许消费者到一个空的缓冲区中取产品，也不允许生产者向一个已经放入产品的缓冲区中再次投放产品.

## *解决方法

 1. 利用记录型信号量解决生产者-消费者问题
 
 >先来看一段伪代码(**信号量机制原理**)

        Var mutex,empty,full:semaphore:=1,n,0;  // 定义三个信号量
        buffer:array[0,...,n-1]of item;  // 定义缓冲池，容量为n
        in,out:integer:=0,0;
        begin
            parbegin
                proceducer:begin // 生产者
	                    repeat
	                    .
	                    .
	                    .
	                    producer an item nextp; // 生产一个产品
	                    .
	                    .
	                    .
	                    wait(empty);   // 申请一个空缓冲区
	                    wait(mutex);  // 申请缓冲池的使用权
	                    buffer(in):=nextp; // 将产品放入缓冲池中
	                    in:=(in+1)mod n;  // 下一个空缓冲区地址
	                    signal(mutex);  //释放缓冲池使用权
	                    signal(full);  // 释放一个满缓冲区
	                    until false;
	                end
	            consumer:begin
	                    repeat
	                    wait(full); //申请一个满缓冲区
	                    wait(mutex); // 申请缓冲池的使用权
	                    nextc:=buffer(out); //从缓冲池中取出产品
	                    out:=(out+1)mod n; //下一个满缓冲区的地址
	                    signal(mutex); //释放缓冲池使用权
	                    signal(empty); //释放一个空缓冲区
	                    consumer the item in nextc; //消费一个产品
	                    until false;
	                end
	        parend
	    end

nextp 应该是next proceducer的意思
nextc 应该是next consumer的意思.

 貌似也不是什么变量，属于语言描述而已

>  至于生产者进程如何被阻塞和唤醒，因为程序中有一个 repeat语句，所以进程不断测试**缓冲池是否有空缓冲区**，以及**缓冲池是否有其他进程使用**。若两个条件不满足，则进入阻塞队列等待。若某一时刻两个条件都能满足，则能唤醒该进程。

##code
>>未完成
 


 
 2. 利用AND信号量解决生产者和消费者问题
 
        @@@@
        
 3. 利用管程解决生产者和消费者问题
    
        @@@@

