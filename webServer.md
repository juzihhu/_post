---
title: 轻量级web服务器
date: 2022-07-20 20:48:44
tags: web server
categories: Linux
cover: /img/photo-1676979526732-cd3b818d3135.png
---
轻量级web服务器
<!-- more -->   

# 第1章 开发环境搭建

## Linux开发环境搭建

- 远程服务器准备/虚拟机安装Linux系统

- VM tools

  ![image-20230315112636669](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151936203.png)

  桌面会生成一个VMware Tools磁盘，双击打开

  

  ![image-20230315113228023](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151936670.png)

  将里面的压缩包拖拽到桌面，右键使用归档管理器打开

  ![image-20230315113339605](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151936044.png)

  点击提取到桌面

  ![image-20230315113701597](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151936261.png)

  

  ![image-20230315113753858](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151936835.png)

  

  ![image-20230315113821764](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151937664.png)

  找到vmware-install.pl文件，右键在终端中打开

  ![image-20230315114100054](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151937204.png)

  

- Xshell 远程连接
  > xshell 安装运行后闪退
  >
  > 找到【flexnet licensing service】服务然后停止，右击属性，把启动类型改成禁止就可以
  >
  > [https://blog.csdn.net/qq_34231823/article/details/120260491](https://blog.csdn.net/qq_34231823/article/details/120260491)

- VS Code远程连接设置
  > 1. 安装远程服务ssh：sudo apt install openssh-server;
  >
  >    可以先创建一个用户  sudo adduser username
  >
  >    ![image-20230315115829207](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151937358.png)
  >
  > 2. xshell中新建会话：主机为云服务器的公网IP或者填Linux系统IP地址
  >
  >    ![image-20230315114313047](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151937477.png)
  >
  > 3. Xftp 用于与服务器文件传输
  >
  > 4. 远程连接
  >    - 安装Remote Development
  >
  >         ![image-20230315120053120](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151937826.png)
  >
  >    - 设置config
  >
  >         ![image-20230315120118016](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151937817.png)
  >
  >         hostname 为主机ip地址
  >
  >         ![image-20230315120156866](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151941496.png)
  >
  >    - 设置免密登录:   
  >
  >         将本机公钥发送给虚拟机 .ssh/目录下
  >         
  >         在虚拟机(云服务器）生成ssh key: ssh-keygen -t rsa
  >         
  >         ![image-20230315193615100](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151942683.png)
  >         
  >         vim authorized_keys 将本机公钥复制到该文件，保存退出
  >         
  >         ![image-20230315194152962](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151942169.png)
  >         
  >         ![image-20230315194250787](https://gitee.com/juzihhu/image_bed/raw/master/img/202303151942346.png)

  

  问题：使用sudo报错提示

  user is not in the sudoers file. This incident will be reported

  解决办法：

  https://blog.csdn.net/csdnzouqi/article/details/95499348

  ![image-20230315203253437](https://gitee.com/juzihhu/image_bed/raw/master/img/202303152032967.png)

  ```shell
  panan@ubuntu18:~$ su - root
  Password: 
  root@ubuntu18:~# 
  root@ubuntu18:~# chmod 640 /etc/sudoers
  root@ubuntu18:~# ll etc/sudoers
  root@ubuntu18:~# ll /etc/sudoers
  -rw-r----- 1 root root 755 Jan 18  2018 /etc/sudoers
  root@ubuntu18:~# # vim /etc/sudoers
  root@ubuntu18:~# ll /etc/sudoers
  -rw-r----- 1 root root 780 Mar 15 20:22 /etc/sudoers
  #修改sudoers文件：只读权限（原有权限）
  root@ubuntu18:~# chmod 440 /etc/sudoers
  root@ubuntu18:~# ll /etc/sudoers
  -r--r----- 1 root root 780 Mar 15 20:22 /etc/sudoers
  root@ubuntu18:~# exit
  logout
  
  ```

  

  

  设置sudo不需要每次都输入密码：`sudo vi /etc/sudoers`

  ![image-20230315210121542](https://gitee.com/juzihhu/image_bed/raw/master/img/202303152101593.png)

  

  mysql 安装：

  参考博客：http://www.tuohang.net/article/251176.html
  
- 为了安装MySQL，需要在你的服务器上通过apt更新包索
  sudo apt update

- 安装默认软件包：
  	
  
  ```shell
  sudo apt install mysql-server
  apt-get isntall mysql-client
  sudo apt-get install libmysqlclient-de
  ```
  
  
  
- 配置MySQL

​	运行安全脚本：sudo mysql_secure_installation

> 该脚本将通过一系列的提示帮你完成MySQL安装安全选项的变更。第一个提示将询问你是否愿意安装密码检测插件，该插件用来测试你设置的MySQL密码的强壮性。无论你如何选择，下一个提示是让你设置MySQL root用户的密码。回车，然后需要确认你输入的密码。

安装过程中会提示设置密码什么的，注意设置了不要忘了，安装完成之后可以使用如下命令来检查是否安装成功：

```shell
sudo netstat -tap | grep mysql
```

通过上述命令检查之后，如果看到有mysql 的socket处于 listen 状态则表示安装成功。

fatal error: mysql/mysql.h: No such file or directory

没有安装`mysql`的相关链接库

```shell
sudo apt-get install libmysqlclient-dev
```

```mysql
mysql> update mysql.user set authentication_string=PASSWORD("123456"), plugin="mysql_native_password" where user="root";
```

报错: ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

mysql安装了validate_password密码校验插件，导致要修改的密码不符合密码策略的要求

查看当前的密码策略是：

SHOW VARIABLES LIKE 'validate_password%';



  ##  GCC

**简介**

> GNU C Compiler
> 是由GUN开发的编程语言编译器
> 安装： `sudo apt install gcc g++`
> ` gcc/g++ -v/--version`
> ![编译过程](https://gitee.com/juzihhu/image_bed/raw/master/img/%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B.png)
> ![GCC工作流程](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245004.png)
>
> 生成运行第一个c++程序
>
> ![image-20230315210245932](https://gitee.com/juzihhu/image_bed/raw/master/img/202303152102876.png)

# 第2章 Linux系统编程

# 第3章 Linux多线程开发

##  线程概述

- *与进程（process）类似，线程（thread）是允许应用程序并发执行多个任务的一种机制。一个进程可以包含多个线程。同一个程序中的所有线程均会独立执行相同程序，且共享同一份全局内存区域，其中包括初始化数据段、未初始化数据段，以及堆内存段。（传统意义上的 UNIX 进程只是多线程程序的一个特例，该进程只包含一个线程*
  
### 进程与线程

  1. *根本区别：进程是操作系统资源分配的基本单位，而线程是处理器任务调度和执行的基本单位；*
  2. *资源开销：每个进程都有独立的代码和数据空间（程序 ），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小*
  3. *内存分配：同一进程的线程共享本进程的地址空间和资源，而进程之间的地址空间和资源是相互独立的；*
  4. *影响关系：一个进程崩溃后，在保护模式下不会对其他进程产生影响，但是一个线程崩溃整个进程都死掉。所以多进程要比多线程健壮；*


- *线程是轻量级的进程（LWP： Light Weight Process），在 Linux 环境下线程的本质仍是进程。*
- *查看指定进程的 LWP 号： ps –Lf pid*

- 进*程间的信息难以共享。由于除去只读代码段外，父子进程并未共享内存，因此必须采用一些进程间通信方式，在进程间进行信息交换。*
- *调用 fork() 来创建进程的代价相对较高，即便利用写时复制技术，仍然需要复制诸如内存页表和文件描述符表之类的多种进程属性，这意味着 fork() 调用在时间上的开销依然不菲。*
  
- *线程之间能够方便、快速地共享信息。只需将数据复制到共享（全局或堆）变量中即可。*
- *创建线程比创建进程通常要快 10 倍甚至更多。线程间是共享虚拟地址空间的，无需采用写时复制来复制内存，也无需复制页表*
  
### 线程之间共享和非共享资源

|共享资源| 非共享资源 |
| :----:| :---:|
|进程 ID 和父进程 ID|线程 ID|
| 进程组 ID 和会话 ID|信号掩码|
| 用户 ID 和 用户组 ID|线程特有数据|
|文件描述符表|线程特有数据|
| 信号处置|实时调度策略和优先级|
| 文件系统的相关信息：文件权限掩码（umask）、当前工作目录|栈，本地变量和函数的调用链接信息|
|虚拟地址空间（除栈、 .text）| |

*NPTL，或称为 Native POSIX Thread Library，是 Linux 线程的一个新实现
它克服了LinuxThreads 的缺点，同时也符合 POSIX 的需求。与 LinuxThreads相比,
它在性能和稳定性方面都提供了重大的改进。
查看当前 pthread 库版本： getconf GNU_LIBPTHREAD_VERSION*

## pthread_create()线程创建

```c++
/*
    一般情况下,main函数所在的线程我们称之为主线程（main线程），其余创建的线程
    称之为子线程。
    程序中默认只有一个进程，fork()函数调用，2进程
    程序中默认只有一个线程，pthread_create()函数调用，2个线程。

    #include <pthread.h>
    int pthread_create(pthread_t *thread, const pthread_attr_t *attr, 
    void *(*start_routine) (void *), void *arg);

        - 功能：创建一个子线程
        - 参数：
            - thread：传出参数，线程创建成功后，子线程的线程ID被写到该变量中。
            - attr : 设置线程的属性，一般使用默认值，NULL
            - start_routine : 函数指针，这个函数是子线程需要处理的逻辑代码
            - arg : 给第三个参数使用，传参
        - 返回值：
            成功：0
            失败：返回错误号。这个错误号和之前errno不太一样。
            不能用perror("")输出错误号信息
            获取错误号的信息：  char * strerror(int errnum);
        - Compile and link with -pthread 
            编译时需要指定静态库
            gcc pthread_create.c -o create -l pthread
            或
            gcc pthread_create.c -o create -pthread

*/
#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <unistd.h>
void * callback(void * arg) {
    printf("child thread...\n");
    printf("arg value: %d\n", *(int *)arg);
    return NULL;
}
int main() {
    pthread_t tid;
    int num = 10;
    // 创建一个子线程
    int ret = pthread_create(&tid, NULL, callback, (void *)&num);
    if(ret != 0) {
        // 传入错误号获取错误信息
        char * errstr = strerror(ret);
        printf("error : %s\n", errstr);
    } 
    for(int i = 0; i < 5; i++) {
        printf("%d\n", i);
    }
    sleep(1);
    return 0;   // exit(0);
}
```

##  终止线程pthread_exit()

```c++
/*
    #include <pthread.h>
    void pthread_exit(void *retval);
        功能：终止一个线程，在哪个线程中调用，就表示终止哪个线程
        参数：
            retval:需要传递一个指针，作为一个返回值，可以在pthread_join()中获取到。s

    pthread_t pthread_self(void);
        功能：获取当前的线程的线程ID

    int pthread_equal(pthread_t t1, pthread_t t2);
        功能：比较两个线程ID是否相等
        不同的操作系统，pthread_t类型的实现不一样，有的是无符号的长整型，有的
        是使用结构体去实现的。
        RETURN VALUE
        If the two thread IDs are equal, pthread_equal() returns a nonzero value; otherwise, it returns 0.
*/
#include <stdio.h>
#include <pthread.h>
#include <string.h>

void * callback(void * arg) {
    printf("child thread id : %ld\n", pthread_self());
    return NULL;    // pthread_exit(NULL);
} 

int main() {

    // 创建一个子线程
    pthread_t tid;
    int ret = pthread_create(&tid, NULL, callback, NULL);

    if(ret != 0) {
        char * errstr = strerror(ret);
        printf("error : %s\n", errstr);
    }

    // 主线程
    for(int i = 0; i < 5; i++) {
        printf("%d\n", i);
    }

    printf("tid : %ld, main thread id : %ld\n", tid ,pthread_self());

    // 让主线程退出,当主线程退出时，不会影响其他正常运行的线程。
    pthread_exit(NULL);

    printf("main thread exit\n");

    return 0;   // exit(0);
}
```
## 连接终止线程pthread_join()

```c++
/*
    #include <pthread.h>
    int pthread_join(pthread_t thread, void **retval);
        - 功能：和一个已经终止的线程进行连接
                回收子线程的资源
                这个函数是阻塞函数(子线程没有结束该函数就不执行)，调用一次只能回收一个子线程
                一般在主线程中使用
        - 参数：
            - thread：需要回收的子线程的ID
            - retval: 接收子线程退出时的返回值

                void pthread_exit(void *retval);
                    功能：终止一个线程，在哪个线程中调用，就表示终止哪个线程
                    参数：retval:需要传递一个指针，作为一个返回值，可以在pthread_join()中获取到

            - void** 可以改变传入指针的值
                #include<stdlib.h>
                void change(int** a){
                    int b=33;
                    *a=&b;
                }
                int main(){
                    int a=20;
                    int* data=&a;
                    // 需要传入该指针的地址才能修改
                    change(&data);
                    printf("*data=%d",*data);
                    return 0;
                }

        - 返回值：
            0 : 成功
            非0 : 失败，返回的错误号
*/

#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <unistd.h>

int value = 10;

void * callback(void * arg) {
    printf("child thread id : %ld\n", pthread_self());
    // sleep(3);
    // return NULL; 
    // int value = 10; // 局部变量
    pthread_exit((void *)&value);   // return (void *)&value;
} 

int main() {

    // 创建一个子线程
    pthread_t tid;
    int ret = pthread_create(&tid, NULL, callback, NULL);

    if(ret != 0) {
        char * errstr = strerror(ret);
        printf("error : %s\n", errstr);
    }

    // 主线程
    for(int i = 0; i < 5; i++) {
        printf("%d\n", i);
    }

    printf("tid : %ld, main thread id : %ld\n", tid ,pthread_self());

    // 主线程调用pthread_join()回收子线程的资源
    int * thread_retval;
    ret = pthread_join(tid, (void **)&thread_retval);

    if(ret != 0) {
        char * errstr = strerror(ret);
        printf("error : %s\n", errstr);
    }

    printf("exit data : %d\n", *thread_retval);

    printf("回收子线程资源成功！\n");

    // 让主线程退出,当主线程退出时，不会影响其他正常运行的线程。
    pthread_exit(NULL);

    return 0; 
}
```
##  线程分离pthread_detach()

```c++
/*
    #include <pthread.h>
    int pthread_detach(pthread_t thread);
        - 功能：分离一个线程。被分离的线程在终止的时候，会自动释放资源返回给系统。
          1.不能多次分离，会产生不可预料的行为。
          2.不能去连接一个已经分离的线程，会报错。
        - 参数：需要分离的线程的ID
        - 返回值：
            成功：0
            失败：返回错误号
*/
#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <unistd.h>

void * callback(void * arg) {
    printf("chid thread id : %ld\n", pthread_self());
    return NULL;
}

int main() {

    // 创建一个子线程
    pthread_t tid;

    int ret = pthread_create(&tid, NULL, callback, NULL);
    if(ret != 0) {
        char * errstr = strerror(ret);
        printf("error1 : %s\n", errstr);
    }

    // 输出主线程和子线程的id
    printf("tid : %ld, main thread id : %ld\n", tid, pthread_self());

    // 设置子线程分离,子线程分离后，子线程结束时对应的资源就不需要主线程释放
    ret = pthread_detach(tid);
    if(ret != 0) {
        char * errstr = strerror(ret);
        printf("error2 : %s\n", errstr);
    }

    // 设置分离后，对分离的子线程进行连接 pthread_join()
    // ret = pthread_join(tid, NULL);
    // if(ret != 0) {
    //     char * errstr = strerror(ret);
    //     printf("error3 : %s\n", errstr);
    // }

    pthread_exit(NULL);

    return 0;
}
```
##  线程取消pthread_cancel()

```c++
/*
    #include <pthread.h>
    int pthread_cancel(pthread_t thread);
        - 功能：取消线程（让线程终止）
            取消某个线程，可以终止某个线程的运行，
            但是并不是立马终止，而是当子线程执行到一个取消点，线程才会终止。
            取消点：系统规定好的一些系统调用，我们可以粗略的理解为从用户区到内核区的切换，这个位置称之为取消点。
*/

#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <unistd.h>

void * callback(void * arg) {
    printf("chid thread id : %ld\n", pthread_self());
    for(int i = 0; i < 5; i++) {
        printf("child : %d\n", i);
    }
    return NULL;
}

int main() {
    
    // 创建一个子线程
    pthread_t tid;

    int ret = pthread_create(&tid, NULL, callback, NULL);
    if(ret != 0) {
        char * errstr = strerror(ret);
        printf("error1 : %s\n", errstr);
    }

    // 取消线程
    pthread_cancel(tid);

    for(int i = 0; i < 5; i++) {
        printf("%d\n", i);
    }

    // 输出主线程和子线程的id
    printf("tid : %ld, main thread id : %ld\n", tid, pthread_self());

    
    pthread_exit(NULL);

    return 0;
}
```

##  线程属性pthread_attr_

输入命令**man pthread_attr_**+两次Tab 查看属性相关操作

```c++
/*
    int pthread_attr_init(pthread_attr_t *attr);
        - 初始化线程属性变量

    int pthread_attr_destroy(pthread_attr_t *attr);
        - 释放线程属性的资源

    int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);
        - 获取线程分离的状态属性

    int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate);
        - 设置线程分离的状态属性
        PTHREAD_CREATE_DETACHED
              Threads that are created using attr will be created in a detached state.

        PTHREAD_CREATE_JOINABLE
              Threads that are created using attr will be created in a joinable state.
*/     

#include <stdio.h>
#include <pthread.h>
#include <string.h>
#include <unistd.h>

void * callback(void * arg) {
    printf("chid thread id : %ld\n", pthread_self());
    return NULL;
}

int main() {

    // 创建一个线程属性变量
    pthread_attr_t attr;
    // 初始化属性变量
    pthread_attr_init(&attr);

    // 设置属性
    pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);

    // 创建一个子线程
    pthread_t tid;

    int ret = pthread_create(&tid, &attr, callback, NULL);
    if(ret != 0) {
        char * errstr = strerror(ret);
        printf("error1 : %s\n", errstr);
    }

    // 获取线程的栈的大小
    size_t size;
    pthread_attr_getstacksize(&attr, &size);
    printf("thread stack size : %ld\n", size);

    // 输出主线程和子线程的id
    printf("tid : %ld, main thread id : %ld\n", tid, pthread_self());

    // 释放线程属性资源
    pthread_attr_destroy(&attr);

    pthread_exit(NULL);

    return 0;
}
```

##  线程同步

**线程同步**
*1. 线程的主要优势在于，能够通过全局变量来共享信息。不过，这种便捷的共享是有代价
的：必须确保多个线程不会同时修改同一变量，或者某一线程不会读取正在由其他线程
修改的变量。
2. 临界区是指访问某一共享资源的代码片段，并且这段代码的执行应为原子操作，也就是
同时访问同一共享资源的其他线程不应终端该片段的执行。
3. 线程同步：即当有一个线程在对内存进行操作时，其他线程都不可以对这个内存地址进
行操作，直到该线程完成操作，其他线程才能对该内存地址进行操作，而其他线程则处
于等待状态*

***线程同步一定程度会影响执行效率，但也只是对临界区的数据同步
整体上还是并发执行***

**售票案例**（*存在问题*）
```c++
/*
    使用多线程实现买票的案例。
    有3个窗口，一共是100张票。
*/

#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

// 全局变量，所有的线程都共享这一份资源。
// 如果作为回调函数局部变量，每个线程都会卖tickets张票
int tickets = 100;

void * sellticket(void * arg) {
    // 卖票
    while(tickets > 0) {
        //睡眠微秒
        // 出现卖同一张票或者负数票问题，三个线程可能同时走进while()
        usleep(6000);
        printf("%ld 正在卖第 %d 张门票\n", pthread_self(), tickets);
        tickets--;
    }
    return NULL;
}

int main() {

    // 创建3个子线程

    pthread_t tid1, tid2, tid3;
    pthread_create(&tid1, NULL, sellticket, NULL);
    pthread_create(&tid2, NULL, sellticket, NULL);
    pthread_create(&tid3, NULL, sellticket, NULL);

    // 回收子线程的资源,阻塞
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    pthread_join(tid3, NULL);

    // 设置线程分离。被分离的线程在终止的时候，会自动释放资源返回给系统
    // pthread_detach(tid1);
    // pthread_detach(tid2);
    // pthread_detach(tid3);

    pthread_exit(NULL); // 退出主线程

    return 0;
}
```

##  互斥锁

- *为避免线程更新共享变量时出现问题，可以使用互斥量（mutex 是 mutual exclusion
的缩写）来确保同时仅有一个线程可以访问某项共享资源。可以使用互斥量来保证对任意共
享资源的原子访问*  

- *互斥量有两种状态：已锁定（locked）和未锁定（unlocked）。任何时候，至多只有一
个线程可以锁定该互斥量。试图对已经锁定的某一互斥量再次加锁，将可能阻塞线程或者报
错失败，具体取决于加锁时使用的方法*

- *一旦线程锁定互斥量，随即成为该互斥量的所有者，只有所有者才能给互斥量解锁。一般情
况下，对每一共享资源（可能由多个相关变量组成）会使用不同的互斥量，每一线程在访问
同一资源时将采用如下协议：*

   1. 针对共享资源锁定互斥量   
   2. 访问共享资源
   3. 对互斥量解锁
- *如果多个线程试图执行这一块代码（一个临界区），事实上只有一个线程能够持有该互斥
量（其他线程将遭到阻塞），即同时只有一个线程能够进入这段代码区域，如下图所示：*
![互斥锁](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245005.png)

```c++
/*
    互斥量的类型 pthread_mutex_t
    int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr);
        - 初始化互斥量
        - 参数 ：
            - mutex ： 需要初始化的互斥量变量
            - attr ： 互斥量相关的属性，NULL
        - restrict : C语言的修饰符，被修饰的指针，不能由另外的一个指针进行操作。
            pthread_mutex_t *restrict mutex = xxx;
            pthread_mutex_t * mutex1 = mutex;

    int pthread_mutex_destroy(pthread_mutex_t *mutex);
        - 释放互斥量的资源

    int pthread_mutex_lock(pthread_mutex_t *mutex);
        - 加锁，阻塞的，如果有一个线程加锁了，那么其他的线程只能阻塞等待

    int pthread_mutex_trylock(pthread_mutex_t *mutex);
        - 尝试加锁，如果加锁失败，不会阻塞，会直接返回。

    int pthread_mutex_unlock(pthread_mutex_t *mutex);
        - 解锁
*/
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

// 全局变量，所有的线程都共享这一份资源。
int tickets = 10000;

// 创建一个互斥量
pthread_mutex_t mutex;

void * sellticket(void * arg) {

    // 卖票
    while(1) {

        // 加锁
        pthread_mutex_lock(&mutex);

        if(tickets > 0) {
            usleep(6000);
            //sleep(10);
            printf("%ld 正在卖第 %d 张门票\n", pthread_self(), tickets);
            tickets--;
        }else {
            // 解锁
            pthread_mutex_unlock(&mutex); 
            printf("票已售完！！！！\n");
            break;
        }

        // 解锁
        pthread_mutex_unlock(&mutex);
    }

    

    return NULL;
}

int main() {

    // 初始化互斥量
    pthread_mutex_init(&mutex, NULL);

    // 创建3个子线程
    pthread_t tid1, tid2, tid3;
    pthread_create(&tid1, NULL, sellticket, NULL);
    pthread_create(&tid2, NULL, sellticket, NULL);
    pthread_create(&tid3, NULL, sellticket, NULL);

    // 回收子线程的资源,阻塞
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    pthread_join(tid3, NULL);

    pthread_exit(NULL); // 退出主线程

    // 释放互斥量资源
    pthread_mutex_destroy(&mutex);

    return 0;
}
```
##  死锁

![死锁](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245006.png)

```c++
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

// 创建2个互斥量
pthread_mutex_t mutex1, mutex2;

void * workA(void * arg) {

    pthread_mutex_lock(&mutex1);
    sleep(1);
    pthread_mutex_lock(&mutex2);

    printf("workA....\n");

    pthread_mutex_unlock(&mutex2);
    pthread_mutex_unlock(&mutex1);
    return NULL;
}


void * workB(void * arg) {
    pthread_mutex_lock(&mutex2);
    sleep(1);
    pthread_mutex_lock(&mutex1);

    printf("workB....\n");

    pthread_mutex_unlock(&mutex1);
    pthread_mutex_unlock(&mutex2);

    return NULL;
}

int main() {

    // 初始化互斥量
    pthread_mutex_init(&mutex1, NULL);
    pthread_mutex_init(&mutex2, NULL);

    // 创建2个子线程
    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, workA, NULL);
    pthread_create(&tid2, NULL, workB, NULL);

    // 回收子线程资源
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);

    // 释放互斥量资源
    pthread_mutex_destroy(&mutex1);
    pthread_mutex_destroy(&mutex2);

    return 0;
}
```

##  读写锁

*当有一个线程已经持有互斥锁时，互斥锁将所有试图进入临界区的线程都阻塞住。但是考
虑一种情形，当前持有互斥锁的线程只是要读访问共享资源，而同时有其它几个线程也想
读取这个共享资源，但是由于互斥锁的排它性，所有其它线程都无法获取锁，也就无法读
访问共享资源了，但是实际上多个线程同时读访问共享资源并不会导致问题。*
- *在对数据的读写操作中，更多的是读操作，写操作较少，例如对数据库数据的读写应用。
为了满足当前能够允许多个读出，但只允许一个写入的需求，线程提供了读写锁来实现。*
- *读写锁的特点：*
    *1. 如果有其它线程读数据，则允许其它线程执行读操作，但不允许写操作。
    2. 如果有其它线程写数据，则其它线程都不允许读、写操作。
    3. 写是独占的，写的优先级高。*

**使用读写锁在读时可以让多个线程并发执行**

```c++
/*
    读写锁的类型 pthread_rwlock_t
    int pthread_rwlock_init(pthread_rwlock_t *restrict rwlock, const pthread_rwlockattr_t *restrict attr);
    int pthread_rwlock_destroy(pthread_rwlock_t *rwlock);
    int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock);
    int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock);
    int pthread_rwlock_wrlock(pthread_rwlock_t *rwlock);
    int pthread_rwlock_trywrlock(pthread_rwlock_t *rwlock);
    int pthread_rwlock_unlock(pthread_rwlock_t *rwlock);

    案例：8个线程操作同一个全局变量。
    3个线程不定时写这个全局变量，5个线程不定时的读这个全局变量
*/

#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

// 创建一个共享数据
int num = 1;
// pthread_mutex_t mutex;
pthread_rwlock_t rwlock;

void * writeNum(void * arg) {

    while(1) {
        pthread_rwlock_wrlock(&rwlock);
        num++;
        printf("++write, tid : %ld, num : %d\n", pthread_self(), num);
        pthread_rwlock_unlock(&rwlock);
        usleep(100);
    }

    return NULL;
}

void * readNum(void * arg) {

    while(1) {
        pthread_rwlock_rdlock(&rwlock);
        printf("===read, tid : %ld, num : %d\n", pthread_self(), num);
        pthread_rwlock_unlock(&rwlock);
        usleep(100);
    }

    return NULL;
}

int main() {

   pthread_rwlock_init(&rwlock, NULL);

    // 创建3个写线程，5个读线程
    pthread_t wtids[3], rtids[5];
    for(int i = 0; i < 3; i++) {
        pthread_create(&wtids[i], NULL, writeNum, NULL);
    }

    for(int i = 0; i < 5; i++) {
        pthread_create(&rtids[i], NULL, readNum, NULL);
    }

    // 设置线程分离
    for(int i = 0; i < 3; i++) {
       pthread_detach(wtids[i]);
    }

    for(int i = 0; i < 5; i++) {
         pthread_detach(rtids[i]);
    }

    pthread_exit(NULL);

    pthread_rwlock_destroy(&rwlock);

    return 0;
}
```
##  生产者和消费者模型

![生产者和消费者模型](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245007.png)



**存在问题：当没有数据了，消费线程依然在不断执行while(),资源浪费**
```c++
/*
    生产者消费者模型（粗略的版本）
*/
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <unistd.h>

// 创建一个互斥量
pthread_mutex_t mutex;

struct Node{
    int num;
    struct Node *next;
};

// 头结点
struct Node * head = NULL;

void * producer(void * arg) {

    // 不断的创建新的节点，添加到链表中
    while(1) {
        pthread_mutex_lock(&mutex);
        struct Node * newNode = (struct Node *)malloc(sizeof(struct Node));
        newNode->next = head;
        head = newNode;
        newNode->num = rand() % 1000;
        printf("add node, num : %d, tid : %ld\n", newNode->num, pthread_self());
        pthread_mutex_unlock(&mutex);
        usleep(100);
    }

    return NULL;
}

void * customer(void * arg) {

    while(1) {
        pthread_mutex_lock(&mutex);
        // 保存头结点的指针
        struct Node * tmp = head;

        // 判断是否有数据
        if(head != NULL) {
            // 有数据
            head = head->next;
            printf("del node, num : %d, tid : %ld\n", tmp->num, pthread_self());
            free(tmp);
            pthread_mutex_unlock(&mutex);
            usleep(100);
        } else {
            // 没有数据
            pthread_mutex_unlock(&mutex);
        }
    }
    return  NULL;
}

int main() {

    pthread_mutex_init(&mutex, NULL);

    // 创建5个生产者线程，和5个消费者线程
    pthread_t ptids[5], ctids[5];

    for(int i = 0; i < 5; i++) {
        pthread_create(&ptids[i], NULL, producer, NULL);
        pthread_create(&ctids[i], NULL, customer, NULL);
    }

    for(int i = 0; i < 5; i++) {
        pthread_detach(ptids[i]);
        pthread_detach(ctids[i]);
    }
    // 防止mutex被立即销毁
    // pthread_detach()是不阻塞的

    while(1) {
        sleep(10);
    }

    pthread_mutex_destroy(&mutex);

    pthread_exit(NULL);

    return 0;
}
```
**头插法理解**
![生头插法](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245008.png)

## 条件变量

```c++
/*
    条件变量的类型 pthread_cond_t
    int pthread_cond_init(pthread_cond_t *restrict cond, const pthread_condattr_t *restrict attr);
    int pthread_cond_destroy(pthread_cond_t *cond);
    int pthread_cond_wait(pthread_cond_t *restrict cond, pthread_mutex_t *restrict mutex);
        - 等待，调用了该函数，线程会阻塞。
    int pthread_cond_timedwait(pthread_cond_t *restrict cond, pthread_mutex_t *restrict mutex, const struct timespec *restrict abstime);
        - 等待多长时间，调用了这个函数，线程会阻塞，直到指定的时间结束。
    int pthread_cond_signal(pthread_cond_t *cond);
        - 唤醒一个或者多个等待的线程
    int pthread_cond_broadcast(pthread_cond_t *cond);
        - 唤醒所有的等待的线程
*/
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <unistd.h>

// 创建一个互斥量
pthread_mutex_t mutex;
// 创建条件变量
pthread_cond_t cond;

struct Node{
    int num;
    struct Node *next;
};

// 头结点
struct Node * head = NULL;

void * producer(void * arg) {

    // 不断的创建新的节点，添加到链表中
    while(1) {
        pthread_mutex_lock(&mutex);
        struct Node * newNode = (struct Node *)malloc(sizeof(struct Node));
        newNode->next = head;
        head = newNode;
        newNode->num = rand() % 1000;
        printf("add node, num : %d, tid : %ld\n", newNode->num, pthread_self());
        
        // 只要生产了一个，就通知消费者消费
        pthread_cond_signal(&cond);

        pthread_mutex_unlock(&mutex);
        usleep(100);
    }

    return NULL;
}

void * customer(void * arg) {

    while(1) {
        pthread_mutex_lock(&mutex);
        // 保存头结点的指针
        struct Node * tmp = head;
        // 判断是否有数据
        if(head != NULL) {
            // 有数据
            head = head->next;
            printf("del node, num : %d, tid : %ld\n", tmp->num, pthread_self());
            free(tmp);
            pthread_mutex_unlock(&mutex);
            usleep(100);
        } else {
            // 没有数据，需要等待
            // 当这个函数调用阻塞的时候，会对互斥锁进行解锁，当不阻塞的，继续向下执行，会重新加锁。
            pthread_cond_wait(&cond, &mutex);
            pthread_mutex_unlock(&mutex);
        }
    }
    return  NULL;
}

int main() {

    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond, NULL);

    // 创建5个生产者线程，和5个消费者线程
    pthread_t ptids[5], ctids[5];

    for(int i = 0; i < 5; i++) {
        pthread_create(&ptids[i], NULL, producer, NULL);
        pthread_create(&ctids[i], NULL, customer, NULL);
    }

    for(int i = 0; i < 5; i++) {
        pthread_detach(ptids[i]);
        pthread_detach(ctids[i]);
    }

    while(1) {
        sleep(10);
    }

    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

    pthread_exit(NULL);

    return 0;
}
```
##  信号量

```c++
/*
    信号量的类型 sem_t
    int sem_init(sem_t *sem, int pshared, unsigned int value);
        - 初始化信号量
        - 参数：
            - sem : 信号量变量的地址
            - pshared : 0 用在线程间 ，非0 用在进程间
            - value : 信号量中的值

    int sem_destroy(sem_t *sem);
        - 释放资源

    int sem_wait(sem_t *sem);
        - 对信号量加锁，调用一次对信号量的值-1，如果值为0，就阻塞

    int sem_trywait(sem_t *sem);

    int sem_timedwait(sem_t *sem, const struct timespec *abs_timeout);
    int sem_post(sem_t *sem);
        - 对信号量解锁，调用一次对信号量的值+1

    int sem_getvalue(sem_t *sem, int *sval);
    // 表示生产者可以生产的量
    sem_t psem;
    // 表示消费者可以消费的量
    sem_t csem;
    init(psem, 0, 8);
    init(csem, 0, 0);

    producer() {
        // 根据生产者信号量判断是否可以生产
        sem_wait(&psem);
        // 生产后通知消费者消费
        sem_post(&csem)
    }

    customer() {
        sem_wait(&csem);
        sem_post(&psem)
    }

*/

#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <unistd.h>
#include <semaphore.h>

// 创建一个互斥量
pthread_mutex_t mutex;
// 创建两个信号量
sem_t psem;
sem_t csem;

struct Node{
    int num;
    struct Node *next;
};

// 头结点
struct Node * head = NULL;

void * producer(void * arg) {

    // 不断的创建新的节点，添加到链表中
    while(1) {
        
        sem_wait(&psem);
        pthread_mutex_lock(&mutex);
        struct Node * newNode = (struct Node *)malloc(sizeof(struct Node));
        newNode->next = head;
        head = newNode;
        newNode->num = rand() % 1000;
        printf("add node, num : %d, tid : %ld\n", newNode->num, pthread_self());
        pthread_mutex_unlock(&mutex);
        sem_post(&csem);
    }

    return NULL;
}

void * customer(void * arg) {

    while(1) {
        sem_wait(&csem);
        pthread_mutex_lock(&mutex);
        // 保存头结点的指针
        struct Node * tmp = head;
        head = head->next;
        printf("del node, num : %d, tid : %ld\n", tmp->num, pthread_self());
        free(tmp);
        pthread_mutex_unlock(&mutex);
        sem_post(&psem);
       
    }
    return  NULL;
}

int main() {

    pthread_mutex_init(&mutex, NULL);
    sem_init(&psem, 0, 8);
    sem_init(&csem, 0, 0);

    // 创建5个生产者线程，和5个消费者线程
    pthread_t ptids[5], ctids[5];

    for(int i = 0; i < 5; i++) {
        pthread_create(&ptids[i], NULL, producer, NULL);
        pthread_create(&ctids[i], NULL, customer, NULL);
    }

    for(int i = 0; i < 5; i++) {
        pthread_detach(ptids[i]);
        pthread_detach(ctids[i]);
    }

    while(1) {
        sleep(10);
    }

    pthread_mutex_destroy(&mutex);

    pthread_exit(NULL);

    return 0;
}
```


# 第4章 Linux网络编程

##  网络结构模式

### C/S结构

- **简介**
  *客户机-服务器结构模式；客户机通过局域网与服务器相连，接受用户的请求，并通过网络向服务器提出请求，对数据库进行操作。服务器接受客户机的请求，将数据提交给客户机，客户机将数据进行计算并将结果呈现给用户。*

- **优点**
  
  *1. 能充分发挥客户端 PC 的处理能力，很多工作可以在客户端处理后再提交给服务器，所以 **C/S 结构客户端响应速度快**
  
  2. 操作界面漂亮、形式多样，可以充分满足客户自身的个性化要求；
  3. C/S 结构的管理信息系统具有较强的事务处理能力，能实现复杂的业务流程；
  4. **安全性较高**，C/S 一般面向相对固定的用户群，程序更加注重流程，它可以对权限进行多层次校验，提供了更安全的存取模式，对信息安全的控制能力很强，一般高度机密的信息系统采用 C/S 结构适宜*

- **缺点**
  *1. 客户端需要安装专用的客户端软件。首先涉及到安装的工作量，其次任何一台电脑出问题，如病毒、硬件损坏，都需要进行安装或维护。系统软件升级时，每一台客户机需要重新安装，其**维护和升级成本非常高***
  *2. **对客户端的操作系统一般也会有限制，不能够跨平台***

### B/S 结构

- **简介**
  *浏览器-服务器结构模式；这种模式统一了客户端，将系统功能实现的核心部分集中到服
  务器上，简化了系统的开发、维护和使用，服务器安装Oracle等数据库。浏览器通过Web Server 同数据
  库进行数据交互*

- **优点**
  
  *B/S 架构最大的优点是总体拥有成本低、维护方便、 分布性强、开发简单，可以不用安装任何专门的软
  件就能实现在任何地方进行操作，客户端零维护，系统的扩展非常容易*

- **缺点**
  *1. **通信开销大、系统和数据的安全性较难保障**;
  
  2. 个性特点明显降低，无法实现具有个性化的功能要求；
  3. 协议一般是固定的：http/https
  4. **客户端服务器端的交互是请求-响应模式，通常动态刷新页面，响应速度明显降低***

##  MAC地址、IP地址

### MAC地址

**网卡**
网卡又称为**网络适配器或网络接口卡NIC**，是一块被设计用来允许计算机在计算机网络上进行通讯的计算机硬件；
其拥有 MAC 地址，属于 OSI 模型的第2层（数据链路层），它使得用户可以通过电缆或无线相互连接。
**每一个网卡都有一个被称为 MAC 地址的独一无二的 48 位串行号**。
网卡的主要功能：1.数据的封装与解封装、2.链路管理、3.数据编码与译码

**MAC地址**
*MAC 地址（Media Access Control Address），也称为**局域网地址、以太网地址、物理地址或硬件地址**，
**它是一个用来确认网络设备位置的位址，由网络设备制造商生产时烧录在网卡中。用于在网络中唯一标识一个网卡**，
MAC 地址的长度为 48 位（6个字节），通常表示为 12 个 16 进制数，如：00-16-EA-AE-3C-40 就是一个MAC 地址，
其中前 3 个字节，16 进制数 00-16-EA 代表网络硬件制造商的编号，而后3个字节，16进制数 AE-3C-40 代表该制造商所制造的某个网络产品（如网卡）的系列号；
MAC 地址就如同身份证上的身份证号码，具有唯一性*

### IP地址

**IP协议**
IP协议是为计算机网络相互连接进行通信而设计的协议；
实际上是一套由软件程序组成的协议软件，它把各种不同“帧”统一转换成“IP 数据报”格式

**IP地址**
IP 地址（Internet Protocol Address）是指**互联网协议地址**，**它为互联网上的每一个网络和每一台主机分配一个逻辑地址**，以此来屏蔽物理地址的差异

**IP地址编址方式**
IP地址=网络号+主机号
| 类别  |  最大网络数   |        IP地址范围         | 单个网段最大主机数 |       私有IP地址范围        |
| :---: | :-----------: | :-----------------------: | :----------------: | :-------------------------: |
|   A   |  126(2^7-2)   |  1.0.0.1-126.255.255.254  |      16777214      |   10.0.0.0-10.255.255.255   |
|   B   |  16384(2^14)  | 128.0.0.1-191.255.255.254 |       65534        |  172.16.0.0-172.31.255.255  |
|   C   | 2097152(2^21) | 192.0.0.1-223.255.255.254  |        254         | 192.168.0.0-192.168.255.255 |

**A类IP地址:** A 类 IP 地址就由 1 字节的网络地址和 3 字节主机地址组成，**网络地址的最高位必须是“0”**
A 类 IP 地址中网络的标识长度为 8 位，主机标识的长度为 24 位
A 类 IP 地址 地址范围 1.0.0.1 - 126.255.255.254 二进制表示为：00000001 00000000 00000000 00000001 - **01111111** 11111111 11111111 11111110
**网络数=2^7-1-1=126 (127.0.0.1为本机地址)**
最后一个是广播地址;A 类 IP 地址的子网掩码为 255.0.0.0，**每个网络支持的最大主机数为 256 的 3 次方 - 2 = 16777214 台**(去除广播地址和0地址)

**B类IP地址：** B 类 IP 地址就由 2 字节的网络地址和 2 字节主机地址组成，**网络地址的最高位必须是“10”**
B 类 IP地址中网络的标识长度为 16 位，主机标识的长度为 16 位
B 类 IP 地址地址范围 128.0.0.1 - 191.255.255.254 二进制表示为：10000000 00000000 00000000 00000001 -** 10111111** 11111111 11111111 11111110
**网络数=2^6 * 2^8 = 2^14**
最后一个是广播地址，B 类 IP 地址的子网掩码为 255.255.0.0，每个网络支持的最大主机数为 256 的 2 次方 - 2 = 65534 台

**C类IP地址**C 类 IP 地址就由 3 字节的网络地址和 1 字节主机地址组成，**网络地址的最高位必须是“110”**
C 类 IP 地址中网络的标识长度为 24 位，主机标识的长度为 8 位
C 类 IP 地址范围 192.0.0.1-223.255.255.254 二进制表示为: 11000000 00000000 0000000000000001 - **11011111** 11111111 11111111 11111110
**网络数=2^5 * 2^16 = 2^21**
C类IP地址的子网掩码为 255.255.255.0，每个网络支持的最大主机数为 256 - 2 = 254 台

**特殊地址**
*- 每一个字节都为 0 的地址（ “0.0.0.0” ）对应于当前主机*
*- IP 地址中的每一个字节都为 1 的 IP 地址（ “255.255.255.255” ）是当前子网的广播地址*
*- IP地址中不能以十进制 “127” 作为开头，该类地址中数字 127.0.0.1 到 127.255.255.255 用于回路测试，如：127.0.0.1可以代表本机IP地址*

### 子网掩码

子网掩码（subnet mask）结合 IP 地址一起使用，将某个 IP 地址划分成网络地址和主机地址两部分
子网掩码是一个 32 位的 2 进制数，其对应网络地址的所有位都置为 1，对应于主机地址的所有位置都为 0
求网络地址：子网掩码和ip按位与
求主机id：子网掩码取反，和ip按位与（或者就是网络地址0的位数）

##  端口

### 简介

可以认为是设备与外界通讯交流的出口，标识计算机中进程的唯一编号
端口可分为虚拟端口和物理端口，其中**虚拟端口指计算机内部或交换机路由器内的端口，不可见**，是特指TCP/IP协议中的端口，是逻辑意义上的端口。例如计算机中的 80 端口；
**物理端口又称为接口，是可见端口**，计算机背板的 RJ45 网口，交换机路由器集线器等 RJ45 端口
一个 IP地址的端口可以有 65536（即：2^16）个，范围是从 0 到65535（2^16-1）

##  网络模型

### OSI 七层参考模型

![OSI 七层参考模型](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245009.png)

1. 物理层：主要定义物理设备标准，如网线的接口类型、光纤的接口类型;
   它的主要作用是传输比特流（数模转换与模数转换）。这一层的数据叫做比特。

2. 数据链路层：**建立逻辑连接、进行硬件地址寻址、差错校验等功能**。
   定义了如何让格式化数据以帧为单位进行传输，以及如何让控制对物理介质的访问。将比特组合成字节进而组合成帧，用MAC地
   址访问介质。

3. 网络层：**进行逻辑地址寻址**

4. 传输层：定义了一些传输数据的协议和端口号（ WWW 端口 80 等），
   如：TCP（传输控制协议，传输效率低，可靠性强，用于传输可靠性要求高，数据量大的数据），UDP（用户数据报协议，与TCP 特性恰恰相反，用于传输可靠性要求不高，数据量小的数据，如 QQ 聊天数据就是通过这种方式传输的）。 主要是将从下层接收的数据进行分段和传输，到达目的地址后再进行重组。常常把这一层数据叫做段

5. 会话层：**通过传输层（端口号：传输端口与接收端口）建立数据传输的通路**。主要在你的系统之间
   发起会话或者接受会话请求。

6. 表示层：数据的表示、安全、压缩。**主要是进行对接收的数据进行解释、加密与解密、压缩与解压
   缩**等（也就是把计算机能够识别的东西转换成人能够能识别的东西（如图片、声音等）

7. 应用层：网络服务与最终用户的一个接口。这一层*为用户的应用程序（例如电子邮件、文件传输和
   终端仿真）提供网络服务*

**物数网传会表应**

### TCP/IP 四层模型

**简介**
TCP/IP协议族是一个四层协议系统，自底而上分别是数据链路层(网络接口层)、网络层、传输层和应用层。
每一层完成不同的功能，且通过若干协议来实现，上层协议使用下层协议提供的服务。
![TCP/IP协议族体系结构](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245010.png)

#### 协议

协议，网络协议的简称，网络协议是通信计算机双方必须共同遵从的一组约定

**常见协议**

1. 应用层常见的协议有：FTP协议（File Transfer Protocol 文件传输协议）、HTTP协议（Hyper TextTransfer Protocol 超文本传输协议）、NFS（Network File System 网络文件系统）
2. 传输层常见协议有：TCP协议（Transmission Control Protocol 传输控制协议）、UDP协议（UserDatagram Protocol 用户数据报协议）
3. 网络层常见协议有：IP 协议（Internet Protocol 因特网互联协议）、ICMP 协议（Internet ControlMessage Protocol 因特网控制报文协议）、IGMP 协议（Internet Group Management Protocol 因特网组管理协议）
4. 网络接口层常见协议有：ARP协议（Address Resolution Protocol 地址解析协议）、RARP协议（Reverse Address Resolution Protocol 反向地址解析协议)

**UDP协议** 

![UDP协议](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245011.png)  

**TCP协议**  

![TCP协议](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245012.png)

1. 源端口号：发送方端口号
2. 目的端口号：接收方端口号
3. 序列号：本报文段的数据的第一个字节的序号
4. 确认序号：期望收到对方下一个报文段的第一个数据字节的序号
5. 首部长度（数据偏移）：TCP 报文段的数据起始处距离 TCP 报文段的起始处有多远，即首部长度。单位：32位，即以 4 字节为计算单位
6. 保留：占 6 位，保留为今后使用，目前应置为 0
7. 紧急 URG ：此位置 1 ，表明紧急指针字段有效，它告诉系统此报文段中有紧急数据，应尽快传送
8. 确认 ACK：仅当 ACK=1 时确认号字段才有效，TCP 规定，在连接建立后所有传达的报文段都必须把 ACK 置1
9. 推送 PSH：当两个应用进程进行交互式的通信时，有时在一端的应用进程希望在键入一个命令后立即就能够收到对方的响应。在这种情况下，TCP 就可以使用推送（push）操作，这时，发送方TCP 把 PSH 置 1，并立即创建一个报文段发送出去，接收方收到 PSH = 1 的报文段，就尽快地（即“推送”向前）交付给接收应用进程，而不再等到整个缓存都填满后再向上交付
10. 复位 RST：用于复位相应的 TCP 连接
11. 同步 SYN：仅在三次握手建立 TCP 连接时有效。当 SYN = 1 而 ACK = 0 时，表明这是一个连接请求报文段，对方若同意建立连接，则应在相应的报文段中使用 SYN = 1 和 ACK = 1。因此，SYN 置1 就表示这是一个连接请求或连接接受报文12. 终止 FIN：用来释放一个连接。当 FIN = 1 时，表明此报文段的发送方的数据已经发送完毕，并要求释放运输连接
12. 窗口：指发送本报文段的一方的接收窗口（而不是自己的发送窗口）
13. 校验和：校验和字段检验的范围包括首部和数据两部分，在计算校验和时需要加上 12 字节的伪头部
14. 紧急指针：仅在 URG = 1 时才有意义，它指出本报文段中的紧急数据的字节数（紧急数据结束后就是普通数据），即指出了紧急数据的末尾在报文中的位置，注意：即使窗口为零时也可发送紧急数据
15. 选项：长度可变，最长可达 40 字节，当没有使用选项时，TCP 首部长度是 20 字节

**IP协议**
![IP协议](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245013.png)

1. 版本：IP 协议的版本。通信双方使用过的 IP 协议的版本必须一致，目前最广泛使用的 IP 协议版本号为 4（即IPv4)
2. 首部长度：单位是 32 位（4 字节）
3. 服务类型：一般不适用，取值为 0
4. 总长度：指首部加上数据的总长度，单位为字节
5. 标识（identification）：IP 软件在存储器中维持一个计数器，每产生一个数据报，计数器就加 1，并将此值赋给标识字段
6. 标志（flag）：目前只有两位有意义。标志字段中的最低位记为 MF。MF = 1 即表示后面“还有分片”的数据报。MF = 0 表示这已是若干数据报片中的最后一个。标志字段中间的一位记为 DF，意思是“不能分片”，只有当 DF = 0 时才允许分片
7. 片偏移：指出较长的分组在分片后，某片在源分组中的相对位置，也就是说，相对于用户数据段的起点，该片从何处开始。片偏移以 8 字节为偏移单位。
8. 生存时间：TTL，表明是数据报在网络中的寿命，即为“跳数限制”，由发出数据报的源点设置这个字段。路由器在转发数据之前就把 TTL 值减一，当 TTL 值减为零时，就丢弃这个数据报。
9. 协议：指出此数据报携带的数据时使用何种协议，以便使目的主机的 IP 层知道应将数据部分上交给哪个处理过程，常用的 ICMP(1)，IGMP(2)，TCP(6)，UDP(17)，IPv6（41）
10. 首部校验和：只校验数据报的首部，不包括数据部分。
11. 源地址：发送方 IP 地址
12. 目的地址：接收方 IP 地址

**以太网帧协议**

![以太网帧协议](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245014.png)

**ARP协议**
![ARP协议](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245015.png)

#### 封装

**上层协议是通过封装（encapsulation）使用下层协议提供的服务的应**
每层协议都将在上层数据的基础上加上自己的头部信息（有时还包括尾部信息），以实现该层的功能，这个过程就称为封装
![封装](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245016.png)

![arp请求封装](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245017.png)

#### 分用

当帧到达目的主机时，将沿着协议栈自底向上依次传递。**各层协议依次处理帧中本层负责的头部数据，
以获取所需的信息，并最终将处理后的帧交给目标应用程序**。这个过程称为分用（demultiplexing）
分用是依靠头部信息中的类型字段实现的

![分用](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245018.png)

#### 网络通信过程

![网络通信过程](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245019.png)

## socket通信基础

### socket 介绍

> 所谓 socket（套接字），就是对**网络中不同主机上的应用进程之间进行双向通信的端点的抽象**
> 
> 从所处的地位来讲，套接字上联应用进程，下联网络协议栈，是应用程序通过网络协议进行通信的接口，是应用程序与网络协议根进行交互的接口
> 
> 通信时其中一个网络应用程序将要传输的一段信息写入它所在主机的socket 中，该 socket 通过与网络接口卡（NIC）相连的传输介质将这段信息送到另外一台主机的 socket 中，使对方能够接收到这段信息。**socket 是由 IP 地址和端口结合的，提供向应用层进程传送数据包的机制**
> 
> socket 本身有“插座”的意思，在 Linux 环境下，用于表示进程间网络通信的特殊文件类型。本质为内核借助缓冲区形成的伪文件。既然是文件，那么理所当然的，我们可以使用文件描述符引用套接字。与管道类似的，Linux 系统将其封装成文件的目的是为了统一接口，使得读写套接字和读写文件的操作一致。**区别是管道主要应用于本地进程间通信，而套接字多应用于网络进程间数据的传递**

![socket介绍](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245020.png)

```c++
// 套接字通信分两部分：
- 服务器端：被动接受连接，一般不会主动发起连接
- 客户端：主动向服务器发起连接
socket是一套通信的接口，Linux 和 Windows 都有，但是有一些细微的差别。
```

### 字节序

**简介**

> 现代 CPU 的累加器一次都能装载（至少）**4 字节**（这里考虑 32 位机），即一个整数。**那么这4字节在内存中排列的顺序将影响它被累加器装载成的整数的值**，这就是字节序问题。在各种计算机体系结构中，对于字节、字等的存储机制有所不同，因而引发了计算机通信领域中一个很重要的问题，即通信双方交流的信息单元（比特、字节、字、双字等等）应该以什么样的顺序进行传送。如
> 
> 套接字通信分两部分：
> 
> - 服务器端：被动接受连接，一般不会主动发起连接
> - 客户端：主动向服务器发起连接
> 
> socket是一套通信的接口，Linux 和 Windows 都有，但是有一些细微的差别。果不达成一致的规则，通信双方将无法进行正确的编码/译码从而导致通信失败
> 
> 字节序，顾名思义字节的顺序，**就是大于一个字节类型的数据在内存中的存放顺序**(一个字节的数据当然就无需谈顺序的问题了)。字节序分为大端字节序（Big-Endian） 和小端字节序（Little-Endian）
> 
> **大端字节序**是指一个整数的最高位字节（23 ~ 31 bit）存储在内存的低地址处，低位字节（0 ~ 7 bit）存储在内存的高地址处；
> **小端字节序**则是指整数的高位字节存储在内存的高地址处，而低位字节则存储在内存的低地址处。

**字节序举例**

- 小端字节序
  0x 01 02 03 04 - ff = 255 (一个字节刚好存放两个16进制数据->4+4)
  内存的方向 ----->
  内存的低位 -----> 内存的高位
  04 03 02 01
  0x 11 22 33 44 12 34 56 78

![小端字节序举例](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245021.png)

- 大端字节序
  0x 01 02 03 04
  内存的方向 ----->
  内存的低位 -----> 内存的高位
  01 02 03 04
  0x 12 34 56 78 11 22 33 44

![大端字节序举例](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245022.png)

```c++
/*  
    字节序：字节在内存中存储的顺序。
    小端字节序：数据的高位字节存储在内存的高位地址，低位字节存储在内存的低位地址
    大端字节序：数据的低位字节存储在内存的高位地址，高位字节存储在内存的低位地址
*/

// 通过代码检测当前主机的字节序
#include <stdio.h>

int main() {

    union {
        short value;    // 2字节
        char bytes[sizeof(short)];  // char[2]
    } test;

    test.value = 0x0102;
    if((test.bytes[0] == 1) && (test.bytes[1] == 2)) {
        printf("大端字节序\n");

    } else if((test.bytes[0] == 2) && (test.bytes[1] == 1)) {
        printf("小端字节序\n");

    } else {
        printf("未知\n");
    }

    return 0;
}

```


在这个程序中，char bytes[sizeof(short)]定义了一个char类型的数组，数组的大小为short类型的字节大小，也就是2个字节。由于数组本身的定义方式就占用了2个字节的存储空间，而该数组中的元素并没有被初始化赋值，所以数组的元素值也没有明确的初值。

在程序中，test变量是一个联合类型，它实际上占用的内存空间是以short类型为准，也就是2个字节的大小。由于联合所占用的内存空间是所有成员变量占用内存空间中最大的一个，所以char类型的bytes数组就是与short类型的value变量共享2个字节的内存空间，这也就意味着，bytes的每一个元素指向了value的内存地址中的每一个字节，即bytes[0]指向value的低字节，bytes[1]指向value的高字节。

在程序中通过给test.value赋值0x0102，其实就是赋的16进制值102，然后计算机会将这个16进制值转换为二进制数，并按照字节顺序存储。在大端字节序中，高位字节存储在低地址，低位字节存储在高地址，所以0x0102会被存储为0x01和0x02两个字节，也就是说test.bytes[0]等于1，test.bytes[1]等于2。而在小端字节序中，低位字节存储在低地址，高位字节存储在高地址，所以0x0102会被存储为0x02和0x01两个字节，也就是说test.bytes[0]等于2，test.bytes[1]等于1。这样就可以根据test.bytes数组的值来判断计算机当前使用的字节顺序是大端（big-endian）还是小端（little-endian）了。


**字节序转换函数**

> 当格式化的数据在两台使用不同字节序的主机之间直接传递时，接收端必然错误的解释之。
> 解决题的方法是：发送端总是把要发送的数据转换成大端字节序数据后再发送
> 而接收端知道对方传送过来的数据总是采用大端字节序，所以接收端可以根据自身采用的字节序决定是否对接收到的数据进行转换（小端机转换，大端机不转换）
> 
> **网络字节顺序**是 TCP/IP 中规定好的一种数据表示格式，它与具体的 CPU 类型、操作系统等无关，从而可以保证数据在不同主机之间传输时能够被正确解释，**网络字节顺序采用大端排序方式**
> BSD Socket提供了封装好的转换接口，方便程序员使用。包括从主机字节序到网络字节序的转换函数：
> htons、htonl；从网络字节序到主机字节序的转换函数：ntohs、ntohl

```c++
h - host 主机，主机字节序
to - 转换成什么
n - network 网络字节序
s - short unsigned short
l - long unsigned int
```

```c++
/*

    网络通信时，需要将主机字节序转换成网络字节序（大端），
    另外一段获取到数据以后根据情况将网络字节序转换成主机字节序。

    // 转换端口
    uint16_t htons(uint16_t hostshort);		// 主机字节序 - 网络字节序
    uint16_t ntohs(uint16_t netshort);		// 主机字节序 - 网络字节序

    // 转IP
    uint32_t htonl(uint32_t hostlong);		// 主机字节序 - 网络字节序
    uint32_t ntohl(uint32_t netlong);		// 主机字节序 - 网络字节序

    s - short unsigned short
    l - long unsigned int

*/

#include <stdio.h>
#include <arpa/inet.h>

int main() {

    // htons 转换端口
    unsigned short a = 0x0102;
    printf("a : %x\n", a);
    unsigned short b = htons(a);
    printf("b : %x\n", b);

    printf("=======================\n");

    // htonl  转换IP
    // (int *)buf 将首地址转化为 指针
    char buf[4] = {192, 168, 1, 100};
    int num = *(int *)buf;
    int sum = htonl(num);
    printf("sum=%d\n",sum);
    
    unsigned char *p = (char *)&sum;

    printf("%d %d %d %d\n", *p, *(p+1), *(p+2), *(p+3));

    printf("=======================\n");

    // ntohl
    unsigned char buf1[4] = {1, 1, 168, 192};
    int num1 = *(int *)buf1;
    int sum1 = ntohl(num1);
    unsigned char *p1 = (unsigned char *)&sum1;
    printf("%d %d %d %d\n", *p1, *(p1+1), *(p1+2), *(p1+3));
    
     // ntohs
    return 0;
}
/*
    panan@ecs-kc1-large-2-linux-20220314145535:~/Linux/lesson31$ ./bytetrans 
    a : 102
    b : 201
    =======================
    sum=-1062731420
    100 1 168 192
    =======================
    192 168 1 1
*/
```

#### socket 地址

通用 socket 地址

socket 网络编程接口中表示 socket 地址的是结构体 sockaddr，其定义如下：

```c++
#include <bits/socket.h>
struct sockaddr {
  sa_family_t sa_family;
  char sa_data[14];
};
typedef unsigned short int sa_family_t;
```

sa_family 成员是地址族类型（sa_family_t）的变量。地址族类型通常与协议族类型对应。常见的协议族（protocol family，也称 domain）和对应的地址族入下所示：
|  协议族  |  地址族  | 描述             |
| :------: | :------: | :--------------- |
| PF_UNIX  | AF_UNIX  | UNIX本地域协议族 |
| PF_INET  | AF_INET  | TCP/IPv4协议族   |
| PF_INET6 | AF_INET6 | TCP/IPv6协议族   |

**宏 PF_\* 和 AF_\*** 都定义在 bits/socket.h 头文件中，且后者与前者有完全相同的值，所以二者通常混用  

sa_data 成员用于存放 socket 地址值。但是，不同的协议族的地址值具有不同的含义和长度，如下所示：

|  协议族  | 地址值含义和长度                                                            |
| :------: | :-------------------------------------------------------------------------- |
| PF_UNIX  | 文件的路径名，长度可达到108字节                                             |
| PF_INET  | 16 bit 端口号和 32 bit IPv4 地址，共 6 字节                                 |
| PF_INET6 | 16 bit 端口号，32 bit 流标识，128 bit IPv6 地址，32 bit 范围 ID，共 26 字节 |

由上表可知，14 字节的 sa_data 根本无法容纳多数协议族的地址值。因此，Linux 定义了下面这个新的通用的 socket 地址结构体，这个结构体不仅提供了足够大的空间用于存放地址值，而且是内存对齐的

```c++
#include <bits/socket.h>
struct sockaddr_storage
{
  sa_family_t sa_family;
  unsigned long int __ss_align;
  char __ss_padding[ 128 - sizeof(__ss_align) ];
};
typedef unsigned short int sa_family_t;
```

**专用 socke**

![专用socket地址](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245023.png)

UNIX 本地域协议族使用如下专用的 socket 地址结构体：

```c++
#include <sys/un.h>
struct sockaddr_un
{
  sa_family_t sin_family;
  char sun_path[108];
};
```

TCP/IP 协议族有 sockaddr_in 和 sockaddr_in6 两个专用的 socket 地址结构体，它们分别用于 IPv4 和IPv6：

```c++
#include <netinet/in.h>
struct sockaddr_in
{
    sa_family_t sin_family; /* __SOCKADDR_COMMON(sin_) */
    in_port_t sin_port; /* Port number. */
    struct in_addr sin_addr; /* Internet address. */
    /* Pad to size of `struct sockaddr'. */
    unsigned char sin_zero[sizeof (struct sockaddr) - __SOCKADDR_COMMON_SIZE -
    sizeof (in_port_t) - sizeof (struct in_addr)];
};
struct in_addr
{
    in_addr_t s_addr;
};
struct sockaddr_in6
{
    sa_family_t sin6_family;
    in_port_t sin6_port; /* Transport layer port # */
    uint32_t sin6_flowinfo; /* IPv6 flow information */
    struct in6_addr sin6_addr; /* IPv6 address */
    uint32_t sin6_scope_id; /* IPv6 scope-id */
};
typedef unsigned short uint16_t;
typedef unsigned int uint32_t;
typedef uint16_t in_port_t;
typedef uint32_t in_addr_t;
#define __SOCKADDR_COMMON_SIZE (sizeof (unsigned short int))
```

所有专用 socket 地址（以及 sockaddr_storage）类型的变量在实际使用时都需要转化为通用 socket 地址类型 sockaddr（强制转化即可），因为所有 socket 编程接口使用的地址参数类型都是 sockaddr。

**IP地址转换（字符串ip-整数 ，主机、网络字节序的转换）**

> 通常，人们习惯用可读性好的字符串来表示 IP 地址，比如用点分十进制字符串表示 IPv4 地址，以及用十六进制字符串表示 IPv6 地址。但编程中我们需要先把它们转化为整数（二进制数）方能使用。而记录日志时则相反，我们要把整数表示的 IP 地址转化为可读的字符串

```c++
#include <arpa/inet.h>
// p:点分十进制的IP字符串，n:表示network，网络字节序的整数
int inet_pton(int af, const char *src, void *dst);
    af:地址族： AF_INET  AF_INET6
    src:需要转换的点分十进制的IP字符串
    dst:转换后的结果保存在这个里面

// 将网络字节序的整数，转换成点分十进制的IP地址字符串
const char *inet_ntop(int af, const void *src, char *dst, socklen_t size);
    af:地址族： AF_INET  AF_INET6
    src: 要转换的ip的整数的地址
    dst: 转换成IP地址字符串保存的地方
    size：第三个参数的大小（数组的大小）
    返回值：返回转换后的数据的地址（字符串），和 dst 是一样的
```

```c++
#include <stdio.h>
#include <arpa/inet.h>

int main() {

    // 创建一个ip字符串,点分十进制的IP地址字符串
    char buf[] = "192.168.1.4";
    unsigned int num = 0;

    // 将点分十进制的IP字符串转换成网络字节序的整数
    inet_pton(AF_INET, buf, &num);
    unsigned char * p = (unsigned char *)&num;
    printf("%d %d %d %d\n", *p, *(p+1), *(p+2), *(p+3));


    // 将网络字节序的IP整数转换成点分十进制的IP字符串
    char ip[16] = "";
    const char * str =  inet_ntop(AF_INET, &num, ip, 16);
    printf("str : %s\n", str);
    printf("ip : %s\n", str);
    printf("%d\n", ip == str);

    return 0;
}
```

#### TCP通信流程

**TCP与UDP**

```c++
// TCP 和 UDP -> 传输层的协议
UDP:用户数据报协议，面向无连接，可以单播，多播，广播， 面向数据报，不可靠

TCP:传输控制协议，面向连接的，可靠的，基于字节流，仅支持单播传输

               UDP                                TCP
是否创建连接    无连接                              面向连接
是否可靠        不可靠                             可靠的
连接的对象个数  一对一、一对多、多对一、多对多        支持一对一
传输的方式      面向数据报                          面向字节流
首部开销        8个字节                            最少20个字节
适用场景      实时应用（视频会议，直播）            可靠性高的应用（文件传输）
```

![TCP通信流程](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245024.png)

**服务器端 （被动接受连接的角色）**

> 1. 创建一个用于监听的套接字
>    - 监听：监听有客户端的连接
>    - 套接字：这个套接字其实就是一个文件描述符
> 2. 将这个监听文件描述符和本地的IP和端口绑定（IP和端口就是服务器的地址信息）
>    - 客户端连接服务器的时候使用的就是这个IP和端口
> 3. 设置监听，监听的fd开始工作
> 4. 阻塞等待，当有客户端发起连接，解除阻塞，接受客户端的连接，会得到一个和客户端通信的套接字（fd）*一个新的fd*
> 5. 通信
>    - 接收数据
>    - 发送数据
> 6. 通信结束，断开连接

**客户端**

> 1. 创建一个用于通信的套接字（fd）
> 2. 连接服务器，需要指定连接的服务器的 IP 和 端口
> 3. 连接成功了，客户端可以直接和服务器通信
>    - 接收数据
>    - 发送数据
> 4. 通信结束，断开连接

#### 套接字函数

```c++
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h> // 包含了这个头文件，上面两个就可以省略
int socket(int domain, int type, int protocol);
    - 功能：创建一个套接字
    - 参数：
        - domain: 协议族
        AF_INET : ipv4
        AF_INET6 : ipv6
        AF_UNIX, AF_LOCAL : 本地套接字通信（进程间通信）
    - type: 通信过程中使用的协议类型
        SOCK_STREAM : 流式协议
        SOCK_DGRAM : 报式协议
    - protocol : 具体的一个协议。一般写0
        - SOCK_STREAM : 流式协议默认使用 TCP
        - SOCK_DGRAM : 报式协议默认使用 UDP
    - 返回值：
        - 成功：返回文件描述符，操作的就是内核缓冲区。
        - 失败：-1
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen); // socket命名
    - 功能：绑定，将fd 和本地的IP + 端口进行绑定
    - 参数：
        - sockfd : 通过socket函数得到的文件描述符
        - addr : 需要绑定的socket地址，这个地址封装了ip和端口号的信息
        - addrlen : 第二个参数结构体占的内存大小
int listen(int sockfd, int backlog); // /proc/sys/net/core/somaxconn
    - 功能：监听这个socket上的连接
    - 参数：
        - sockfd : 通过socket()函数得到的文件描述符
        - backlog : 未连接的和已经连接的和的最大值， 5
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
    - 功能：接收客户端连接，默认是一个阻塞的函数，阻塞等待客户端连接
    - 参数：
        - sockfd : 用于监听的文件描述符
        - addr : 传出参数，记录了连接成功后客户端的地址信息（ip，port）
        - addrlen : 指定第二个参数的对应的内存大小
    - 返回值：
        - 成功 ：用于通信的文件描述符
        - -1 ： 失败
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
      - 功能： 客户端连接服务器
      - 参数：
          - sockfd : 用于通信的文件描述符
          - addr : 客户端要连接的服务器的地址信息
          - addrlen : 第二个参数的内存大小
      - 返回值：成功 0， 失败 -1

ssize_t write(int fd, const void *buf, size_t count); // 写数据
ssize_t read(int fd, void *buf, size_t count); // 读数据
```

#### tcp套接字通信的实现

**server.c**

```c++
// TCP 通信的服务器端

#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>
#include <stdlib.h>

int main() {

    // 1.创建socket(用于监听的套接字)
    int lfd = socket(AF_INET, SOCK_STREAM, 0);

    if(lfd == -1) {
        perror("socket");
        exit(-1);
    }

    // 2.绑定
    // TCP/IP 协议族有 sockaddr_in 和 sockaddr_in6 两个专用的 socket 地址结构体
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;

    // inet_pton(AF_INET, "192.168.193.128", saddr.sin_addr.s_addr);
    // 计算机有多个网卡，对应可能有多个IP 0 表示任意一个
    saddr.sin_addr.s_addr = INADDR_ANY;  // 0.0.0.0
    saddr.sin_port = htons(9999);
    /*
        为了向前兼容，现在sockaddr 退化成了（void *）的作用，传递一个地址给函数，
        至于这个函数是 sockaddr_in 还是sockaddr_in6，由地址族确定，
        然后函数内部再强制类型转化为所需的地址类型。
    */
    int ret = bind(lfd, (struct sockaddr *)&saddr, sizeof(saddr));

    if(ret == -1) {
        perror("bind");
        exit(-1);
    }

    // 3.监听
    ret = listen(lfd, 8);
    if(ret == -1) {
        perror("listen");
        exit(-1);
    }

    // 4.接收客户端连接
    struct sockaddr_in clientaddr;
    int len = sizeof(clientaddr);
    int cfd = accept(lfd, (struct sockaddr *)&clientaddr, &len);

    if(cfd == -1) {
        perror("accept");
        exit(-1);
    }

    // 输出客户端的信息
    char clientIP[16];
    // 将网络字节序的整数，转换成点分十进制的IP地址字符串
    inet_ntop(AF_INET, &clientaddr.sin_addr.s_addr, clientIP, sizeof(clientIP));
    // 将网络字节序-> 主机字节序
    unsigned short clientPort = ntohs(clientaddr.sin_port);
    printf("client ip is %s, port is %d\n", clientIP, clientPort);

    // 5.通信
    char recvBuf[1024] = {0};
    while(1) {

        // 获取客户端的数据 从通信的文件描述符 
        // 若客户端没有数据发送则阻塞
        int num = read(cfd, recvBuf, sizeof(recvBuf));
        if(num == -1) {
            perror("read");
            exit(-1);
        } else if(num > 0) {
            printf("recv client data : %s\n", recvBuf);
        } else if(num == 0) {
            // 表示客户端断开连接
            printf("clinet closed...");
            break;
        }

        char * data = "hello,i am server";
        // 给客户端发送数据
        //write(cfd, data, strlen(data));
        // 给客户端发送接收到的数据  回射服务器
        write(cfd, recvBuf, strlen(recvBuf));
    }

    // 关闭文件描述符
    close(cfd);
    close(lfd);

    return 0;
}
```

**client.c**

```c++
// TCP通信的客户端

#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>
#include <stdlib.h>

int main() {

    // 1.创建套接字
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    if(fd == -1) {
        perror("socket");
        exit(-1);
    }

    // 2.连接服务器端  connect()
    struct sockaddr_in serveraddr;
    serveraddr.sin_family = AF_INET;
    //  p:点分十进制的IP字符串，n:表示network，网络字节序的整数
    //  将IP字符串转化为网络字节序的整数
    //  192.168.0.171 为本机ip地址
    inet_pton(AF_INET, "192.168.0.171", &serveraddr.sin_addr.s_addr);

    // htons() 主机字节序->网络字节序  设置端口
    serveraddr.sin_port = htons(9999);
    int ret = connect(fd, (struct sockaddr *)&serveraddr, sizeof(serveraddr));

    if(ret == -1) {
        perror("connect");
        exit(-1);
    }


    // 3. 通信
    char recvBuf[1024] = {0};
    while(1) {

        //char * data = "hello,i am client";
        // fgets()  reads in at most one less than size characters from stream and stores them into the buffer pointed to by s.  
        // Reading stops after an EOF or a newline.  
        // If a newline is read, it is stored into the buffer.  A terminating null byte ('\0') is stored after the last character in the buffer.

        // char  data[100] ;
        // fgets(data,100,stdin);

        char * data="i am client!";

        // 给客户端发送数据
        write(fd, data , strlen(data));

        sleep(1);

        int len = read(fd, recvBuf, sizeof(recvBuf));
        if(len == -1) {
            perror("read");
            exit(-1);
        } else if(len > 0) {
            printf("recv server data : %s\n", recvBuf);
        } else if(len == 0) {
            // 表示服务器端断开连接
            printf("server closed...");
            break;
        }

    }

    // 关闭连接
    close(fd);

    return 0;
}
```

#### TCP 三次握手

> TCP 提供了一种可靠、面向连接、字节流、传输层的服务，采用三次握手建立一个连接。采用四次挥手来关闭一个连接
> 三次握手的目的是保证双方互相之间建立了连接。
> 三次握手发生在客户端连接的时候，当调用connect()，底层会通过TCP协议进行三次握手。

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245025.png)

- 16 位端口号（port number）：告知主机报文段是来自哪里（源端口）以及传给哪个上层协议或应用程序（目的端口）的。进行 TCP 通信时，客户端通常使用系统自动选择的临时端口号。

- 32 **位序列号seq**（sequence number）：一次 TCP 通信（从 TCP 连接建立到断开）过程中某一个传输方向上的字节流的每个字节的编号。
  > 假设主机 A 和主机 B 进行 TCP 通信，A 发送给 B 的第一个TCP 报文段中，序号值被系统初始化为某个随机值 ISN（Initial Sequence Number，初始序号值）。
  > 那么在该传输方向上（从 A 到 B），后续的 TCP 报文段中序号值将被系统设置成 ISN 加上该报文段所携带数据的第一个字节在整个字节流中的偏移。
  > 例如，某个 TCP 报文段传送的数据是字节流中的第 1025 ~ 2048 字节，那么该报文段的序号值就是 ISN + 1025。另外一个传输方向（从B 到 A）的 TCP 报文段的序号值也具有相同的含义。

- 32 位**确认号ack**（acknowledgement number）：用作对另一方发送来的 TCP 报文段的响应。
  
> 其值是收到的 TCP 报文段的序号值 + 标志位长度（SYN，FIN） + 数据长度 。假设主机 A 和主机 B 进行TCP 通信，那么 A 发送出的 TCP 报文段不仅携带自己的序号，而且包含对 B 发送来的 TCP 报文段的确认号。反之，B 发送出的 TCP 报文段也同样携带自己的序号和对 A 发送来的报文段的确认序号

- 4 位头部长度（head length）：标识该 TCP 头部有多少个 32 bit(4 字节)。因为 4 位最大能表示15，所以 TCP 头部最长是60 字节。

- 6 位标志位包含如下几项：
  
  | 标志  |                                   含义                                    |
  | :---: | :-----------------------------------------------------------------------: |
  |  URG  |              紧急指针是否有效。为1，表示某一位需要被优先处理              |
  |  ACK  |                        确认号是否有效，一般置为1。                        |
  |  PSH  |               提示接收端应用程序立即从TCP缓冲区把数据读走。               |
  |  RST  |                       对方要求重新建立连接，复位。                        |
  |  SYN  | 请求建立连接，并在其序列号的字段进行序列号的初始值设定。建立连接，设置为1 |
  |  FIN  |                              希望断开连接。                               |

- 16 位窗口大小（window size）：是 TCP 流量控制的一个手段。这里说的窗口，指的是接收通告窗口（Receiver Window，RWND）。它告诉对方本端的 TCP 接收缓冲区还能容纳多少字节的数据，这样对方就可以控制发送数据的速度。

- 16 位校验和（TCP checksum）：由发送端填充，接收端对 TCP 报文段执行 CRC 算法以校验TCP 报文段在传输过程中是否损坏。注意，这个校验不仅包括 TCP 头部，也包括数据部分。这也是 TCP 可靠传输的一个重要保障。

- 16 位紧急指针（urgent pointer）：是一个正的偏移量。它和序号字段的值相加表示最后一个紧急数据的下一个字节的序号。因此，确切地说，这个字段是紧急指针相对当前序号的偏移，不妨称之为紧急偏移。TCP 的紧急指针是发送端向接收端发送紧急数据的方法。

![三次握手](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245026.png)


> 第一次握手：
    1.客户端将**SYN标志**位置为1  同步序列编号
    2.生成一个随机的32位的**序号seq**=J 
>
>第二次握手：
    1.服务器端接收客户端的连接： **ACK=1 表示确认号有效**
    2.服务器会回发一个确认序号： ack=客户端的序号 + 数据长度 + SYN/FIN(按一个字节算) **ack=J+1**
    3.服务器端会向客户端发起连接请求： **SYN=1**  请求建立连接
    4.服务器会生成一个随机序号：**seq = K**
>
>第三次握手：
    1.客户单应答服务器的连接请求：**ACK=1 表示确认号有效**
    2.客户端回复收到了服务器端的数据：ack=服务端的序号 + 数据长度 + SYN/FIN(按一个字节算) **ack=K+1**


![TCP三次握手形象演示](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245027.png)

#### TCP 滑动窗口

**简介**
> **滑动窗口协议是用来改善吞吐量的一种技术**，即容许发送方在接收任何应答之前传送附加的包。接收方告诉发送方在某一时刻能送多少包;解决道网络拥塞状况，同时发送数据，导致中间节点阻塞掉包等问题
> 
> **TCP 中采用滑动窗口来进行传输控制，滑动窗口的大小意味着接收方还有多大的缓冲区可以用于接收数据。发送方可以通过滑动窗口的大小来确定应该发送多少字节的数据**
> 
> 滑动窗口是 TCP 中实现诸如 ACK 确认、流量控制、拥塞控制的承载结构
> 

![滑动窗口](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245028.png)

#### 四次挥手

> 四次挥手发生在断开连接的时候，在程序中当调用了close()会使用TCP协议进行四次挥手。
> 
> 客户端和服务器端都可以主动发起断开连接，谁先调用close()谁就是发起
> 
> 因为在TCP连接的时候，采用三次握手建立的的连接是双向的，在断开的时候需要双向断开。

![四次挥手](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245029.png)

```c++
1. 客户端进程发出连接释放报文，并且停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u（等于前面已经传送过来的数据的最后一个字节的序号加1），
此时，客户端进入FIN-WAIT-1（终止等待1）状态。 TCP规定，FIN报文段即使不携带数据，也要消耗一个序号。

2. 服务器收到连接释放报文，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，
服务端就进入了CLOSE-WAIT（关闭等待）状态。
TCP服务器通知高层的应用进程，客户端向服务器的方向就释放了，这时候处于半关闭状态，即客户端已经没有数据要发送了，
但是服务器若发送数据，客户端依然要接受。这个状态还要持续一段时间，也就是整个CLOSE-WAIT状态持续的时间。

3. 客户端收到服务器的确认请求后，此时，客户端就进入FIN-WAIT-2（终止等待2）状态，
等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最后的数据）。

4. 服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，FIN=1，ack=u+1，
由于在半关闭状态，服务器很可能又发送了一些数据，假定此时的序列号为seq=w，
此时，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认

5. 客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，
此时，客户端就进入了TIME-WAIT（时间等待）状态。注意此时TCP连接还没有释放，
必须经过2∗∗MSL（最长报文段寿命）的时间后，当客户端撤销相应的TCB后，才进入CLOSED状态。

6. 服务器只要收到了客户端发出的确认，立即进入CLOSED状态。同样，撤销TCB后，就结束了这次的TCP连接。
可以看到，服务器结束TCP连接的时间要比客户端早一些

```
#### 多进程实现并发服务器

**server_process.c**

```c++
#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>
#include <wait.h>
#include <errno.h>

void recyleChild(int arg) {
    // 在同一时间内需要回收的进程不止一个
    while(1) {
        int ret = waitpid(-1, NULL, WNOHANG);
        if(ret == -1) {
            // 所有的子进程都回收了
            break;
        }else if(ret == 0) {
            // 还有子进程活着
            break;
        } else if(ret > 0){
            // 被回收了
            printf("子进程 %d 被回收了\n", ret);
        }
    }
}

int main() {
   /*
    #include <signal.h>
    int sigaction(int signum, const struct sigaction *act,
                            struct sigaction *oldact);

        - 功能：检查或者改变信号的处理。信号捕捉
        - 参数：
            - signum : 需要捕捉的信号的编号或者宏值（信号的名称）
            - act ：捕捉到信号之后的处理动作
            - oldact : 上一次对信号捕捉相关的设置，一般不使用，传递NULL
        - 返回值：
            成功 0
            失败 -1

     struct sigaction {
        // 函数指针，指向的函数就是信号捕捉到之后的处理函数
        void     (*sa_handler)(int);
        // 不常用
        void     (*sa_sigaction)(int, siginfo_t *, void *);
        // 临时阻塞信号集，在信号捕捉函数执行过程中，临时阻塞某些信号。
        sigset_t   sa_mask;
        // 使用哪一个信号处理对捕捉到的信号进行处理
        // 这个值可以是0，表示使用sa_handler,也可以是SA_SIGINFO表示使用sa_sigaction
        int        sa_flags;
        // 被废弃掉了
        void     (*sa_restorer)(void);
    };

    */
    struct sigaction act;
    act.sa_flags = 0;
    sigemptyset(&act.sa_mask);
    act.sa_handler = recyleChild;
    // 注册信号捕捉
    sigaction(SIGCHLD, &act, NULL);
    
    /*
        int socket(int domain, int type, int protocol);
        - 功能：创建一个套接字
        - 参数：
        - domain: 协议族
        AF_INET : ipv4
        AF_INET6 : ipv6
        AF_UNIX, AF_LOCAL : 本地套接字通信（进程间通信）
        - type: 通信过程中使用的协议类型
        SOCK_STREAM : 流式协议
        SOCK_DGRAM : 报式协议
        - protocol : 具体的一个协议。一般写0
        - SOCK_STREAM : 流式协议默认使用 TCP
        - SOCK_DGRAM : 报式协议默认使用 UDP
        - 返回值：
        - 成功：返回文件描述符，操作的就是内核缓冲区。
        - 失败：-1
    */
    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    if(lfd == -1){
        perror("socket");
        exit(-1);
    }
    /*
        struct sockaddr_in
        {
            sa_family_t sin_family; // __SOCKADDR_COMMON(sin_) 
            in_port_t sin_port; // Port number. 
            struct in_addr sin_addr; // Internet address. 
            // Pad to size of `struct sockaddr'. 
    
            unsigned char sin_zero[sizeof (struct sockaddr) - __SOCKADDR_COMMON_SIZE -
            sizeof (in_port_t) - sizeof (struct in_addr)];
        };
        struct in_addr
        {
            in_addr_t s_addr;
        };
    */
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(9999);
    saddr.sin_addr.s_addr = INADDR_ANY;

    // 绑定
    int ret = bind(lfd,(struct sockaddr *)&saddr, sizeof(saddr));
    if(ret == -1) {
        perror("bind");
        exit(-1);
    }

    // 监听

    /*
        int listen(int sockfd, int backlog); // /proc/sys/net/core/somaxconn
        - 功能：监听这个socket上的连接
        - 参数：
            - sockfd : 通过socket()函数得到的文件描述符
            - backlog : 未连接的和已经连接的和的最大值， 5
    */
    ret = listen(lfd, 128);
    if(ret == -1) {
        perror("listen");
        exit(-1);
    }

    // 不断循环等待客户端连接
    while(1) {

        struct sockaddr_in cliaddr;
        int len = sizeof(cliaddr);
        // 接受连接
        int cfd = accept(lfd, (struct sockaddr*)&cliaddr, &len);
        if(cfd == -1) {
            // EINTR:  The system call was interrupted 
            // by a signal that was caught 
            // before a valid connection arrived; see signal(7).
            if(errno == EINTR) {
                 // 某个子进程结束 连接中断 继续去等待下一个连接
                continue;
            }
            perror("accept");
            // 父进程结束
            exit(-1);
        }

        // 每一个连接进来，创建一个子进程跟客户端通信
        pid_t pid = fork();
        if(pid == 0) {
            // 子进程
            // 获取客户端的信息
            char cliIp[16];
            inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, cliIp, sizeof(cliIp));
            unsigned short cliPort = ntohs(cliaddr.sin_port);
            printf("client ip is : %s, prot is %d\n", cliIp, cliPort);

            // 接收客户端发来的数据
            char recvBuf[1024];
            while(1) {
                int len = read(cfd, &recvBuf, sizeof(recvBuf));

                if(len == -1) {
                    perror("read");
                    exit(-1);
                }else if(len > 0) {
                    printf("recv client : %s\n", recvBuf);
                } else if(len == 0) {
                    printf("client closed....\n");
                    break;
                }
                write(cfd, recvBuf, strlen(recvBuf) + 1);
            }
            close(cfd);
            exit(0);    // 退出当前子进程
        }

    }
    close(lfd);
    return 0;
}
```
 **client.c**

 ```c++
 // TCP通信的客户端
#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>
#include <stdlib.h>

int main() {

    // 1.创建套接字
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    if(fd == -1) {
        perror("socket");
        exit(-1);
    }

    // 2.连接服务器端
    // 需要更改为本机ip
    struct sockaddr_in serveraddr;
    serveraddr.sin_family = AF_INET;
    const char* ip="192.168.0.171";
    inet_pton(AF_INET, ip, &serveraddr.sin_addr.s_addr);
    serveraddr.sin_port = htons(9999);
    int ret = connect(fd, (struct sockaddr *)&serveraddr, sizeof(serveraddr));

    if(ret == -1) {
        perror("connect");
        exit(-1);
    }
    
    // 3. 通信
    char recvBuf[1024];
    int i = 0;
    while(1) {
        
        sprintf(recvBuf, "data : %d\n", i++);
        
        // 给服务器端发送数据
        // 数据写入写缓存区
        // 考虑字符串结束符 strlen(recvBuf)+1
        write(fd, recvBuf, strlen(recvBuf)+1);

        // 从读缓存区读取数据
        int len = read(fd, recvBuf, sizeof(recvBuf));
        if(len == -1) {
            perror("read");
            exit(-1);
        } else if(len > 0) {
            printf("recv server : %s\n", recvBuf);
        } else if(len == 0) {
            // 表示服务器端断开连接
            printf("server closed...");
            break;
        }

        sleep(1);
    }

    // 关闭连接
    close(fd);

    return 0;
}
 ```
#### 多线程实现并发服务器

**server_thread.c**

```c++
#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>

struct sockInfo {
    int fd; // 通信的文件描述符
    struct sockaddr_in addr;
    pthread_t tid;  // 线程号
};

// 相当于设置最大能同时连接客户端
struct sockInfo sockinfos[128];

void * working(void * arg) {
    // 子线程和客户端通信   cfd 客户端的信息 线程号
    // 获取客户端的信息
    struct sockInfo * pinfo = (struct sockInfo *)arg;

    char cliIp[16];
    inet_ntop(AF_INET, &pinfo->addr.sin_addr.s_addr, cliIp, sizeof(cliIp));
    unsigned short cliPort = ntohs(pinfo->addr.sin_port);
    printf("client ip is : %s, prot is %d\n", cliIp, cliPort);

    // 接收客户端发来的数据
    char recvBuf[1024];
    while(1) {
        int len = read(pinfo->fd, &recvBuf, sizeof(recvBuf));

        if(len == -1) {
            perror("read");
            exit(-1);
        }else if(len > 0) {
            printf("recv client : %s\n", recvBuf);
        } else if(len == 0) {
            printf("client closed....\n");
            break;
        }
        write(pinfo->fd, recvBuf, strlen(recvBuf) + 1);
    }
    close(pinfo->fd);
    return NULL;
}

int main() {

    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    if(lfd == -1){
        perror("socket");
        exit(-1);
    }

    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(9999);
    saddr.sin_addr.s_addr = INADDR_ANY;

    // 绑定
    int ret = bind(lfd,(struct sockaddr *)&saddr, sizeof(saddr));
    if(ret == -1) {
        perror("bind");
        exit(-1);
    }

    // 监听
    ret = listen(lfd, 128);
    if(ret == -1) {
        perror("listen");
        exit(-1);
    }

    // 初始化数据
    int max = sizeof(sockinfos) / sizeof(sockinfos[0]);
    for(int i = 0; i < max; i++) {
        bzero(&sockinfos[i], sizeof(sockinfos[i]));
        sockinfos[i].fd = -1;
        sockinfos[i].tid = -1;
    }

    // 循环等待客户端连接，一旦一个客户端连接进来，就创建一个子线程进行通信
    while(1) {

        struct sockaddr_in cliaddr;
        int len = sizeof(cliaddr);
        // 接受连接
        int cfd = accept(lfd, (struct sockaddr*)&cliaddr, &len);

        struct sockInfo * pinfo;
        for(int i = 0; i < max; i++) {
            // 从这个数组中找到一个可以用的sockInfo元素
            if(sockinfos[i].fd == -1) {
                pinfo = &sockinfos[i];
                break;
            }
            if(i == max - 1) {
                // 防止没有可用sockInfo元素时跳出循环
                sleep(1);
                i--;
            }
        }

        pinfo->fd = cfd;
        memcpy(&pinfo->addr, &cliaddr, len);

        // 创建子线程
        pthread_create(&pinfo->tid, NULL, working, pinfo);

        pthread_detach(pinfo->tid);
    }

    close(lfd);
    return 0;
}
```
#### TCP状态转换

![TCP状态转换](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245030.png)

![TCP状态转换2](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245031.png)

- 2MSL（Maximum Segment Lifetime）
  > 主动断开连接的一方, 最后进入一个 TIME_WAIT状态, 这个状态会持续: 2msl
  > msl: 官方建议: 2分钟, 实际是30s
  > 
  > 当 TCP 连接主动关闭方接收到被动关闭方发送的 FIN 和最终的 ACK 后
  > 连接的主动关闭方必须处TIME_WAIT 状态并持续 2MSL 时间。
  >
  > 这样就能够让 TCP 连接的主动关闭方在它发送的 ACK 丢失的情况下重新发送最终的 ACK。
  > 主动关闭方重新发送的最终 ACK 并不是因为被动关闭方重传了 ACK
  > 而是因为被动关闭方重传了它的 FIN。事实上，被动关闭方总是重传 FIN 直到它收到一个最终的 ACK。
  > 


- 半关闭
  
    > 当 TCP 链接中 A 向 B 发送 FIN 请求关闭，另一端 B 回应 ACK 之后（A 端进入 FIN_WAIT_2状态），并没有立即发送 FIN 给 A，A 方处于半连接状态（半开关），此时 A 可以接收 B 发送的数据，但是 A 已经不能再向 B 发送数据。

 ```c++
#include <sys/socket.h>
int shutdown(int sockfd, int how)
sockfd: 需要关闭的socket的描述符
    how: 允许为shutdown操作选择以下几种方式:
        SHUT_RD(0)： 关闭sockfd上的读功能，此选项将不允许sockfd进行读操作。
        该套接字不再接收数据，任何当前在套接字接受缓冲区的数据将被无声的丢弃掉。
        SHUT_WR(1): 关闭sockfd的写功能，此选项将不允许sockfd进行写操作。进程不能在对此套接字发
        出写操作。
        SHUT_RDWR(2):关闭sockfd的读写功能。相当于调用shutdown两次：首先是以SHUT_RD,然后以
        SHUT_WR。    
 ```

- 使用 close 中止一个连接，但它只是减少描述符的引用计数，并不直接关闭连接，只有当描述符的引用计数为 0 时才关闭连接

- shutdown 不考虑描述符的引用计数，直接关闭描述符。也可选择中止一个方向的连接，只中止读或只中止写
  
-  如果有多个进程共享一个套接字，close 每被调用一次，计减 1 ，直到计数为 0 时，也就是所用进程都调用了 close，套接字将被释放
-  在多进程中如果一个进程调用了 shutdown(sfd, SHUT_RDWR后，其它的进程将无法进行通信。但如果一个进程 close(sfd) 将不会影响到其它进


#### 端口复用

端口复用最常用的用途是:
  - 防止服务器重启时之前绑定的端口还未释放
  - 程序突然退出而系统没有释放端口

```c++
#include <sys/types.h>
#include <sys/socket.h>
// 设置套接字的属性（不仅仅能设置端口复用）
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
 参数：
     - sockfd : 要操作的文件描述符
     - level : 级别 - SOL_SOCKET (端口复用的级别)
     - optname : 选项的名称
       - SO_REUSEADDR
       - SO_REUSEPORT
     - optval : 端口复用的值（整形）
       - 1 : 可以复用
       - 0 : 不可以复用
     - optlen : optval参数的大小
端口复用，设置的时机是在服务器绑定端口之前。
setsockopt();
bind();
```
**查看网络相关信息**

netsat
参数:   

    -a 所有的socket
    -p 显示正在使用socket的程序的名称
    -n 直接使用IP地址，而不通过域名服务器

**案例见code lesson34**
```c++
panan@ecs-kc1-large-2-linux-20220314145535:~/Linux/lesson34$ netstat -anp | grep 9999
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 0.0.0.0:9999            0.0.0.0:*               LISTEN      10343/./server      
tcp        0      0 127.0.0.1:38082         127.0.0.1:9999          ESTABLISHED 10381/./client      
tcp        0      0 127.0.0.1:9999          127.0.0.1:38082         ESTABLISHED 10343/./server      
```

 ## IO多路复用

> **I/O 多路复用使得程序能同时监听多个文件描述符，能够提高程序的性能**
> 
> **Linux 下实现 I/O 多路复用的系统调用主要有 select、poll 和 epoll**

### BIO模型与NIO模型

![阻塞等待](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245033.png)
![BIO模型](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245034.png)
![非阻塞忙轮询](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245035.png)
![NIO模型](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245036.png)

### IO多路转接技术 select/poll

![IO多路转接技术多路转接技术1](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245037.png)

#### select

> 主旨思想：
> 1. 首先要构造一个关于文件描述符的列表，将要监听的文件描述符添加到该列表中。
> 2. 调用一个系统函数，监听该列表中的文件描述符，直到这些描述符中的一个或者多个进行I/O操作时，该函数才返回。
      a.这个函数是阻塞
      b.函数对文件描述符的检测的操作是由内核完成的
> 3. 在返回时，它会告诉进程有多少（哪些）描述符要进行I/O操作。

 ```c++
// sizeof(fd_set) = 128 1024
#include <sys/time.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/select.h>
int select(int nfds, fd_set *readfds, fd_set *writefds,fd_set *exceptfds, struct timeval *timeout);
- 参数：
    - nfds : 委托内核检测的最大文件描述符的值 + 1
    - readfds : 要检测的文件描述符的读的集合，委托内核检测哪些文件描述符的读的属性
        - 一般检测读操作
        - 对应的是对方发送过来的数据，因为读是被动的接收数据，检测的就是读缓冲区
        - 是一个传入传出参数
    - writefds : 要检测的文件描述符的写的集合，委托内核检测哪些文件描述符的写的属性
        - 委托内核检测写缓冲区是不是还可以写数据（不满的就可以写）
    - exceptfds : 检测发生异常的文件描述符的集合
    - timeout : 设置的超时时间
        struct timeval {
        long tv_sec; /* seconds */
        long tv_usec; /* microseconds */
        };
        - NULL : 永久阻塞，直到检测到了文件描述符有变化
        - tv_sec = 0 tv_usec = 0， 不阻塞
        - tv_sec > 0 tv_usec > 0， 阻塞对应的时间
    - 返回值 :
        - -1 : 失败
        - >0(n) : 检测的集合中有n个文件描述符发生了变化
// 将参数文件描述符fd对应的标志位设置为0
void FD_CLR(int fd, fd_set *set);
// 判断fd对应的标志位是0还是1， 返回值 ： fd对应的标志位的值，0，返回0， 1，返回1
int FD_ISSET(int fd, fd_set *set);
// 将参数文件描述符fd 对应的标志位，设置为1
void FD_SET(int fd, fd_set *set);
// fd_set一共有1024 bit, 全部初始化为0
void FD_ZERO(fd_set *set);
 ```
![select工作过程](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245038.png)
![select多路复用](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245039.png)
![select的缺点](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245040.png)

**select.c**
```c++
#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <sys/select.h>

int main() {

    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    struct sockaddr_in saddr;
    saddr.sin_port = htons(9999);
    saddr.sin_family = AF_INET;
    saddr.sin_addr.s_addr = INADDR_ANY;

    // 绑定
    bind(lfd, (struct sockaddr *)&saddr, sizeof(saddr));

    // 监听
    listen(lfd, 8);

    // 创建一个fd_set的集合，存放的是需要检测的文件描述符
    fd_set rdset, tmp;
    FD_ZERO(&rdset);
    FD_SET(lfd, &rdset);
    int maxfd = lfd;

    while(1) {

        tmp = rdset;

        // 调用select系统函数，让内核帮检测哪些文件描述符有数据
        int ret = select(maxfd + 1, &tmp, NULL, NULL, NULL);
        if(ret == -1) {
            perror("select");
            exit(-1);
        } else if(ret == 0) {
            continue;
        } else if(ret > 0) {
            // 说明检测到了有文件描述符的对应的缓冲区的数据发生了改变
            if(FD_ISSET(lfd, &tmp)) {
                // 监听的文件描述符lfd为1 表示有新的客户端连接进来了
                struct sockaddr_in cliaddr;
                int len = sizeof(cliaddr);
                // 返回一个通信的文件描述符
                int cfd = accept(lfd, (struct sockaddr *)&cliaddr, &len);

                
                // 将新的文件描述符加入到集合中 
                FD_SET(cfd, &rdset);

                // 更新最大的文件描述符
                maxfd = maxfd > cfd ? maxfd : cfd;
            }

            for(int i = lfd + 1; i <= maxfd; i++) {
                if(FD_ISSET(i, &tmp)) {
                    // 说明这个文件描述符对应的客户端发来了数据
                    char buf[1024] = {0};
                    int len = read(i, buf, sizeof(buf));
                    if(len == -1) {
                        perror("read");
                        exit(-1);
                    } else if(len == 0) {
                        printf("client %d closed...\n",i);
                        close(i);
                        // 客户端关闭，对应文件描述符置0
                        FD_CLR(i, &rdset);
                    } else if(len > 0) {
                        printf("from %d:read buf = %s \n", i,buf);
                        write(i, buf, strlen(buf) + 1);
                    }
                }
            }

        }

    }
    close(lfd);
    return 0;
}

```

 **client.c**

 ```c++
 #include <stdio.h>
#include <arpa/inet.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main() {

    // 创建socket
    int fd = socket(PF_INET, SOCK_STREAM, 0);
    if(fd == -1) {
        perror("socket");
        return -1;
    }

    struct sockaddr_in seraddr;
    inet_pton(AF_INET, "127.0.0.1", &seraddr.sin_addr.s_addr);
    seraddr.sin_family = AF_INET;
    seraddr.sin_port = htons(9999);

    // 连接服务器
    int ret = connect(fd, (struct sockaddr *)&seraddr, sizeof(seraddr));

    if(ret == -1){
        perror("connect");
        return -1;
    }

    int num = 0;
    while(1) {
        char sendBuf[1024] = {0};
        sprintf(sendBuf, "send data %d", num++);
        write(fd, sendBuf, strlen(sendBuf) + 1);

        // 接收
        int len = read(fd, sendBuf, sizeof(sendBuf));
        if(len == -1) {
            perror("read");
            return -1;
        }else if(len > 0) {
            printf("read buf = %s\n", sendBuf);
        } else {
            printf("服务器已经断开连接...\n");
            break;
        }
        
        sleep(1);
        // usleep(1000);
    }

    close(fd);

    return 0;
}
 ```



 #### poll

 ```c++
#include <poll.h>
struct pollfd {
    int fd; /* 委托内核检测的文件描述符 */
    short events; /* 委托内核检测文件描述符的什么事件 */
    short revents; /* 文件描述符实际发生的事件 */
};
struct pollfd myfd;
myfd.fd = 5;
myfd.events = POLLIN | POLLOUT;
int poll(struct pollfd *fds, nfds_t nfds, int timeout);
    - 参数：
    - fds : 是一个struct pollfd 结构体数组，这是一个需要检测的文件描述符的集合
    - nfds : 这个是第一个参数数组中最后一个有效元素的下标 + 1
    - timeout : 阻塞时长
        0 : 不阻塞
        -1 : 阻塞，当检测到需要检测的文件描述符有变化，解除阻塞
        >0 : 阻塞的时长
    - 返回值：
    -1 : 失败
    >0（n） : 成功,n表示检测到集合中有n个文件描述符发生变化
 ```

![poll多路复用](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245041.png)

 ```c++
#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <poll.h>
int main() {
    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    struct sockaddr_in saddr;
    saddr.sin_port = htons(9999);
    saddr.sin_family = AF_INET;
    saddr.sin_addr.s_addr = INADDR_ANY;
    // 绑定
    bind(lfd, (struct sockaddr *)&saddr, sizeof(saddr));

    // 监听
    listen(lfd, 8);

    // 初始化检测的文件描述符数组
    struct pollfd fds[1024];
    for(int i = 0; i < 1024; i++) {
        fds[i].fd = -1;
        fds[i].events = POLLIN;
    }
    // 加入监听文件描述符
    fds[0].fd = lfd;
    // nfds : 这个是第一个参数数组中最后一个有效元素的下标 + 1
    int nfds = 0;

    while(1) {

        // 调用poll系统函数，让内核帮检测哪些文件描述符有数据
        int ret = poll(fds, nfds + 1, -1);
        if(ret == -1) {
            perror("poll");
            exit(-1);
        } else if(ret == 0) {
            continue;
        } else if(ret > 0) {
            
            // 说明检测到了有文件描述符的对应的缓冲区的数据发生了改变
            if(fds[0].revents & POLLIN) {
                // 表示有新的客户端连接进来了
                struct sockaddr_in cliaddr;
                int len = sizeof(cliaddr);
                int cfd = accept(lfd, (struct sockaddr *)&cliaddr, &len);

                // 将新的文件描述符加入到集合中
                for(int i = 1; i < 1024; i++) {
                    if(fds[i].fd == -1) {
                        fds[i].fd = cfd;
                        fds[i].events = POLLIN;
                        break;
                    }
                }

                // 更新最大的文件描述符的索引
                nfds = nfds > cfd ? nfds : cfd;
            }

            for(int i = 1; i <= nfds; i++) {
                if(fds[i].revents & POLLIN) {
                    // 说明这个文件描述符对应的客户端发来了数据
                    char buf[1024] = {0};
                    int len = read(fds[i].fd, buf, sizeof(buf));
                    if(len == -1) {
                        perror("read");
                        exit(-1);
                    } else if(len == 0) {
                        printf("client closed...\n");
                        close(fds[i].fd);
                        fds[i].fd = -1;
                    } else if(len > 0) {
                        printf("read buf = %s\n", buf);
                        write(fds[i].fd, buf, strlen(buf) + 1);
                    }
                }
            }

        }

    }
    close(lfd);
    return 0;
}
 ```

#### epoll

```c++
#include <sys/epoll.h>
// 创建一个新的epoll实例。在内核中创建了一个数据，这个数据中有两个比较重要的数据，一个是需要检
测的文件描述符的信息（红黑树），还有一个是就绪列表，存放检测到数据发送改变的文件描述符信息（双向链表）
int epoll_create(int size);
- 参数：
    size : 目前没有意义了。随便写一个数，必须大于0
- 返回值：
    -1 : 失败
    > 0 : 文件描述符，操作epoll实例的

typedef union epoll_data {
    void *ptr;
    int fd;
    uint32_t u32;
    uint64_t u64;
} epoll_data_t;

struct epoll_event {
    uint32_t events; /* Epoll events */
    epoll_data_t data; /* User data variable */
};
常见的Epoll检测事件：
    - EPOLLIN
    - EPOLLOUT
    - EPOLLERR
// 对epoll实例进行管理：添加文件描述符信息，删除信息，修改信息
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
    - 参数：
        - epfd : epoll实例对应的文件描述符
        - op : 要进行什么操作
            EPOLL_CTL_ADD: 添加
            EPOLL_CTL_MOD: 修改
            EPOLL_CTL_DEL: 删除
        - fd : 要检测的文件描述符
        - event : 检测文件描述符什么事情
// 检测函数
int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);
    - 参数：
        - epfd : epoll实例对应的文件描述符
        - events : 传出参数，保存了发送了变化的文件描述符的信息
        - maxevents : 第二个参数结构体数组的大小
        - timeout : 阻塞时间
            - 0 : 不阻塞
            - -1 : 阻塞，直到检测到fd数据发生变化，解除阻塞
            - > 0 : 阻塞的时长（毫秒）
        - 返回值：
            - 成功，返回发送变化的文件描述符的个数 > 0
            - 失败 -1
```
**Epoll 的工作模式**

- LT 模式 （水平触发）
    假设委托内核检测读事件 -> 检测fd的读缓冲区   
    读缓冲区有数据 - > epoll检测到了会给用户通知  
    a.用户不读数据，数据一直在缓冲区，epoll 会一直通知
    b.用户只读了一部分数据，epoll会通知
    c.缓冲区的数据读完了，不通知
> LT（level - triggered）是缺省的工作方式，并且同时支持 block 和 no-block socket。在这
种做法中，内核告诉你一个文件描述符是否就绪了，然后你可以对这个就绪的 fd 进行 IO 操
作。如果你不作任何操作，内核还是会继续通知你的

- ET 模式（边沿触发）
    假设委托内核检测读事件 -> 检测fd的读缓冲区
    读缓冲区有数据 - > epoll检测到了会给用户通知
    a.用户不读数据，数据一致在缓冲区中，epoll下次检测的时候就不通知了
    b.用户只读了一部分数据，epoll不通知
    c.缓冲区的数据读完了，不通知

> ET（edge - triggered）是高速工作方式，只支持 no-block socket。在这种模式下，当描述
符从未就绪变为就绪时，内核通过epoll告诉你。然后它会假设你知道文件描述符已经就绪，
并且不会再为那个文件描述符发送更多的就绪通知，直到你做了某些操作导致那个文件描述
符不再为就绪状态了。但是请注意，如果一直不对这个 fd 作 IO 操作（从而导致它再次变成
未就绪），内核不会发送更多的通知（only once）。
ET 模式在很大程度上减少了 epoll 事件被重复触发的次数，因此效率要比 LT 模式高。epoll
工作在 ET 模式的时候，必须使用非阻塞套接口，以避免由于一个文件句柄的阻塞读/阻塞写
操作把处理多个文件描述符的任务饿死。

```c++
struct epoll_event {
    uint32_t events; /* Epoll events */
    epoll_data_t data; /* User data variable */
};
常见的Epoll检测事件：
    - EPOLLIN
    - EPOLLOUT
    - EPOLLERR
    - EPOLLET
```
 ![epoll多路复用图解](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245042.png)

 **client.c**
 ```c++
 #include <stdio.h>
#include <arpa/inet.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main() {

    // 创建socket
    int fd = socket(PF_INET, SOCK_STREAM, 0);
    if(fd == -1) {
        perror("socket");
        return -1;
    }

    struct sockaddr_in seraddr;
    inet_pton(AF_INET, "127.0.0.1", &seraddr.sin_addr.s_addr);
    seraddr.sin_family = AF_INET;
    seraddr.sin_port = htons(9998);

    // 连接服务器
    int ret = connect(fd, (struct sockaddr *)&seraddr, sizeof(seraddr));

    if(ret == -1){
        perror("connect");
        return -1;
    }

    int num = 0;
	char sendBuf[1024] = {0};
        // sprintf(sendBuf, "send data %d", num++);
    fgets(sendBuf, sizeof(sendBuf), stdin);

    write(fd, sendBuf, strlen(sendBuf));
	
    while(1) {
        // 接收
        
        memset(sendBuf,'\0',sizeof(sendBuf));
        int len = read(fd, sendBuf, sizeof(sendBuf));
        if(len == -1) {
            perror("read");
            return -1;
        }else if(len > 0) {
            printf("read buf = %s\n", sendBuf);
        } else {
            printf("服务器已经断开连接...\n");
            break;
        }
    }

    close(fd);

    return 0;
}
 ```

**epoll_lt.c**
```c++
#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <sys/epoll.h>

int main() {

    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    struct sockaddr_in saddr;
    saddr.sin_port = htons(9998);
    saddr.sin_family = AF_INET;
    saddr.sin_addr.s_addr = INADDR_ANY;

    // 绑定
    bind(lfd, (struct sockaddr *)&saddr, sizeof(saddr));

    // 监听
    listen(lfd, 8);

    // 调用epoll_create()创建一个epoll实例
    int epfd = epoll_create(100);

    // 将监听的文件描述符相关的检测信息添加到epoll实例中
    struct epoll_event epev;
    epev.events = EPOLLIN;
    epev.data.fd = lfd;
    epoll_ctl(epfd, EPOLL_CTL_ADD, lfd, &epev);

    struct epoll_event epevs[1024];

    while(1) {

        int ret = epoll_wait(epfd, epevs, 1024, -1);
        if(ret == -1) {
            perror("epoll_wait");
            exit(-1);
        }

        printf("ret = %d\n", ret);

        for(int i = 0; i < ret; i++) {

            int curfd = epevs[i].data.fd;

            if(curfd == lfd) {
                // 监听的文件描述符有数据达到，有客户端连接
                struct sockaddr_in cliaddr;
                int len = sizeof(cliaddr);
                int cfd = accept(lfd, (struct sockaddr *)&cliaddr, &len);

                epev.events = EPOLLIN;
                epev.data.fd = cfd;
                epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &epev);
            } else {
                if(epevs[i].events & EPOLLOUT) {
                    continue;
                }   
                // 有数据到达，需要通信
                // 长度是3
                // a b c
                char buf[3] = {0};
                int len = read(curfd, buf, sizeof(buf)-1);
                if(len == -1) {
                    perror("read");
                    exit(-1);
                } else if(len == 0) {
                    printf("client closed...\n");
                    epoll_ctl(epfd, EPOLL_CTL_DEL, curfd, NULL);
                    close(curfd);
                } else if(len > 0) {
                    buf[len]='\0';
                    printf("read buf = %s\n", buf);
                    //write(curfd, buf, strlen(buf) + 1);
                    write(curfd, buf, 2);
                }

            }

        }
    }

    close(lfd);
    close(epfd);
    return 0;
}
```

 **epoll_et.c**

 ```c++
#include <stdio.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <sys/epoll.h>
#include <fcntl.h>
#include <errno.h>

int main() {

    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    struct sockaddr_in saddr;
    saddr.sin_port = htons(9999);
    saddr.sin_family = AF_INET;
    saddr.sin_addr.s_addr = INADDR_ANY;

    // 绑定
    bind(lfd, (struct sockaddr *)&saddr, sizeof(saddr));

    // 监听
    listen(lfd, 8);

    // 调用epoll_create()创建一个epoll实例
    int epfd = epoll_create(100);

    // 将监听的文件描述符相关的检测信息添加到epoll实例中
    struct epoll_event epev;
    epev.events = EPOLLIN;
    epev.data.fd = lfd;
    epoll_ctl(epfd, EPOLL_CTL_ADD, lfd, &epev);

    struct epoll_event epevs[1024];

    while(1) {

        int ret = epoll_wait(epfd, epevs, 1024, -1);
        if(ret == -1) {
            perror("epoll_wait");
            exit(-1);
        }

        printf("ret = %d\n", ret);

        for(int i = 0; i < ret; i++) {

            int curfd = epevs[i].data.fd;

            if(curfd == lfd) {
                // 监听的文件描述符有数据达到，有客户端连接
                struct sockaddr_in cliaddr;
                int len = sizeof(cliaddr);
                int cfd = accept(lfd, (struct sockaddr *)&cliaddr, &len);

                // 设置cfd属性非阻塞
                int flag = fcntl(cfd, F_GETFL);
                flag |= O_NONBLOCK;
                fcntl(cfd, F_SETFL, flag);

                epev.events = EPOLLIN | EPOLLET;    // 设置边沿触发
                epev.data.fd = cfd;
                epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &epev);
            } else {
                if(epevs[i].events & EPOLLOUT) {
                    continue;
                }  

                // 循环读取出所有数据
                char buf[5];
                int len = 0;
                while( (len = read(curfd, buf, sizeof(buf))) > 0) {
                    // 打印数据
                    // printf("recv data : %s\n", buf);
                    write(STDOUT_FILENO, buf, len);
                    write(curfd, buf, len);
                }
                if(len == 0) {
                    printf("client closed....");
                }else if(len == -1) {
                    // 考虑文件读完情况 
                    if(errno == EAGAIN) {
                        printf("data over.....");
                    }else {
                        perror("read");
                        exit(-1);
                    }
                    
                }

            }

        }
    }

    close(lfd);
    close(epfd);
    return 0;
}
 ```
 ## UDP通信的实现
 ![/UDP通信过程](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245043.png)

```C++
#include <sys/types.h>
#include <sys/socket.h>
ssize_t sendto(int sockfd, const void *buf, size_t len, int flags,
const struct sockaddr *dest_addr, socklen_t addrlen);
    - 参数：
        - sockfd : 通信的fd
        - buf : 要发送的数据
        - len : 发送数据的长度
        - flags : 0
        - dest_addr : 通信的另外一端的地址信息
        - addrlen : 地址的内存大小
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags,
struct sockaddr *src_addr, socklen_t *addrlen);
    - 参数：
        - sockfd : 通信的fd
        - buf : 接收数据的数组
        - len : 数组的大小
        - flags : 0
        - src_addr : 用来保存另外一端的地址信息，不需要可以指定为NULL
        - addrlen : 地址的内存大小
```

 **udp_server.c**

 ```c++
 #include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>

int main() {

    // 1.创建一个通信的socket
    int fd = socket(PF_INET, SOCK_DGRAM, 0);
    
    if(fd == -1) {
        perror("socket");
        exit(-1);
    }   

    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(9999);
    addr.sin_addr.s_addr = INADDR_ANY;

    // 2.绑定
    int ret = bind(fd, (struct sockaddr *)&addr, sizeof(addr));
    if(ret == -1) {
        perror("bind");
        exit(-1);
    }

    // 3.通信
    while(1) {
        char recvbuf[128];
        char ipbuf[16];

        struct sockaddr_in cliaddr;
        int len = sizeof(cliaddr);

        // 接收数据
        int num = recvfrom(fd, recvbuf, sizeof(recvbuf), 0, (struct sockaddr *)&cliaddr, &len);

        printf("client IP : %s, Port : %d\n", 
            inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ipbuf, sizeof(ipbuf)),
            ntohs(cliaddr.sin_port));

        printf("client say : %s\n", recvbuf);

        // 发送数据
        sendto(fd, recvbuf, strlen(recvbuf) + 1, 0, (struct sockaddr *)&cliaddr, sizeof(cliaddr));

    }

    close(fd);
    return 0;
}
 ```
**udp_client.c**

```c++
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>

int main() {

    // 1.创建一个通信的socket
    int fd = socket(PF_INET, SOCK_DGRAM, 0);
    
    if(fd == -1) {
        perror("socket");
        exit(-1);
    }   

    // 服务器的地址信息
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(9999);
    inet_pton(AF_INET, "127.0.0.1", &saddr.sin_addr.s_addr);

    int num = 0;
    // 3.通信
    while(1) {

        // 发送数据
        char sendBuf[128];
        sprintf(sendBuf, "hello , i am client %d \n", num++);
        sendto(fd, sendBuf, strlen(sendBuf) + 1, 0, (struct sockaddr *)&saddr, sizeof(saddr));

        // 接收数据
        int num = recvfrom(fd, sendBuf, sizeof(sendBuf), 0, NULL, NULL);
        printf("server say : %s\n", sendBuf);

        sleep(1);
    }

    close(fd);
    return 0;
}
```


## 广播

> 向子网中多台计算机发送消息，并且子网中所有的计算机都可以接收到发送方发送的消息，每个广
播消息都包含一个特殊的IP地址，这个IP中子网内主机标志部分的二进制全部为1。
a.只能在局域网中使用。
b.客户端需要绑定服务器广播使用的端口，才可以接收到广播消息。

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245044.png)

```c++
// 设置广播属性的函数
int setsockopt(int sockfd, int level, int optname,const void *optval, socklen_t
optlen);
    - sockfd : 文件描述符
    - level : SOL_SOCKET
    - optname : SO_BROADCAST
    - optval : int类型的值，为1表示允许广播
    - optlen : optval的大小

```


**code 见lesson37 bro_server.c**


## 组播（多播）

> 单播地址标识单个 IP 接口，广播地址标识某个子网的所有 IP 接口，多播地址标识一组 IP 接口。单播和广播是寻址方案的两个极端（要么单个要么全部），多播则意在两者之间提供一种折中方案。
> 
> 多播数据报只应该由对它感兴趣的接口接收，也就是说由运行相应多播会话应用系统的主机上的接口接收。另外，广播一般局限于局域网内使用，而多播则既可以用于局域网，也可以跨广域网使用。
> 
> a.组播既可以用于局域网，也可以用于广域网
> b.客户端需要加入多播组，才能接收到多播的数据

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245045.png)
![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245046.png)

```c++
int setsockopt(int sockfd, int level, int optname,const void *optval,
socklen_t optlen);
// 服务器设置多播的信息，外出接口
    - level : IPPROTO_IP
    - optname : IP_MULTICAST_IF
    - optval : struct in_addr
    // 客户端加入到多播组：
    - level : IPPROTO_IP
    - optname : IP_ADD_MEMBERSHIP
    - optval : struct ip_mreq
struct ip_mreq
{
    /* IP multicast address of group. */
    struct in_addr imr_multiaddr; // 组播的IP地址
    /* Local IP address of interface. */
    struct in_addr imr_interface; // 本地的IP地址
};
typedef uint32_t in_addr_t;

struct in_addr
{
    in_addr_t s_addr;
};
```
 **code 见lesson37 multi_server.c**

 ## 本地套接字

 > 本地套接字的作用：本地的进程间通信
有关系的进程间的通信
没有关系的进程间的通信
本地套接字实现流程和网络套接字类似，一般呢采用TCP的通信流程。

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245047.png)

 ```c++
// 本地套接字通信的流程 - tcp
// 服务器端
1. 创建监听的套接字
    int lfd = socket(AF_UNIX/AF_LOCAL, SOCK_STREAM, 0);

2. 监听的套接字绑定本地的套接字文件 -> server端
    struct sockaddr_un addr;
    // 绑定成功之后，指定的sun_path中的套接字文件会自动生成。
    bind(lfd, addr, len);

3. 监听
    listen(lfd, 100);

4. 等待并接受连接请求
    struct sockaddr_un cliaddr;
    int cfd = accept(lfd, &cliaddr, len);

5. 通信
    接收数据：read/recv
    发送数据：write/send

6. 关闭连接
    close();
    // 客户端的流程
    1. 创建通信的套接字
    int fd = socket(AF_UNIX/AF_LOCAL, SOCK_STREAM, 0);
    2. 监听的套接字绑定本地的IP 端口
    struct sockaddr_un addr;
    // 绑定成功之后，指定的sun_path中的套接字文件会自动生成。
    bind(lfd, addr, len);

3. 连接服务器
    struct sockaddr_un serveraddr;
    connect(fd, &serveraddr, sizeof(serveraddr));

4. 通信
    接收数据：read/recv
    发送数据：write/send

5. 关闭连接
close();

// 头文件: sys/un.h
#define UNIX_PATH_MAX 108
struct sockaddr_un {
sa_family_t sun_family; // 地址族协议 af_local
char sun_path[UNIX_PATH_MAX]; // 套接字文件的路径, 这是一个伪文件, 大小永远=0
};
 ```

**ipc_server.c**

```c++
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
#include <arpa/inet.h>
#include <sys/un.h>

int main() {

    unlink("server.sock");

    // 1.创建监听的套接字
    int lfd = socket(AF_LOCAL, SOCK_STREAM, 0);
    if(lfd == -1) {
        perror("socket");
        exit(-1);
    }

    // 2.绑定本地套接字文件
    // 绑定成功之后，指定的sun_path中的套接字文件会自动生成。
    struct sockaddr_un addr;
    addr.sun_family = AF_LOCAL;
    strcpy(addr.sun_path, "server.sock");
    int ret = bind(lfd, (struct sockaddr *)&addr, sizeof(addr));
    if(ret == -1) {
        perror("bind");
        exit(-1);
    }

    // 3.监听
    ret = listen(lfd, 100);
    if(ret == -1) {
        perror("listen");
        exit(-1);
    }

    // 4.等待客户端连接
    struct sockaddr_un cliaddr;
    int len = sizeof(cliaddr);
    int cfd = accept(lfd, (struct sockaddr *)&cliaddr, &len);
    if(cfd == -1) {
        perror("accept");
        exit(-1);
    }

    printf("client socket filename: %s\n", cliaddr.sun_path);

    // 5.通信
    while(1) {

        char buf[128];
        int len = recv(cfd, buf, sizeof(buf), 0);

        if(len == -1) {
            perror("recv");
            exit(-1);
        } else if(len == 0) {
            printf("client closed....\n");
            break;
        } else if(len > 0) {
            printf("client say : %s\n", buf);
            send(cfd, buf, len, 0);
        }

    }

    close(cfd);
    close(lfd);

    return 0;
}
```

 **ipc_client.c**

 ```c++
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>
#include <arpa/inet.h>
#include <sys/un.h>

int main() {

    unlink("client.sock");

    // 1.创建套接字
    int cfd = socket(AF_LOCAL, SOCK_STREAM, 0);
    if(cfd == -1) {
        perror("socket");
        exit(-1);
    }

    // 2.绑定本地套接字文件
    struct sockaddr_un addr;
    addr.sun_family = AF_LOCAL;
    strcpy(addr.sun_path, "client.sock");
    int ret = bind(cfd, (struct sockaddr *)&addr, sizeof(addr));
    if(ret == -1) {
        perror("bind");
        exit(-1);
    }

    // 3.连接服务器
    struct sockaddr_un seraddr;
    seraddr.sun_family = AF_LOCAL;
    strcpy(seraddr.sun_path, "server.sock");
  
    ret = connect(cfd, (struct sockaddr *)&seraddr, sizeof(seraddr));
    if(ret == -1) {
        perror("connect");
        exit(-1);
    }

    // 4.通信
    int num = 0;
    while(1) {

        // 发送数据
        char buf[128];
        sprintf(buf, "hello, i am client %d\n", num++);
        send(cfd, buf, strlen(buf) + 1, 0);
        printf("client say : %s\n", buf);

        // 接收数据
        int len = recv(cfd, buf, sizeof(buf), 0);

        if(len == -1) {
            perror("recv");
            exit(-1);
        } else if(len == 0) {
            printf("server closed....\n");
            break;
        } else if(len > 0) {
            printf("server say : %s\n", buf);
        }

        sleep(1);

    }

    close(cfd);
    return 0;
}
 ```

# 第5章 项目实战与总结

## Unix/Linux上的五种IO模型

### 网络IO模型

网络IO涉及**用户空间**和**内核空间**，一般会经历两个阶段：

- **一阶段**：**等待数据准备就绪**，即等待网络数据被copy到内核缓冲区

- **二阶段**：**将数据从内核缓冲区copy到用户缓冲区**

  

**陈硕：在处理 IO 的时候，阻塞和非阻塞都是同步 IO，只有使用了特殊的 API 才是异步 IO**

- 网络IO：网络通信从TCP内核缓存区读写
- 磁盘IO：应用程序从磁盘读数据到内存，或者修改内存数据写入磁盘

**同步与异步**
在网络编程中，同步和异步通常用于描述网络请求和响应之间的交互方式。同步通信也称为阻塞式通信，发送请求后程序将被阻塞等待响应返回，直到接收到响应后才能继续执行后续操作。异步通信也称为非阻塞式通信，发送请求后程序将继续执行后续操作，而不需要等待响应返回，当响应返回后会触发相应的回调函数进行处理。使用异步通信可以提高网络通信效率，避免线程阻塞等问题。

**并发与并行**
在网络编程中，并发和并行通常用于描述服务器处理多个客户端请求的方式。并发是指服务器能够同时处理多个客户端请求，同时每个客户端请求看起来都在单独等待自己的响应，但实际上，服务器使用了一些机制来处理所有请求，例如线程池等。并发可以提高服务器的吞吐量和响应能力。

相比之下，并行是指服务器在处理多个客户端请求时使用多个处理器或核心同时进行处理。在并行处理中，服务器能够同时对多个客户端请求进行处理，每个核心或处理器可用于处理不同的请求。并行可以显著提高服务器的处理速度，但是不同请求之间可能会发生资源竞争和同步问题等，需要特别注意。
    
在操作系统中，同步与异步、并发与并行等概念经常被提及。它们主要用于描述操作系统处理任务的方式，下面对它们进行详细解释：

同步和异步通常用于描述操作系统任务之间的调度方式。同步任务是指任务之间必须按照一定的先后顺序依次执行，每个任务的完成都依赖于前一个任务的完成。同步任务需要等待前一个任务完成后才能进行下一个任务，如果前一个任务未完成，那么它将一直等待下去。而异步任务则是指任务之间相互独立，每个任务都能够独立执行，不需要等待其他任务的完成。异步任务能够有效地避免任务之间相互阻塞的问题，提高系统的响应能力。


![阻塞、非阻塞、同步、异步](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245048.png) 

##### 1. 阻塞IO

调用者调用了某个函数，等待这个函数返回，期间什么也不做，不停的检查这个函数有没有返回，必须等待这个函数返回才能进行下一步动作。用户进程全程阻塞直到两阶段完成，即，一阶段等待数据会阻塞，二阶段将数据从内核copy到用户空间也会阻塞，只有copy完数据后内核返回，用户进程才会解除阻塞状态，重新运行。

linux中socket默认blocking

缺点：同一时刻智能处理一个操作，效率低

结论：阻塞IO，两阶段都阻塞。


![阻塞blocking](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245049.png)

##### 2. 非阻塞IO

用户进程系统调用时，如果没有数据，则直接返回，不管事件有没有发生，若没有发生，则返回-1。因此一阶段数据准备不会阻塞用户进程。但是，用户进程需要不断的询问内核数据是否准备好（会造成CPU空转浪费资源，因此很少使用）。当数据准备好时，用户进程会阻塞直到**数据从内核空间copy到用户空间完成（**二阶段），内核返回结果。

可使用fcntl将socket设置为NON-BLOCKING(fcntl(fd, F_SETFL, O_NONBLOCK);)，使其变为非阻塞。

这里需要单独判断一下返回值为-1的情况，以recv 返回值为例，当返回值为-1时，需要先判断errno，errno为EAGAIN表示recv操作未完成，如果errno 为 EWOULDBLOCK, 表示无数据，这两种情况下都不是真正的系统错误，排除这两个以后，表示遇到系统错误。含义：

- 大于0，接收数据完毕，返回值即收到的字节数
- 等于0，连接已经正常断开

缺点：忙轮询，需要占用CPU资源。

结论：非阻塞IO一阶段不阻塞，二阶段阻塞。


![非阻塞non-blocking](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245050.png)

##### 3. IO多路复用 (select/poll/epoll)

linux通过使用select/poll/epoll函数实现IO复用，这些函数本身是阻塞的。但是和阻塞IO所不同的是这些函数可以同时阻塞多个IO操作。可在单个进程/线程中同时监听多个网络连接的socket fd，一旦有事件触发，才真正的调用IO操作函数，进行相应处理。

结论：两阶段都处于阻塞状态，优点是单个线程可同时监听和处理多个网络连接。

> 如果连接数不是很高的话，使用select/epoll的web server不一定比使用multi-threading + blocking IO的web server性能更好，可能延迟更大。因为前者需要两个系统调用(select/epoll + read)，而后者只有一个(read)。但是在连接数很多的情况下，select/epoll的优势就凸显出来了。
> 

![IO复用](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245051.png)


##### 4. 信号驱动IO
linux用socket进行信号驱动IO，通过sigaction系统调用，建立起信号驱动IO的socket，并绑定一个信号处理函数；sigaction不会阻塞，立即返回。进程继续运行。当数据准备好，内核就为进程产生一个SIGIO信号，随后在信号处理函数中调用recv接收数据。

与非阻塞IO的区别在于它提供了消息通知机制，不需要用户进程不断地轮询检查，减少了系统调用次数，提高了效率。

结论：一阶段不阻塞（异步），二阶段阻塞（同步）

以上四种模型都有一个共同点：二阶段阻塞，也就是在真正IO操作的时候需要用户进程参与，因此以上四种模型均称为同步IO模型。


![IO复用](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245051.png)

> **内核在第一个阶段是异步，在第二个阶段是同步**；与非阻塞IO的区别在于它提供了消息通知机制，不需要用户进程不断的轮询检查，减少了系统API的调用次数，提高了效率


##### 5. 异步IO
Linux中提供了异步IO的接口aio_read和aio_write，内核收到用户进程的aio_read之后会立即返回，不会阻塞，aio_read会给内核传递文件描述符，缓冲区指针，缓冲区大小，文件偏移等；当数据准备好，内核直接将数据copy到用户空间，copy完后给用户进程发送一个信号，进行用户数据异步处理（aio_read）。因此，异步IO中用户进程是不需要参与数据从内核空间copy到用户空间这个过程的，也即二阶段不阻塞。

结论：两阶段都不阻塞

![异步](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245052.png)

```c++
/* Asynchronous I/O control block. （异步IO控制块）*/
struct aiocb
{
    int aio_fildes; /* File desriptor. */
    int aio_lio_opcode; /* Operation to be performed. */
    int aio_reqprio; /* Request priority offset. */
    volatile void *aio_buf; /* Location of buffer. */
    size_t aio_nbytes; /* Length of transfer. */
    struct sigevent aio_sigevent; /* Signal number and value. */

    /* Internal members. */
    struct aiocb *__next_prio;
    int __abs_prio;
    int __policy;
    int __error_code;
    __ssize_t __return_value;

    #ifndef __USE_FILE_OFFSET64
        __off_t aio_offset; /* File offset. */
        char __pad[sizeof (__off64_t) - sizeof (__off_t)];
    #else
        __off64_t aio_offset; /* File offset. */
    #endif
        char __glibc_reserved[32];
};
```

## Web Server

> 一个 Web Server 就是一个服务器软件（程序），或者是运行这个服务器软件的硬件（计算机）。其主
要功能是通过 HTTP 协议与客户端（通常是浏览器（Browser））进行通信，来接收，存储，处理来自
客户端的 HTTP 请求，并对其请求做出 HTTP 响应，返回给客户端其请求的内容（文件、网页等）或返
回一个 Error 信息。

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245053.png)

> 通常用户使用 Web 浏览器与相应服务器进行通信。在浏览器中键入“域名”或“IP地址:端口号”，浏览器则
先将你的域名解析成相应的 IP 地址或者直接根据你的IP地址向对应的 Web 服务器发送一个 HTTP 请
求。这一过程首先要通过 TCP 协议的三次握手建立与目标 Web 服务器的连接，然后 HTTP 协议生成针
对目标 Web 服务器的 HTTP 请求报文，通过 TCP、IP 等协议发送到目标 Web 服务器上。

###  HTTP协议(应用层的协议)

### 简介

> 超文本传输协议（Hypertext Transfer Protocol，HTTP）是一个简单的请求 - 响应协议，它通常运行在
TCP 之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。请求和响应消息的
头以 ASCII 形式给出；而消息内容则具有一个类似 MIME 的格式。HTTP是万维网的数据通信的基础。

### 概述

> HTTP 是一个客户端终端（用户）和服务器端（网站）请求和应答的标准（TCP）。通过使用网页浏览
器、网络爬虫或者其它的工具，客户端发起一个HTTP请求到服务器上指定端口（默认端口为80）。我们
称这个客户端为用户代理程序（user agent）。应答的服务器上存储着一些资源，比如 HTML 文件和图
像。我们称这个应答服务器为源服务器（origin server）。在用户代理和源服务器中间可能存在多个“中
间层”，比如代理服务器、网关或者隧道（tunnel）。

> 尽管 TCP/IP 协议是互联网上最流行的应用，HTTP 协议中，并没有规定必须使用它或它支持的层。事实
上，HTTP可以在任何互联网协议上，或其他网络上实现。HTTP 假定其下层协议提供可靠的传输。因
此，任何能够提供这种保证的协议都可以被其使用。因此也就是其在 TCP/IP 协议族使用 TCP 作为其传
输层。

> 通常，由HTTP客户端发起一个请求，创建一个到服务器指定端口（默认是80端口）的 TCP 连接。HTTP
服务器则在那个端口监听客户端的请求。一旦收到请求，服务器会向客户端返回一个状态，比
如"HTTP/1.1 200 OK"，以及返回的内容，如请求的文件、错误消息、或者其它信息。


### 工作原理

> HTTP 协议定义 Web 客户端如何从 Web 服务器请求 Web 页面，以及服务器如何把 Web 页面传送给客
户端。HTTP 协议采用了请求/响应模型。客户端向服务器发送一个请求报文，请求报文包含请求的方
法、URL、协议版本、请求头部和请求数据。服务器以一个状态行作为响应，响应的内容包括协议的版
本、成功或者错误代码、服务器信息、响应头部和响应数据。

以下是 HTTP 请求/响应的步骤：
1. 客户端连接到 Web 服务器
一个HTTP客户端，通常是浏览器，与 Web 服务器的 HTTP 端口（默认为 80 ）建立一个 TCP 套接字连接。例如，http://www.baidu.com。（URL）

2. 发送 HTTP 请求
通过 TCP 套接字，客户端向 Web 服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据 4 部分组成。

3. 服务器接受请求并返回 HTTP 响应
Web 服务器解析请求，定位请求资源。服务器将资源复本写到 TCP 套接字，由客户端读取。一个响应由状态行、响应头部、空行和响应数据 4 部分组成。

4. 释放连接 TCP 连接
若 connection 模式为 close，则服务器主动关闭 TCP连接，客户端被动关闭连接，释放 TCP 连接；若connection 模式为 keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求;

5. 客户端浏览器解析 HTML 内容
客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的 HTML 文档和文档的字符集。客户端浏览器读取响应数据 HTML，根据HTML 的语法对其进行格式化，并在浏览器窗口中显示。

例如：在浏览器地址栏键入URL，按下回车之后会经历以下流程：
1. 浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;
2. 解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立 TCP 连接;
3. 浏览器发出读取文件（ URL 中域名后面部分对应的文件）的 HTTP 请求，该请求报文作为 TCP 三
次握手的第三个报文的数据发送给服务器;
4. 服务器对浏览器请求作出响应，并把对应的 HTML 文本发送给浏览器;
5. 释放 TCP 连接;
6. 浏览器将该 HTML 文本并显示内容。

> HTTP 协议是基于 TCP/IP 协议之上的应用层协议，基于 请求-响应 的模式。HTTP 协议规定，请求从客
户端发出，最后服务器端响应该请求并返回。换句话说，肯定是先从客户端开始建立通信的，服务器端
在没有接收到请求之前不会发送响应。

### HTTP 请求报文格式

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245054.png)

​	**请求首行**

> GET / HTTP/1.1Host: www.baidu.com

​	**请求头**

> User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:86.0) Gecko/20100101 Firefox/86.0
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,/;q=0.8
> Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
> Accept-Encoding: gzip, deflate, br
> Connection: keep-alive
> Cookie: BAIDUID=6729CB682DADC2CF738F533E35162D98:FG=1;
> BIDUPSID=6729CB682DADC2CFE015A8099199557E; PSTM=1614320692; BD_UPN=13314752;
> BDORZ=FFFB88E999055A3F8A630C64834BD6D0;
> __yjs_duid=1_d05d52b14af4a339210722080a668ec21614320694782; BD_HOME=1;
> H_PS_PSSID=33514_33257_33273_31660_33570_26350;
> BA_HECTOR=8h2001alag0lag85nk1g3hcm60q
> Upgrade-Insecure-Requests: 1
> Cache-Control: max-age=0

### HTTP响应报文格式

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245055.png)

> HTTP/1.1 200 OK
Bdpagetype: 1
Bdqid: 0xf3c9743300024ee4
Cache-Control: private
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html;charset=utf-8
Date: Fri, 26 Feb 2021 08:44:35 GMT
Expires: Fri, 26 Feb 2021 08:44:35 GMT
Server: BWS/1.1
Set-Cookie: BDSVRTM=13; path=/
Set-Cookie: BD_HOME=1; path=/
Set-Cookie: H_PS_PSSID=33514_33257_33273_31660_33570_26350; path=/; domain=.baidu.com
Strict-Transport-Security: max-age=172800
Traceid: 1614329075128412289017566699583927635684
X-Ua-Compatible: IE=Edge,chrome=1
Transfer-Encoding: chunked


### HTTP请求方法
HTTP/1.1 协议中共定义了八种方法（也叫“动作”）来以不同方式操作指定的资源：
1. GET：向指定的资源发出“显示”请求。使用 GET 方法应该只用在读取数据，而不应当被用于产生“副作用”的操作中，例如在 Web Application 中。其中一个原因是 GET 可能会被网络蜘蛛等随意访问。

2. HEAD：与 GET 方法一样，都是向服务器发出指定资源的请求。只不过服务器将不传回资源的本文部分。它的好处在于，使用这个方法可以在不必传输全部内容的情况下，就可以获取其中“关于该资源的信息”（元信息或称元数据）。

3. POST：向指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。数据被包含在请求本文中。这个请求可能会创建新的资源或修改现有资源，或二者皆有。

4. PUT：向指定资源位置上传其最新内容

5. DELETE：请求服务器删除 Request-URI 所标识的资源。
  
6. TRACE：回显服务器收到的请求，主要用于测试或诊断
  
7.  OPTIONS：这个方法可使服务器传回该资源所支持的所有 HTTP 请求方法。用'*'来代替资源名称，向 Web 服务器发送 OPTIONS 请求，可以测试服务器功能是否正常运作。
  
8. CONNECT：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。通常用于SSL加密服务器的链接（经由非加密的 HTTP 代理服务器）。


### HTTP状态码
所有HTTP响应的第一行都是状态行，依次是当前HTTP版本号，3位数字组成的状态代码，以及描述状态
的短语，彼此由空格分隔。
状态代码的第一个数字代表当前响应的类型：
1xx消息——请求已被服务器接收，继续处理
2xx成功——请求已成功被服务器接收、理解、并接受
3xx重定向——需要后续操作才能完成这一请求
4xx请求错误——请求含有词法错误或者无法被执行
5xx服务器错误——服务器在处理某个正确请求时发生错误


###  服务器编程基本框架

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245056.png)

|模块|             功能|
|:---:|:-------------:|
|I/O处理单元  |  处理客户连接，读写网络数据|
|逻辑单元      | 业务进程或线程|
|网络存储单元   | 数据库、文件或缓存|
|请求队列        |各单元之间的通信方式|

> I/O 处理单元是服务器管理客户连接的模块。它通常要完成以下工作：等待并接受新的客户连接，接收客户数据，将服务器响应数据返回给客户端。但是数据的收发不一定在 I/O 处理单元中执行，也可能在逻辑单元中执行，具体在何处执行取决于事件处理模式。

> 一个逻辑单元通常是一个进程或线程。它分析并处理客户数据，然后将结果传递给 I/O 处理单元或者直接发送给客户端（具体使用哪种方式取决于事件处理模式）。服务器通常拥有多个逻辑单元，以实现对多个客户任务的并发处理。

> 网络存储单元可以是数据库、缓存和文件，但不是必须的。

> 请求队列是各单元之间的通信方式的抽象。I/O 处理单元接收到客户请求时，需要以某种方式通知一个逻辑单元来处理该请求。同样，多个逻辑单元同时访问一个存储单元时，也需要采用某种机制来协调处理竞态条件。请求队列通常被实现为池的一部分。

###  两种高效的事件处理模式

服务器程序通常需要处理三类事件：I/O 事件、信号及定时事件。有两种高效的事件处理模式：Reactor和 Proactor，==同步 I/O 模型通常用于实现 Reactor 模式==，异步 I/O 模型通常用于实现 Proactor 模式。

### Reactor模式

> 要求**主线程（I/O处理单元）只负责监听文件描述符上是否有事件发生**，有的话就立即将该事件通知工作线程（逻辑单元），将 socket 可读可写事件放入请求队列，交给工作线程处理。除此之外，主线程不做任何其他实质性的工作。读写数据，接受新的连接，以及处理客户请求均在工作线程中完成。

使用同步 I/O（以 epoll_wait 为例）实现的 Reactor 模式的工作流程是：
1. 主线程往 epoll 内核事件表中注册 socket 上的读就绪事件。
2. 主线程调用 epoll_wait 等待 socket 上有数据可读。
3. 当 socket 上有数据可读时， epoll_wait 通知主线程。主线程则将 socket 可读事件放入请求队列。
4. 睡眠在请求队列上的某个工作线程被唤醒，它从 socket 读取数据，并处理客户请求，然后往 epoll内核事件表中注册该 socket 上的写就绪事件。
5. 当主线程调用 epoll_wait 等待 socket 可写。
6. 当 socket 可写时，epoll_wait 通知主线程。主线程将 socket 可写事件放入请求队列。
7. 睡眠在请求队列上的某个工作线程被唤醒，它往 socket 上写入服务器处理客户请求的结果。
  

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245057.png)



### Proactor模式
> Proactor 模式将所有 I/O 操作都交给主线程和内核来处理（进行读、写），工作线程仅仅负责业务逻
辑。使用异步 I/O 模型（以 aio_read 和 aio_write 为例）实现的 Proactor 模式的工作流程是：


1. 主线程调用 aio_read 函数向内核注册 socket 上的读完成事件，并告诉内核用户读缓冲区的位置，
以及读操作完成时如何通知应用程序（这里以信号为例）。
2. 主线程继续处理其他逻辑。
3. 当 socket 上的数据被读入用户缓冲区后，内核将向应用程序发送一个信号，以通知应用程序数据已经可用。
4. 应用程序预先定义好的信号处理函数选择一个工作线程来处理客户请求。工作线程处理完客户请求后，调用 aio_write 函数向内核注册 socket 上的写完成事件，并告诉内核用户写缓冲区的位置，以及写操作完成时如何通知应用程序。
5. 主线程继续处理其他逻辑。
6. 当用户缓冲区的数据被写入 socket 之后，内核将向应用程序发送一个信号，以通知应用程序数据已经发送完毕。
7. 应用程序预先定义好的信号处理函数选择一个工作线程来做善后处理，比如决定是否关闭 socket。
  

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245058.png)


### 模拟 Proactor 模式

> 使用同步 I/O 方式模拟出 Proactor 模式。原理是：主线程执行数据读写操作，读写完成之后，主线程向工作线程通知这一”完成事件“。那么从工作线程的角度来看，它们就直接获得了数据读写的结果，接下来要做的只是对读写的结果进行逻辑处理。

使用同步 I/O 模型（以 epoll_wait为例）模拟出的 Proactor 模式的工作流程如下：
1. 主线程往 epoll 内核事件表中注册 socket 上的读就绪事件。
2. 主线程调用 epoll_wait 等待 socket 上有数据可读。
3. 当 socket 上有数据可读时，epoll_wait 通知主线程。主线程从 socket 循环读取数据，直到没有更多数据可读，然后将读取到的数据封装成一个请求对象并插入请求队列。
4. 睡眠在请求队列上的某个工作线程被唤醒，它获得请求对象并处理客户请求，然后往 epoll 内核事件表中注册 socket 上的写就绪事件。
5. 主线程调用 epoll_wait 等待 socket 可写。
6. 当 socket 可写时，epoll_wait 通知主线程。主线程往 socket 上写入服务器处理客户请求的结果。
  

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245059.png)

### 线程池

> 线程池是由服务器预先创建的一组子线程，线程池中的线程数量应该和 CPU 数量差不多。线程池中的所有子线程都运行着相同的代码。当有新的任务到来时，主线程将通过某种方式选择线程池中的某一个子线程来为之服务。相比与动态的创建子线程，选择一个已经存在的子线程的代价显然要小得多。至于主线程选择哪个子线程来为新任务服务，则有多种方式：

- 主线程使用某种算法来主动选择子线程。最简单、最常用的算法是随机算法和 Round Robin（轮流选取）算法，但更优秀、更智能的算法将使任务在各个工作线程中更均匀地分配，从而减轻服务器的整体压力。

- 主线程和所有子线程通过一个共享的工作队列来同步，子线程都睡眠在该工作队列上。当有新的任务到来时，主线程将任务添加到工作队列中。这将唤醒正在等待任务的子线程，不过只有一个子线程将获得新任务的”接管权“，它可以从工作队列中取出任务并执行之，而其他子线程将继续睡眠在工作队列上。

线程池的一般模型为：

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245060.png)

> 线程池中的线程数量最直接的限制因素是中央处理器(CPU)的处理器(processors/cores)的数量N ：如果你的CPU是4-cores的，对于CPU密集型的任务(如视频剪辑等消耗CPU计算资源的任务)来说，那线程池中的线程数量最好也设置为4（或者+1防止其他因素造成的线程阻塞）；对于IO密集型的任务，一般要多于CPU的核数，因为线程间竞争的不是CPU的计算资源而是IO，IO的处理一般较慢，多于cores数的线程将为CPU争取更多的任务，不至在线程处理IO的过程造成CPU空闲导致资源浪费。

- 空间换时间，浪费服务器的硬件资源，换取运行效率。
- 池是一组资源的集合，这组资源在服务器启动之初就被完全创建好并初始化，这称为静态资源。
- 当服务器进入正式运行阶段，开始处理客户请求的时候，如果它需要相关的资源，可以直接从池中获取，无需动态分配。
- 当服务器处理完一个客户连接后，可以把相关的资源放回池中，无需执行系统调用释放资源。

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245061.png)
![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303082245062.png)
    