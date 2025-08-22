# \<thread\>
### 线程创建
- std::thread\(Callable Object, paras\)
- Callable Object 可以是函数、lambda表达式、函数对象、std::function对象、***类成员函数指针？***等

### 比较两种创建线程的方法：pthread_create & thread 对象
- pthread_create 接收两个主要参数：pthread_t* 的线程id 和 StartRoutine，StartRoutine 接收的参数只能是 void*
- pthread_create 创建出来的线程只能接收一个 void* 指针作为参数，可能有类型不安全问题
- thread对象应用了RAII机制，资源管理更安全 -> ***thread对象有什么资源要释放的？***
- pthread_create 更底层，效率更快（但可能也快不到哪里去）；但是 thread对象的开发效率更高（可读性更好）、更安全
- thread 对象的写法可以跨平台移植

### thread库相关的接口
- std::thread::join
- std::thread::detach
- std::thread::joinable
- std::thread::get_id

# \<mutex\>
- 互斥锁：当多个线程访问共享资源时，需要使用互斥锁来防止数据竞争
- 最简单的互斥量
  - std::mutex mtx; // 定义互斥量
  - mtx.lock\(\); // 上锁
  - mtx.unlock\(\); // 解锁
 - 其他类型的锁，引入了RAII机制
  - std::lock_guard // 基本锁
  - std::unique_lock // 允许手动解锁和重新锁定

### 使用作为锁
- sem_init
- sem_wait
- sem_post
