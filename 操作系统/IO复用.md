# IO复用

> **UNIX网络编程卷1:套接字 139页**

1. Unix可用的5中IO模型
   
   - 阻塞式IO
   
   - 非阻塞式IO
   
   - IO复用（select/epoll)
   
   - 信号驱动式IO
   
   - 异步IO（POSIX的aio_系列函数）

2.  一个输入操作包括两个不同阶段

   1. 等待数据准备好

   2. 从内核复制数据到用户态进程中

3. IO模型
   1. **阻塞式IO模型**

      系统调用直到数据报到达且复制到应用进程缓冲区或错误时才返回（不阻塞  

![](https://raw.githubusercontent.com/zyb-992/Photobed/master/zyb/202209111656833.png)

   2-  **非阻塞式IO模型**

      将套接字设置为非阻塞是通知内核：当请求的IO操作需要把进程投入睡眠才能完成时 不用将进程投入睡眠 而是返回一个错误 如此反复轮询 直到数据准备好 此时复制到应用进程缓冲区 然后recvfrom系统调用成功返回 


  			![	](D:\Program%20Files\电子书\go\md\image\2022-07-24-22-59-07-image.png)		

3. **IO复用模型**
         

      阻塞于select函数调用 等待某个套接字可读 当该函数返回某个套接字可读条件时 函数返回并调用recvfrom把所读数据报复制到应用进程缓冲区
      
      `调用select监视 -> 返回 -> 调用recvfrom读取`
      


​		  ![](https://raw.githubusercontent.com/zyb-992/Photobed/master/zyb/202209111656834.png)			   

4.  **信号驱动式IO**
           ![](https://raw.githubusercontent.com/zyb-992/Photobed/master/zyb/202209111656835.png)

      使用信号 当内核在描述符准备就绪（可读 发送SIGIO信号通知客户

5. **异步IO**

      告知内核启动某操作 并让内核在整个操作（**包括将数据从内核复制到我们自己的缓冲区**）完成后通知客户
      
      与信号驱动式IO的区别是 
      
      - 异步模型是由内核通知我们IO操作何时完成（不需要我们调用系统函数去读取数据 内核帮我们
      
      - 信号驱动式IO是内核告知我们数据准备好了 可以启动某个IO操作进行数据读取

​		  ![](https://raw.githubusercontent.com/zyb-992/Photobed/master/zyb/202209111656836.png)	

## IO模型比较

- 真正的IO操作将阻塞进程 而异步IO模型不需要我们执行IO操作

- 同步IO操作：导致请求进程阻塞 直到IO操作完成
  
  - 阻塞IO/非阻塞IO/IO复用/信号驱动式IO

- 异步IO操作：不导致请求进程阻塞
  
  - 异步IO模型

![](https://raw.githubusercontent.com/zyb-992/Photobed/master/zyb/202209111656837.png)





### select函数

1. 

![](https://raw.githubusercontent.com/zyb-992/Photobed/master/zyb/202209111656838.png)

![](https://raw.githubusercontent.com/zyb-992/Photobed/master/zyb/202209111656839.png)

