# Core Java Questions :

## Concurracncy ::
-----------------------------------------------
### 1. Interthread communication between the threads.
### 2. What is the deadlock and how to prevent it ?
- When we are working with the multiple threads and if we applied some bad synchronization then there might be an having the deadlock situation.
- In the deadlock situtation suppose that one thread is working on the resouce1 and it taken an lock on it but to proceed with the execution it needs to access the  resource 2 which lock has been taken by the thread 1. and vice versa the thread 2 is taken lock on the resource 2 and waiting for thread 1 to release lock on the resource 1.
- Here the both threads are waiting on the each other so it will be an deadlock situation.

<ins> Prevention </ins> : 
1. Firstly developer needs to be take care while coding that is there any unessesary lock has been taken or not.
2. Dev should avoid of adding nessted synchronization blocks
3. Whenever we are working on the multiple threads we should be join then so that if any deadlock situation then it will not be wait for the indefinitly.

<ins> Example </ins>  : 
```
public static void main(String[] args) throws InterruptedException {
        {
            final String r1 = "edureka";
            final String r2 = "java";
            Thread t1 = new Thread()
            {
                public void run()
                {
                    synchronized(r1)
                    {
                        System.out.println("Thread 1: Locked r1");
                        try
                        { Thread.sleep(100);} catch(Exception e) {}
                        synchronized(r2)
                        {
                            System.out.println("Thread 1: Locked r2");
                        }
                    }
                }
            };
            Thread t2 = new Thread()
            {
                public void run()
                {
                    synchronized(r2)
                    {
                        System.out.println("Thread 2: Locked r2");
                        try{ Thread.sleep(100);} catch(Exception e) {}
                        synchronized(r1)
                        {
                            System.out.println("Thread 2: Locked r2");
                        }
                    }
                }
            };
            t1.start();
            t2.start();
            t1.join(100);
            t2.join(100);
        }
```
links for reference : [https://www.adservio.fr/post/deadlock-in-java]

### 3. What is race condition and critical section in java ? 

#### <ins> Race Condition </ins> : 
- Is the problem occure while working with the multi threaded enviorment. If suppose that there are the multiple threads are working on the same piece of the code or share the same resources at the same time so it will be causing an issue with wither the data inconsistency or any problem bug which can be introduced in the application were this problem is called as the Race condition.
- This can be introduce because of the improper synchronization.
Therea re two types of the race conditions :
1. Read-modify-write :
One thread is read the value and modify the value which has been read but before it write to the memory another thread is accessed this old value and process on it. So new thread is having an old value which was not committed by the thread one.

2. Check-Then-Act :
One thead is accessed the value and performing some operation on it but mean time another tthread is modifed the same value which is then not available for the previous thread who accessed that value.

#### <ins> Critical Section </ins> :
- Critical section is we can say that the piece of the code which is getting shared with the mutliple thread which can cause the race confition problem.

#### <ins> Prevention </ins> :
- To avoid of the race condition we have to make sure that there is the proper synchronization for the critical section of the code.
- So that one thread should be access at the one time for the critical section of the code.

### 4. Why wait , notify and notifyAll methods should be called inside the synchronization block ? 
- Basically this methods are used for the inter comuunication between the threads.
- Wait : Method is used for the keep waiting the current thread and release the lock and allow other thread to process.
- Notify : Method is used to notify the waiting thread on the same object so that waiting thread can reaquire the lock and proceed with the execution. If there are multiple threads are waiting on the same object then in that case internal alogehythm will decide which thread to get the chance.
- Notify All : method is used for the notifying the all the threads which are waiting on the same object.

- So in this case if suppose that we have not made the wait and notify thread safe by applying on the synch block. then when both the threads are started parllely then if we take an example of producer and consumer producer adding the data and before notifying consumer is checking that the data is not avaialble and it notify the prosucer to add the data and due to this producer miss this notification and add the data and keep in waiting state.
- To avoid this mentioned above problem we should add the synchronization block for the wait notify and notifyAll method.

### 5. Difference between the wait and Sleep Method ? 
#### <ins> Wait </ins> :
1. Release lock : so that other thread can access the data.
2. Wait can be wakes up when there is notification from the other thread on the same object.

#### <ins> Sleep </ins> :
1. Not Releases the lock. If thread is on sleep another thread can not proceed on the critical section of the code.
2. Can come out from the sleep based on the time added.
  
### 6. How to stop the threads ? 
- So there are two options of the stopping the threads
1. We can call the Thread.stop() method on the running thread. But, when we stop the thread it will be prematured death of the thread. Which may cause the corruption of the data which is getting shared with the other threads or the resources. Like if suppose that one thread is working on resources for updating the value and the meantime if we stop the thread it may corrupte the resource.
2. Another way is that intruptting the thread by calling of the Thread.intruptt method. In this case for the running threads we have to provide the handling for the intrupption by checking the flag from Thread.intruptted() method. There is not an any kind of the universal implementation for this but you can add the logic in context of the thread clean up.
3. Also if the thread is in the blocking state and you intruptted that thread then it will be an throw the IntrupttedException. This is the special kind of the exception which indicate the intupttion of the thread which we can handle and provide the cleanup implementation.

Reference Link : [https://4comprehension.com/how-to-stop-a-java-thread-without-using-thread-stop/]

### 7. Whats Yield method from thread ? 
- This method is basically used for suspending of the currrent thread execution and providing an chance to execute other same / higher priority thread.
- When you call the yield method then it hint for the thread scheduler that current thread is not doing any critical work so if you want you can give chance to other same / higer priority thread to get executed. It's sechedulers job whether to ignore or act on the hint.
- if there are not same or higher priority thread then it will be continue the execution.
- There is very rear chace whether we can apply the yeild method behavior majrly it will be used for the debugging purpose.

### 8. Difference betn Yeild and Sleep ? 
- Main difference is sleep throws an intruppted exception if another thread is intruptte the sleeping thread. But, yeild does not.
- Sleep is for the specific time frame. Yeild suspend the thread unit complete the higher priority thread task.

### 9. ThreadLocal Varaibles in java ?
- ThreadLocal varaibles are created for the thread specific the value which is updated in that thread local is accessible to the specific thread only. It will not be available for the other threads.
- ThreadLocal varaibles are stored in the thread stack which is seperate for the every thread.
- We can create the thread local varaible and set and remove the value from it. Also if we want to create the ThreadLocal varaibles withInitial value then you can create by overriding of the initialValue method from threadlocal

<ins> Example </ins>: 
```
private ThreadLocal<Object> threadLocal =
         new ThreadLocal<Object>();
threadLocal.set( (int) (Math.random() * 100D) );
```

### 10. What is Join Method from the Thread ? 
- This method is used for to wait the current thread to complete the execution of the thread on which join method is called. which means that it will be wait for the another thread to dead.
- You can provide the milliseconds and nons in the join method which will be wait for the specific time frame to compelete the execution of the thread.

Reference link : [https://www.edureka.co/blog/join-method-java]

### 11. Java Memory model : MultiThreading 
- ThreadStack is the stack which is created for the every thread. All the local varaibles and primitive varaibles and function calls are stored in the thread stack. also the object which is created in the local varaibles reference is stored in the thread stack but actual object is created in the heap memory only.
- ThreadStack of the one thread is not accessible or visible to the other thread.
- All the varaibles created in the main memory and accessing from that main memory is very expensive operation. So in this case there will be the cache memory in the CPU chip it self who is making an copy of the varaibles to the cache and work on it to optimize the performance of the application.
- But, if multiple thread is working then one thread update will not be avaialable for the another thread as cache is thread specific.
- In this case we have to implement the propert synchronization and atomicity to avoid this behaviour of the Threads.

### 12. Whats is starvation and livelock ? 
#### <ins> Starvation </ins> :
- When there are multiple threads are running parallely and some are lower priority thread and some are the higher priority threads. And they are sharing the common resources amongest the all threds.
- But as due to the higher priority threads are getting an chance to run first and it will be executing the critical section of the code which is in the synch block and due to the higher priority thread taken an lock on it lower priority thread is not getting an chance to run.
- Also synch block is taking an too much time to execute and higher priority thread is executing the critical section of the code frequently due to this lower priority of the thread is not getting an chance to execute/process.
- This is called as starvation. which can be due to the im proper resource allowcation.

#### <ins> LiveLock </ins> :
- Live lock is an similar to the deadlock. In the dead lock the threads are waiting on each other to release the lock. But in the case of the livelock the both thread 1 and thread 2 are doing the same action recurcuvelly which is due to the threads are not getting chance to execute.
- In this case like dead lock threads are not in the blocking state they are running but taking same action.
- Liek an e.g of walking of the mens in corridor from the opposite side of each other and they are going left and right at the same time to give an chance to proceed another one. which is happening same with the threads they are giving an chance to run at the same time by releasing an lock.

### 13. Countdownlatch and cyclicBarrier in Thread 
This are the patterns based on which we can wait the maiin thread till the task which is provided to the other threads completed.
#### <ins> Countdownlatch </ins> :
- In this case we are creating the count down latch by mentioning of the number of task which are goiing to executed in the thread. And we are passing this latch to the every thread. And keeps the main thread await.
```
CountDownLatch latch = new CountDownLatch(1);
            latch.await(); //  keeps main thread await
            latch.countDown(); // pass this to the every thread and down the latch once done 
```
- So once the every thread completes their operation then it will be down the latch and once the latch is reaches to the zero it means that the all task has been completed and main thread can proceed to the execution.

#### <ins> CyclicBarrier </ins> :
- This is the same as the count down latch it actuall wait the thread till all threads are complete theier operations.
- Like here it will create the cyclic barrier with the number of threads which are executing and then pass this to the every thread.
- And every thread will call the await method once it complete it execution.
- And when the await count of the thread is matches with the count provided in the cyclic barrier then it will be continue the main thread operation.

```
CyclicBarrier barrier = new CyclicBarrier(10);
            barrier.await();
```

### 14. How many threads should be created ?  how to decide this ?
- This can be decided by the different factors :
1. Based on the applications architecture. Thanks how many threads your application is required to process this specific task i the parllel.
2. Based on the CPU core. like whatever the resources and core are avaiable for your system where the application is rnning.
3. There are some limitations on the os as well that you can create that many threads at the time.
4. There are other limitations as well that suppose we have the thread pool with the 200 threads to make the connection to the data base but the data base connections are limit of 100 parrlel session at the same time.
5. So there are couple of the resources limit which we should be take in mind also other limitation can be vary based on the senarios and application architecture.





