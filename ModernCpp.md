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
 - 直接使用std::mutex有可能会出现忘记解锁等问题，所以引入了其他类型的锁，使用了RAII机制
  - std::lock_guard // 基本锁，当lock_guard对象销毁时，就解锁了
  - std::unique_lock // 允许手动解锁和重新锁定

### 使用semphore作为锁
- sem_init\(sem_t *sem\) -> 初始化信号值
- sem_wait\(sem_t *sem\) -> 如果信号值大于0，那么直接减1；如果信号值等于0，那么就等待直到信号值变成大于0的
  - sem_trywait()
  - sem_timedwait()
- sem_post\(sem_t *sem\) -> 释放信号值，加1
- sem_destroy\(\sem_t *sem)
- 确保原子性：现代CPU指令集提供对应的原子指令

### c++20新引进的
  - counting_semaphore
  - binary_semaphore
 
### 总结
  -  使用POSIX信号量，需要手动管理，容易出错
  -  c++11引入std::mutex和lock_guard，自动释放，类型安全，但是仅支持二元锁
  -  c++20引入 counting_semaphore 和 binary_semaphore

# 条件变量


# lambda表达式
- 格式：\[ capture-list \] \( parameters \) -> return-type \{ body \}
- lambda表达式实际上会编译为一个匿名函数对象
- 捕获列表，值捕获 -> 直接写变量名，引用捕获 -> &变量名
  - 当要捕获成员变量的时候，只要包含了this指针就可以，成员变量无需再显式包含到捕获列表中
- 如果使用 mutable 关键字的话，那么编译器会将 operator\(\) 定义为 const成员函数，那么就无法修改
  - 但是即使没有使用 mutable 关键字，对于引用捕获的值还是可以修改的，因为 const 成员函数只保证不修改成员本身，但是对于成员指向的内容（引用 & 指针），都是可以修改的

### 其他可调用对象
- 函数，本质上不是对象，而是代码块
- 函数指针，指针本身是对象，但是指向的代码块不是对象
- 函数对象（functor），重构了operator\(\)的用户自定义
- lambda表达式，匿名的函数对象
- std::function
- 成员函数指针，需要绑定到对象实例才可以使用

# std::function & std::bind
### std::function
- 头文件，include \<functional\>
- function 数据类型定义，function<返回类型\(形参列表\)>
- 可以调用 !function 来判断 std::function 对象有没有绑定可调用对象

### std::bind
- 头文件，include \<functional\>
- 本质上是生成了一个函数对象，通过定义 operator\(\) 来实现函数逻辑的重构

  
