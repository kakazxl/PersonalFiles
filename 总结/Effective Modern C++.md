# Effective Modern C++

## 智能指针

### std::unique_ptr

- 用std::unique_ptr管理独占所有权的资源。
- std::unique_ptr只能移动，不能拷贝。

### std::shared_ptr

- 用std::shared_ptr共享所有权来管理生命期。
- std::shared_ptr的大小是原生指针的两倍，因为它包含一个指向资源的原生指针，还有资源的引用计数。
- 通过咨询资源的引用计数，std::shared_ptr的构造函数会增加这个计数，拷贝赋值运算也会。
- 如果一个std::shared_ptr在递减后发现引用计数为0，意味着没有其他的std::shared_ptr指向这资源，所以销毁资源。

#### std::shared_ptr使用注意

- 避免用原生指针构造std::shared_ptr。通常的选择是使用std::make_shared，但在某些例子中，我们需要用自定义删除器，这时没办法使用make_shared。
- 如果你一定要用原生指针构造std::shared_ptr，那么直接把new出来的结果传递过去，而不是传递原生指针变量。
- 如果一定要使用原生指针变量，避免2个std::shared_ptr指向同一个指针变量，会导致2次引用计数，std::shared_ptr变量结束时会对指针调用两次析构函数。

#### std::enable_shared_from_this

通过std::enable_shared_from_this，安全地用this指针创建std::shared_ptr。

### std::weak_ptr

把std::weak_ptr当作类似std::shared_ptr的、可空悬的指针使用。通过std::shared_ptr创建std::weak_ptr，std::weak_ptr指向的地方和std::shared_ptr指向的地方一致，但是不会增加引用计数。

#### 用途

- 和std::shared_ptr配套使用，当多线程环境下，std::shared_ptr可能会在一个线程内被销毁掉，通过std::weak_ptr::lock判断std::shared_ptr对象是否被销毁掉。
- 如果存在两个指针相互指向对方的情况，采用std::shared_ptr会导致两个指针都无法释放，采用std::weak_ptr可以避免这种情况发生。

### 使用std::make_unique和std::make_shared替代new

- 使用new可能造成内存泄露的隐患，考虑到processWidget(std::shared_ptr<Widget>(new Widget),               computePriority());这个函数。编译器生成代码的执行顺序有可能是这样的：
  1. 执行“new Widget”。
  2. 执行computePriority，可能会发生崩溃。
  3. 执行std::shared_ptr的构造函数。
- 使用new会进行两次内存分配，一次是分配对象内存，一次是分配指向控制块内存，而std::make_xxx只需要分配一次内存同时持有对象和控制块。

### std::move和std::forword

- **std::move**表现为无条件的右值转换，就其本身而已，它不会移动任何东西。
- **std::forward**仅当参数被右值绑定时，通过引用折叠，把参数转换为右值。
- **std::move**和**std::forward**在运行时不做任何事情。

### 左值引用和右值引用

- 如果一个模板函数的参数类型是T&&而T是要被推断类型，或者一个对象使用auto&&声明，那么这个参数或者对象是个通用引用。
- 如果类型声明并不精确地具备type&&的形式，或者不发生类型推断，那么type&&指代的是右值引用。
- 如果用右值初始化通用引用，通用引用相当于右值引用；如果用左值初始化通用引用，通用引用相当于左值引用。
