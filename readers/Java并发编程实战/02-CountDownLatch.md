####CountDownLatch作用
用于等待一个或者一组线程完成操作，以便进行下个操作。

比如，创建一个数量为10的ContDownLatch，当10个线程都完成任务后，再执行其它操作。


####源代码分析
#####构造函数
1.	CountDownLatch(int count)

内部采用了AQS的实现Sync。

	public boolean tryReleaseShared(int releases) {
            // Decrement count; signal when transition to zero
            for (;;) {
                int c = getState();
                if (c == 0)
                    return false;
                int nextc = c-1;
                if (compareAndSetState(c, nextc))
                    return nextc == 0;
            }
        }
        
 通过无线循环以及原子操作CAS来做资源的释放。
 
 
 		public int tryAcquireShared(int acquires) {
            return getState() == 0? 1 : -1;
        }
tryAcquireShared用于await()方法中，await()调用tryAcquireShared方法判断当前技术是否为0，否则一直等待，直到为0.

#####主要方法
1.	countDown() 递减计数，当为0后，则唤醒所有等待的线程
2.	await() 在计数器为0前一直等待
3.	await(long timeout, TimeUnit unit) 在计数器为0前一直等待，除非过了指定的时间
4.	getCount() 返回当前计数


#####源代码
启动3个工作线程，直到3个线程都完成后，输出工作完成的实例。

[源代码](https://github.com/llohellohe/cp/blob/master/src/yangqi/jcp/latch/CountDownLatchTest.java)


#####参考资料
[深入浅出 Java Concurrency (10): 锁机制 part 5 闭锁 (CountDownLatch)](http://www.blogjava.net/xylz/archive/2010/07/09/325612.html)