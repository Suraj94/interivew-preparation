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

#### <ins> Critical Section </ins> :
- Critical section is we can say that the piece of the code which is getting shared with the mutliple thread which can cause the race confition problem.





