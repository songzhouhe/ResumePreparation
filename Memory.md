# Dynamic allocation
### new expression 
- new expression 执行了两个动作，申请内存和调用构造函数
- auto *ptr = new type;
- auto *ptr = new type\(\); // invoke constructor
- 动态分配 const 对象，const auto *ptr = new const type\(\);
- 内存耗尽时，调用 new 动态分配空间就会失败，抛出异常（bad_alloc）；但可以通过 auto *ptr = new \(nothrow\) type; 来指定不抛出异常而是返回一个空指针

### delete expression
- 执行两个操作：销毁给定的对象，释放对应的内存

### 动态数组
- new type\[len\] 返回一个type类型指针
- delete \[\] ptr

# Smart Pointer
### shared_ptr 
- 可以通过调用 make_shared\<T\> 或者 在shared_ptr\<T\> 的构造函数中传入一个原始指针的方式来创建 shared_ptr
- shared_ptr 实例管理着一个**引用计数 reference count**，记录着有多少个 shared_ptr 实例指向同一块内存，当计数器变为0，对应内存就会被释放
  - 管理两块空间：用于对象数据保存 & 控制块
- 使用智能指针应当遵循原则：不使用 new 出来的指针，因为智能指针的析构函数中默认会调用 delete 来释放内存，动态空间的申请和释放应该统一交给智能指针来管理
- reset接口，可以重置 shared_ptr 所管理的内置指针，并且指定删除原内置指针时用的删除器
- 如果要使用shared_ptr来管理动态数组的话
  - shared_ptr\<type\[\]\> -> c++17 and above
  - 要使用自定义的删除器 -> before c++17

### unique_ptr
- 与shared_ptr不同，unique_ptr拥有所指向的对象；同一时间，只能由唯一个unique_ptr指向一个给定的对象，所以 unique_ptr 不支持拷贝或者赋值操作
- 创建 unique_ptr 时，要通过构造函数传递进来要管理的内置指针
- 除非是拷贝或者赋值一个即将被销毁的 unique_ptr
- 如果要指定unique_ptr的删除器，要通过模板参数传递
- reset & release
- unique_ptr\<type\[\]\>

### weak_ptr
- 指向一个 shared_ptr 管理的对象，但只跟该对象弱绑定
- 可以通过直接在构造函数中传递 shared_ptr 或者 operator= 的方式给 weak_ptr 指定要观察的 shared_ptr
- 接口
  - weak_ptr\<T\>::reset\(\)
  - weak_ptr\<T\>::use_count\(\)
  - weak_ptr\<T\>::expired\(\)，判断该 weak_ptr 是否指向有效的 shared_ptr
  - weak_ptr\<T\>::lock\(\)，如果指向 valid 的 shared_ptr，则返回该 shared_ptr，否则返回空的 shared_ptr
 
# Others
### 尾后指针
- 指向容器中最后一个元素之后的位置，而不是最后一个元素本身


