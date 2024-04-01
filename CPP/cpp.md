## C++基础

### 基础

#### cerr,clog  

- 以红色显示，cerr数据不缓冲，clog缓冲，cerr写到cout设备，clog写到日志

### 变量和基本类型

#### 数据类型

- char可能有/无符号  
- 用signed char/unsigned char显式指定  
- long ≈int，长度与机器有关  
- 用double而不是float，代价差不多，float精度不够  

#### 声明和定义

- extern，只声明，不定义
- 不可以在函数内定义，可以全局定义
- 不可以重复定义一个变量
- 预处理变量不属于namespace std，由预处理器管理

#### void* 指针

- 可以指向任意对象，可以比较，传递，不能操作对象  

#### 数据类型and类型修饰符

- 数据类型：int，float，etc..  
- 修饰符：指针*，引用&  
- 修饰符修饰单个变量而非一整行所有变量,应该和变量写在一起  
- 应写为 int \*a，而不是int\* a

#### const

- 必须初始化  
- 可以运行时初始化（运行时赋值or编译时初始化（常量赋值，未初始化是错误的

        const int a = read()

#### 常量引用

- 不可以把常量赋给非常量引用  
- 跨类型引用赋值：先类型转换再赋值，中间用到临时量对象

#### 指针引用

- 指向常量的指针：const int \*a or int const\* a，不能通过a修改其指向的对象，底层const  
    > const在类型说明符左右，效果一样  
- 常量指针：int \*const a，不能改变a的指向，a必须初始化指向某个对象，属于顶层const  
const在指针修饰符 \* 左右，效果不一样  
- 顶层const  
  - 对象本身是常量  
- 底层const  
  - 指针所指的对象是常量，而指针本身不是  
- constexpr/编译时确定
  - 常量表达式，本身是常量，而且初值也是常量
  - 声明为constexpr，由编译器检查合法性

### 字符串，向量，数组

#### 类型别名

- typedef
- using  new_name = origin_name

#### auto

- auto一般会忽略顶层const，保留底层const

#### 类型指示符 decltype（declared type）

- decltype(f()) 取得f的返回类型
- decltype(a) 取得a的类型
  - 如果a是引用，返回引用类型，否则返回正常类型
- decltype((a)) 双层括号，取得a的引用类型
- decltype(\*a) 解引用，取得a的引用类型

#### 头文件

- 通常包含只能被定义一次的实体（const，类，etc.
- 预处理

        #ifndef                  #ifdef，条件和ifndef相反
         #define
         ....
        #endif

- 函数声明写头文件里，定义写cpp文件里，其他文件使用函数，包含头文件即可

#### using

- using namespace::name 单独名字 / using namespace space_name 空间所有名字
- 避免using namespace std，而是单独using std::cout ...etc
- 头文件不应包含using
- cname头文件属于namespace std，name.h不属于

#### 对象初始化

- 直接初始化：type a(parameters)
- 拷贝初始化：type a = xxx

#### size_type类型

- 不是int也不是unsigned int，是无符号数
- 尽量不要和int混用

#### STL::string

- ```s=s1+s2``` 合法，```s=s1+"str"``` 合法，```s="str1" + "str2"``` 不合法

#### 范围for语句

- for(auto c : str)  c是tmp变量
- for(auto &c : str)  c是引用

#### STL::vector

- 列表初始化：```vector<int> a{1,2,3,4}```，直接给出成员的值
- 构造初始化：```vector<int> a(10,1)```，根据对应方法初始化对象
- 只能对已知成员元素用下标访问

#### STL::Iterator

- for循环尽量用!= 而非 <，多数标准库重载了!=，而没有重载 <
- iterator可以读写元素，const_iterator只能读不能写，类似常量指针
- begin：第一个元素，cbegin，第一个元素的const_iterator，rbegin：末尾元素
- 可能改变容量的操作都会使iterator失效（如push_back  

        mid = begin + （end - begin）/2  √
        mid = （begin + end）/2  ×
    > *iterator是指针，不是下标  

#### 数组

- 类型不允许auto
- char数组
  - 字符串赋值初始化时，要预留空字符 '\0' 的位置
- 不存在引用的数组

        int &ref[10]   ×
        int (&ref)[10] √   引用一个数组，该数组有10个成员
        int *p[10]     指针数组
        int (*p)[10] = &arr  数组指针，指向arr，arr有10个元素

- 可用标准库函数 begin（arr）和end（arr）提取指针，位置同vector

#### string

- 运算符 + 允许C风格字符串（“xxxx \0”），但不能两个都是
- C风格字符串不能 cat string，需要转成c_str（返回值是C string的指针）

#### 多维数组

- 可以一字排开初始化多维数组
- 可以只初始化首元素，其他元素默认初始化
- 使用范围for语句[ for (auto item : arr) ]遍历多维数组，除了最内层都必须是引用

### 表达式

#### 运算符

- 可以重载，但运算对象个数，优先级，结合律无法改变

#### 左值右值

- 左值：使用的是对象的身份（地址）
- 右值：使用的是对象的值（内容）
- decltype作用于左值，得到引用；作用于右值，得到普通类型

#### 求值顺序

- &&、|| 先左后右，短路求值
- ? : 正则  ，先求条件，为真返回第一个，假返回第二个
- ,  逗号，从左到右

#### 赋值顺序

- 从右到左
- a = b , 赋值式子 返回 a
- 赋值优先级低，注意加括号

#### 递增递减

- 不要用后置，返回副本有开销
- *iter++  等价于  (*iter)++
  - 如果一条语句出现同一个对象多次，不要对这个对象做++/--运算
    > 运算顺序是未定义的，对象可按任意顺序求值
    >
#### 成员访问  ->

- 对象左值，得到左值（变量
- 对象右值，得到右值（const

#### 条件运算符   ?

- 两个结果都为左值，返回左值，否则右值
- 优先级很低，最好加括号

#### 位运算符

- 会把数字当二进制处理，但如何处理符号位没有规定/依赖于机器

#### sizeof

- 可以作用于空指针，和解引用空指针（不安全）
- sizeof arr，得到整个数组的大小

#### 逗号

- a,b  先求a，再求b，丢弃a，返回b
- 真正结果是逗号右边的值

### 语句

#### if else

- else与最近的if匹配，不管缩进
- 想控制匹配，要使用花括号

#### switch case

- 多个case可以写在同一行
- 要写default
- 在switch大括号内，只能声明，不能初始化（显/隐都不行）

#### try catch

- 如果没有匹配的catch，就会执行terminate，一般会导致程序非正常退出

### 函数

#### 形参实参

- 没有规定求值顺序（带有++等，可能会产生未定义结果
- 如果可以隐式类型转换，参数类型可以不匹配

#### 局部静态对象

- 第一次经过该语句时创建，程序终止时销毁

#### 分离式编译

- 分别编译.cpp文件，生成.o文件
- 链接.o文件，生成可执行文件

#### 参数传递（约等于新声明一个变量然后赋值

- 引用传递/指针传递，本质是同一个对象
- 最好用引用代替指针
- 如果函数不改变形参的值，最好声明成常量引用
- 非const和const都可以赋值给const
- const不可以赋值给非const
- 可以用引用参数返回多个对象（return一个，其他的修改引用对象
- 值传递，要进行拷贝，是两个对象
- 传数组
- ```int [n]```，期望有n个元素，但实际不一定
- 数组引用，int [&arr](10) ，必须加括号，否则意思是10个引用的数组
- 数组声明只能省略最高一维

#### main处理命令行参数

- argv0是程序名字，参数从1开始处理

#### 可变形参

- initializer_list<>（模板）
  - 参数个数不定，类型相同
  - 使用iterator，begin、end

#### 返回值

- 也可以隐式类型转换
- 会创建一个临时变量（涉及拷贝）
- 不要返回局部对象的引用or指针（会被释放掉），只能返回值
- 返回引用是返回左值，其他是右值
- 列表初始化返回值
  - 用  { xx,xx }返回结果，就像初始化时一样
  - 如果是内置类型（int,char,etc)，括号里最多一个值（等于没有括号）

#### 函数重载

- 不允许参数列表一样，返回值不同的重载
- 参数是否常量，可以重载
- 函数选择：const只能选const，非const优先非const

> 可能有二义性错误（都不完全匹配，但是可以隐式类型转换），会报错

#### 默认实参

- 一个参数有默认值，后面所有参数必须都有
- 默认实参变化，不能作为重载函数的依据

#### inline

- 用于优化 规模小，流程直接，频繁调用的函数
- 只是对编译器的请求，是否执行不一定

#### assert（预处理宏

- 如果表达式为假，输出信息并终止执行

#### 函数匹配

- 最佳匹配（精确匹配优先
- 多个参数匹配

> 如果没有最优函数，报二义性错误

#### 函数指针

- 声明  
    ```type (*pf)(para list)```

- 赋值

    ```pf=function```   or  ```pf=&function```（等价）

- 使用

    ```pf(para)``` or ```(*pf)(para)``` （等价）
- 重载函数的函数指针
    > 必须有精确匹配的函数，否则错误

#### 函数指针形参（容器的cmp

- ```void function ( param ..., pf(param) )``` or ```void function ( param ..., (*pf)(param) )```
    > 用pf 或者（*pf） 等价，前者会自动转换成指针

  作用：作为返回值

### 类

#### basis

- 定义在类内的函数是隐式的inline
- 成员要在类内声明，但成员函数可以在类外实现

#### this

- 可以不用this，默认认为是访问当前对象的对应名字的变量
- 用this显式指定当前对象
- this是常量指针不允许修改this

#### const成员函数

    int func(params) const{
        xxx
    }

- onst作用于this指针，即不允许通过this指针修改成员的值
- const对象只能调用const成员函数

#### 类外成员函数

- 要用域作用符  ::  

#### 构造函数

- 不能为const，初始化const对象时可以往里写值

#### 默认构造函数

- 不写会默认生成
- 有初始值用初始值，没有初始值用默认初始值
- 定义了构造函数之后，编译器不会生成默认构造函数
- 可以写作 ```ClassName() = default;```
- 如果有其他构造函数，要习惯性写默认初始化 [上一行的内容]

#### 列表初始化

    ClassName() : member(val){}

- 初始值列表不限定初始化顺序，初始化顺序与类内声明顺序相同
- 初始化顺序最好与声明顺序一致
- 初始化时，对象已经创建，可以用this指针
- 必须初始化const和引用，因为后续不能赋值
- 而且不能用赋值方式，只能用括号初始化方式，进入构造函数之后，初始化已经完成，剩下的都是赋值操作
    > more，在构造函数内的赋值，是先初始化，再赋值，效率不如直接初始化高

#### 委托构造函数

- 把构造函数的任务委托给其他函数，方法是在初始化列表里调用其他函数
- 执行顺序：委托者列表->收委托者列表->受委托者函数体->委托者函数体
- ```class_name var()```，括号带参可以执行有参初始化，不带参会有二义性（默认初始化or函数，函数返回值是class_name类型

#### 拷贝和赋值

- 拷贝：```int a=10 ; int b=a ;```
- 赋值：```int a=10 , b ; b=a;```

#### 拷贝、赋值、析构

- 和构造函数一样，编译器会生成默认版本，但不一定可靠
- 只能有一个，不需要重载

#### 友元

- friend func   or    friend some_class
- 友元函数声明不等于函数声明，函数要另外声明
- 友元函数最好跟类放在同一个头文件
- 友元函数可以实现在类的内部，隐式inline
- 友元函数只针对某个函数+参数列表，不针对所有重载的同名函数

#### 可变数据成员

- 关键字mutable，即使对象是cosnt，mutable成员也是可变的

        mutable int a

#### 返回 *this的函数

- 返回是左值，可以连续调用
- 如果返回的是副本，则为右值，不可以连续调用
- const成员函数的this指针是const，返回this指针后也不可以连续调用非cosnt成员函数 or 修改

#### 成员函数基于const重载

- 一般来说，const对象调const版本，非const调非const版本
- 写两个版本，可以避免返回const指针，而调用非const函数

#### 类的声明和定义

- 名字出现过 / 即声明 ，之后可以使用类的引用或指针
- 类在被定义之前，不可以初始化类对象（编译器无法确定类的大小/所占空间，以及访问其成员

#### 声明-使用顺序-作用域

- 包括普通变量/函数的先声明后使用
- 友元要自身先被声明，才能被声明为友元
- 友元函数使用的对象要先被定义，才能被使用
- 不能只在类内声明和定义友元函数（作用域被限定在类内）还要在类外声明（要保证先声明后使用
- 如果在类外定义，返回值是类的成员，要加域限定符

#### 名字查找

- 大括号由内向外（内层同名会隐蔽外层变量
- 类先处理所有声明，再处理成员函数的定义，写的顺序不重要
  - 所以函数优先使用类内定义的变量，即使变量声明在函数之后
- 仅限于函数体中使用的名字，不包括参数和返回值
- 尽量避免同名，内外层同名可能混淆
- 如果形参名字和类成员名字重名，默认使用形参，可以用this指针指定使用成员

#### 类类型转换

- 条件

   1. 类B的一个构造函数只有一个参数
   2. 有一个其他类型的实参A
   3. 实参A可以用于传给构造函数
   4. 其他函数的实参类型是这个类B
- 编译器会自动用A初始化一个对象B，然后把临时对象传入函数
- 只允许一步类型转换
  - 不可以传入一个字符串，期望它转成A类型，再用A类型初始化类B（2步转换了
- 不总是有效
  - 过程会执行，结果不一定符合逻辑

#### 抑制隐式类型转换：关键字explicit

- 被限制后，使用显式转换即可，即手动初始化一个临时对象
- 或者，static_cast ，显式转换
- 标准库中的explicit
  1. string(char*) 不是explicit
  1. vector<>(int n) 是explicit

#### 静态成员的声明/定义/使用

- 不绑定对象
- 不包含this指针
- static函数不能使用this指针
- 用域运算符访问静态成员和函数

      cout<<class::static_member<<endl;
      cout<<class::static_member_func()<<endl;

- 类外定义静态函数不重复static关键字

      class test{
          static void func();
      }
      void test::func(){}

- 不能在类内定义静态数据成员
    > 区分声明和定义
- 初始化类的静态成员，可以调类的私有函数(甚至不用加域运算符)

      int class::static_member = 10;
      int class::static_member = init()

- 定义静态成员函数

      int class::func(){}

- 静态成员类内初始化
  - 除非有const值

      class test{
          static int a = 30;     合法
      }

## C++标准库

### IO库

#### IO类

- 分类
  - iostream
    - 从/向流读写数据
    > cin就是一个istream对象
  - fstream
    - 从/向文件读写数据
  - sstream
    - 从/向string读写数据  
- IO对象不能拷贝或者赋值
- 流的状态
  - 状态不正常时，后续所有操作都会失败
  - ```>>``` 和 ```<<```如果正常，返回流的引用
    - ```if```和```while```可以判断cin，如果状态正常，是```true```，如果状态不正常是```false```
    - ```<<```除了可以跟```endl```，还可以跟```flush（刷新缓冲区）```，```ends（输出一个空字符，然后刷新缓冲区）```

#### 文件IO

- C++文件读写
  - 打开

        fstream fstm;
        fstream fstm(file_path[,mode]);   //[]为可选
        fstm.open(file_path[,mode]);
        fstm.close();
        fstm.is_open();  //查询状态

        fstm >> i;
        or
        fstm << i

  - mode
    - 要用域作用符::
    - eg. ```fstream::in```
      - in      读
      - out     写
      - app  append
      - ate  打开然后定位到文件末尾
      - trunc  截断文件
      - binary  二进制方式IO

#### string流

- istringstream 从string读取数据
- ostringstream 向string写数据
- ```sstm.str()```返回sstm保存的string
- ```sstm >> i``` or ```sstm << str```都是可以的，stringstream继承了iostream

## 顺序容器

- 概述
  - array
    - 更安全的数组类型
  - forward_list
    - 期望与最好的手写单向链表表现相当
- 选择
  - 如果必须在中间位置插入元素，可以考虑list，并且完成输入阶段后转成vector
- insert和emplace
  - insert是构造-拷贝构造
    - push_back 同insert
  - emplace是直接初始化构造
    - 用emplace代替push和insert
    - 用emplace_back 代替 push_back
- iterator
  - 不能用```<```和```>```，只能判断 ```!=```
  - cbegin
    - 获取const iterator（常量指针），如果不修改数据，用这个
  - rbegin
    - 做```++```操作会得到上一个元素
  - 向vector、string、deque插入元素会导致所有iterator失效
- 容器初始化
  - 默认构造 ```C c;```
  - 拷贝构造 ```C c1(c2)``` or ```C c1 = c2 ;```
  - 列表初始化 ```C c{values}``` or ```C c = {values}```
  - 用两个迭代器 ```C c(begin,end)``` 指定拷贝范围
- 赋值
  - ```c1 = c2``` 做的是拷贝操作
- swap
  - ```swap(c1,c2)```
    - 交换的是两个容器内部的数据结构指针，会很快（除了array
- vector扩容
  - 虽然是申请新空间然后赋值，但是一般比list和deque都快
  - ```reserve()``` 空间不够则申请新的，空间够则不作操作
- string操作
  - 搜索 
    - ```str.find()``` 返回首次出现的下标
    - 大小写敏感
  - 比较
  - ```str.compare()``` 类似C的strcmp()
  - 数值转换
    - ```to_string()``` 
      - 转成字符串，参数可以是任意类型
    - ```stoi()```     int
    - ```stoll()```  long long
    - ```stoull()``` unsigned long long
    - ```stof()``` float
    - ```stod()``` double
    - ```stold()``` long double
- 适配器adapter
  - 接受一个容器类型，并让其行为像另一种类型
    - 约等于逻辑结构，可以在不同物理结构上实现，完成一些功能。而vector，dequeue等，约等于物理结构
    - stack , queue , priority_queue
  - 定义

        stack<int> stk();      //创建一个空的
        stack<int> stk(deq);   //将deq元素拷贝到stk
        stack<int,vector<int>> stk;  //显式指定基于vector实现

## 泛型算法

- 关键
  - 泛型算法不会执行容器的操作，只会运行于iterator之上，执行iterator的操作
  > 不执行容器操作，所以不会改变容器大小，也不会使iterator失效
- 只读算法
  - ```find()``` ```accumulate()``` 等
  - 最好传```cbegin()``` ```cend()```
  - 不检查写操作
    - 如单iterator算法，不检查容器是否为空
    - 要先确保容器不为空再调用
  - back_inserter

        vector<int> vec;  //空容器
        auto ite = back_inserter(vec); //插入迭代器
        *ite = 42; //向插入迭代器赋值，会在vec后插入一个元素

  - unique

        auto unq_ite = unique(vec.begin(),vec.end());
        //返回一个iterator，指向不重复序列尾后元素，从unq_ite到end()是重复的元素
        //然后erase 或者 resize

  - sort()的comp()函数
    - 如果是类成员，要写成static
    - 传参尽量用引用
    - 传comp不带括号（传的是函数指针
- lambda表达式
  - 尾置返回类型（返回类型在参数列表后面 ```-> return_type```
  - 返回值类型可以推断
        
        auto f = 
        [sz](string a,string b){
        //[]是捕获列表，使用当前大括号内的局部变量
        //必须明确指明才能使用
            return a.size()<b.size();  
            or return sz.size()<b.size();  
            //两个都合法
        };

        auto lambda = 
        [](int x, int y) -> double 
        { return x + y; };
        //捕获列表为空，传入xy，显式指定返回类型为double
        //前面的auto可以写成别的，但很麻烦，写auto即可

  - 值捕获和引用捕获
    - 捕获相当于新建一个同名临时变量，然后传参赋值
    - 如果是临时变量，就是值拷贝，后续修改不会影响lambda
    - 如果是引用，后续修改会影响lambda读到的值
    > 保持捕获简单化
  - 隐式捕获
    - 让编译器自己对应变量名，如果参数列表和大括号没声明，则去表达式所在大括号找
    - [=]和[&] 分别是值捕获和引用捕获
  - 可变lambda

        auto f =
        [v1]() mutable {
            ++v1;
        };
        //显式说明mutable，就可以修改捕获的变量了
        //如果传值，修改的是副本
        //如果传引用，修改的是变量本身
- bind
  - 作用：函数适配器
  - 可以接收无限参数，但是只处理定义过的参数，即用占位符 ```_1 , _2```定义的参数

        auto g = bind(f,a,b,_2,c,_1);
        /*
        g(p1,p2);
        等价于
        f(a,b,p2,c,p1);
        */
  - placeholders
    - _n定义在namespace placeholder中
    - 要么```using namespace placeholders```，要么```placeholders::_1```
  - 用来调换参数顺序
    - ```auto g = bind(f,_2,_1)``` 
    - ```g(a,b)``` = ```f(b,a)```
    - 用这种例子可以轻松把comp的方向调转，而不用写两个函数
  - bind使用引用
    - 库函数ref()
    - ```auto f = bind(print,ref(os),_1)```
    > 这样print才能拿到os的引用
- 迭代器
  - 插入迭代器
    - back_inserter  -> push_back
    - front_inserter -> push_front
    - inserter -> insert
      - 容器必须有对应的操作，才可以创建对应迭代器