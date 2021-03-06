## Rust



Rust 是一种 **预编译静态类型**（*ahead-of-time compiled*）语言，这意味着可以编译程序，并将可执行文件送给其他人，他们甚至不需要安装 Rust 就可以运行，即脱离环境。

编译： rustc main.rs 

运行： ./main.exe



#### 代码风格

函数和变量名使用 *snake case* 规范风格，即字母都小写并用下划线分隔



#### 基础



`main` 函数是一个特殊的函数，即主函数，是Rust程序的入口



`std`:标准库



`io`:输入输出库



std::env 处理环境变量，

args()能获取命令行参数



`&` 表示这个参数是一个 **引用**（*reference*），它允许多处代码访问同一处数据，而无需在内存中多次拷贝。



println!中的{}会夹住合适的值来打印

println!都会写入到标准输出中

eprintln! 会把错误信息写入到标准错误中



二进制项目会提供主要功能，库项目提供依赖



#### 数据类型



变量：let 默认不可变，添加mut使其可变

常量：const 总是不可变，命名约定是单词使用全大写且之间加下划线

隐藏：可以在同一作用域内定义一个与之前变量同名的新变量，使之前的变量被隐藏，常用于改变值的类型



数据类型中的四种标量类型

整型：默认i32,整型溢出在debugger模式会编译报错，在release模式会表现为环，整数除法会向下舍入到最接近的整数

浮点数：默认f64

布尔型：用bool表示

字符型：用单引号声明 `char` 字面量，使用双引号声明字符串字面量



两种复合类型

元组：元素的类型可以不同，可以解构获得值，也可以使用下标索引直接访问

数组：元素的类型必须相同

vector：初步判断应该是实现了动态扩容和动态缩容



#### 函数



Rust 是一门基于表达式的语言，语句不返回值

表达式：函数调用，宏调用，if



#### 控制流



if条件不用加括号，必须是bool值



有三种循环：`loop`、`while` 和 `for`

loop：重复执行代码，直到要求停止

在嵌套循环中，`break` 和 `continue` 应用于此时最内层的循环，或者使用循环标签，这样就会被作用于被标签的循环

break可以返回值，和return一样



while：和其他语言一样



for：用来遍历集合，for...in...

这样看，rust中的几个循环结构和js中还是有较大差异的



#### 所有权



string的数据被存储在堆空间中，赋值拷贝时只拷贝其指针、长度和容量，被拷贝后原变量会无效，这样一个操作被称为 **移动**，这种方式还隐含了一个设计选择：Rust 永远也不会自动创建数据的 “深拷贝”。因此，任何 **自动** 的复制可以被认为对运行时性能影响较小。

rust还提供了一个叫做 `clone` 的通用函数，用来拷贝堆上的数据

drop：释放占用的内存

看了所有权这章后感觉这种设计真是太棒了，所有变量都可以清晰的被追踪



& 符号就是 **引用**，它们允许你使用值但不获取其所有权，**引用**（*reference*）像一个指针，因为它是一个地址，但与指针不同，引用确保指向某个特定类型的有效值，而不是像指针一样访问到不被预料到的值。

创建一个引用的行为称为 **借用**，无法通过引用修改原始值，但可以创建一个可变引用，可以修改可变引用来修改原始值，可以变量同一时间只能存在一个可变引用， **也** 不能在拥有不可变引用的同时拥有可变引用，但拥有多个不可变引用是可以的

Rust通过这种方式避免数据竞争，再次感慨，Rust太棒了！

悬垂指针是其指向的内存可能已经被分配给其它持有者，rust确保引用永远也不会变成悬垂状态



**字符串 slice**（*string slice*）是 `String` 中一部分值的引用，采用range语法，`starting_index` 是 slice 的第一个位置，`ending_index` 则是 slice 最后一个位置的后一个值

字符串字面值就是 slice，这也就是为什么字符串字面值是不可变的；`&str` 是一个不可变引用。



#### 结构体



struct的**字段初始化简写语法**有点类似js中对象的属性简写

**结构体更新语法**：就是把上个实例的部分值直接赋值给当前实例嘛，但如果更新中发生了移动，上个实例就无法再被使用了



**元组结构体**：用来给元组取名

每一个结构体有其自己的类型，即使结构体中的字段有着相同的类型



`dbg!` 宏接收一个表达式的所有权，打印出代码中调用 dbg! 宏时所在的文件和行号，以及该表达式的结果值，并返回该值的所有权



方法与函数是不同的，因为它们在结构体的上下文中被定义（或者是枚举或 trait 对象的上下文），并且它们第一个参数总是 `self`，它代表调用该方法的结构体实例。



不是方法的关联函数经常被用作返回一个结构体新实例的构造函数

在 `impl` 块中，可以定义方法和函数



#### 枚举



枚举允许你通过列举可能的 **成员**（*variants*） 来定义一个类型，以此提供一个更宽泛的结构

在rust中，**空值**（*Null* ）是一个值，它代表没有值

match必须是穷尽的，对于未匹配的值，可以使用通配模式来解决

`if let` 是 `match` 的一个语法糖，它当值匹配某一模式时执行代码而忽略所有其他值。



#### 包，crate和模块



模块，是以 `mod` 关键字为起始，在模块内还可以定义其他的模块

路径有两种形式：

- **绝对路径**（*absolute path*）从 crate 根开始，以 crate 名或者字面值 `crate` 开头。
- **相对路径**（*relative path*）从当前模块开始，以 `self`、`super` 或当前模块的标识符开头。

绝对路径和相对路径都后跟一个或多个由双冒号（`::`）分割的标识符

要倾向于使用绝对路径，因为把代码定义和项调用各自独立地移动是常见的。

Rust 中默认所有项（函数、方法、结构体、枚举、模块和常量）都是私有的。父模块中的项不能使用子模块中的私有项，但是子模块中的项可以使用他们父模块中的项。

用 `super` 开头来构建从父模块开始的相对路径

在一个结构体定义的前面使用了 `pub` ，这个结构体会变成公有的，但是这个结构体的字段仍然是私有的。

将枚举设为公有，则它的所有成员都将变为公有。

在作用域中增加 `use` 和路径类似于在文件系统中创建软连接

使用 `as` 指定一个新的本地名称或者别名，就像js中import的as那样

 “*重导出*（*re-exporting*）”：将 `pub` 和 `use` 合起来使用

嵌套路径可以消除大量的 use 行

 glob 运算符可以将所有的公有定义引入作用域，要谨慎使用

将模块移动到同名文件中，并在库文件中声明，就是模块拆分



#### 集合



*vector* 允许我们一个挨着一个地储存一系列数量可变的值，感觉就是数组，单行矩阵哈哈哈哈哈哈？？？？

`&` 和 `[]` 返回一个引用，当引用一个不存在的元素时程序会发生报错

用 `get` 方法以索引作为参数来返回一个 `Option<&T>`，如果不存在则返回None



*字符串*（*string*）是字符的集合。

字符串的两种创建方法：'aaa'.to_string()和String::from('aaa')

 `format!` 宏，返回一个带有结果内容的 `String`，不会获取任何参数的所有权

 Rust 不允许使用索引获取 `String` 字符

有一说一，rust的string设计的着实复杂，不过莫名觉得合理哈哈



**哈希 map**（*hash map*）允许我们将值与一个特定的键（key）相关联。这是一个叫做 *map* 的更通用的数据结构的特定实现。

哈希 map 是同质的：所有的键必须是相同类型，值也必须都是相同类型。

`zip` 方法可以创建一个元组的迭代器

`Entry` 的 `or_insert` 方法在键对应的值存在时就返回这个值的可变引用，如果不存在则将参数作为新值插入并返回新值的可变引用

`HashMap` 默认使用一种叫做 SipHash 的哈希函数，它可以抵御涉及哈希表的DoS攻击



#### 错误处理



Rust 将错误分为两大类：**可恢复的**和 **不可恢复的**错误。

 `panic!`宏。当执行这个宏时，程序会打印出一个错误信息，展开并清理栈数据，然后接着退出

panic = 'abort'，直接终止，由操作系统来清理内存

*backtrace* 是一个执行到目前位置所有被调用的函数的列表

`unwrap`：打开，如果 `Result` 值是成员 `Ok`，`unwrap` 会返回 `Ok` 中的值。如果 `Result` 是成员 `Err`，`unwrap` 会为我们调用 `panic!`

`expect`:在`unwarp`基础上自己指定错误信息

**传播**（*propagating*）错误：让调用者知道这个错误并决定该如何处理，简写：`?` 运算符，只能被用于返回值与 `?` 作用的值相兼容的函数，`?` 也可用于 `Option<T>` 值

对任何错误场景都调用 `panic!`，不管是否有可能恢复，这样就是代替调用者决定了这是不可恢复的。选择返回 `Result` 值的话，就将选择权交给了调用者，而不是代替他们做出决定



#### 泛型、trait 和生命周期



泛型是具体类型或其他属性的抽象替代

 **单态化**（*monomorphization*）：泛型在编译时会被展开



trait 定义是一种将方法签名组合起来的方法，目的是定义一个实现某些目的所必需的行为的集合。

不能为外部类型实现外部 trait

**孤儿规则**：其得名于不存在父类型。这条规则确保了其他人编写的代码不会破坏你代码，反之亦然

trait 作为参数使用时，有`impl Trait` 语法和*trait bound*语法，`impl Trait` 语法实质上是*trait bound*语法的一种较长的语法糖，猜测是为了表现上更直观，但*trait bound*语法应该是有更强的表达能力

返回时只能返回某个实现 trait 的类型，而不能多个

对任何满足特定 trait bound 的类型实现 trait 被称为 *blanket implementations*



Rust 中的每一个引用都有其 **生命周期**（*lifetime*），也就是引用保持有效的作用域

**借用检查器**（*borrow checker*）：比较作用域来确保所有的借用都是有效的

生命周期注解：用来显式表达生命周期的存活时间，且有多个时会指向最短那个

指定生命周期参数的正确方式依赖函数实现的具体功能

生命周期语法是用于将函数的多个参数与其返回值的生命周期进行关联的。一旦他们形成了某种关联，Rust 就有了足够的信息来允许内存安全的操作并阻止会产生悬垂指针亦或是违反内存安全的行为。

编码进 Rust 引用分析的模式被称为 **生命周期省略规则**（*lifetime elision rules*）

函数或方法的参数的生命周期被称为 **输入生命周期**（*input lifetimes*），而返回值的生命周期被称为 **输出生命周期**（*output lifetimes*）。

编译器采用三条规则来判断引用何时不需要明确的注解。

第一条规则是每一个是引用的参数都有它自己的生命周期参数。

第二条规则是如果只有一个输入生命周期参数，那么它被赋予所有输出生命周期参数

第三条规则是如果方法有多个输入生命周期参数并且其中一个参数是 `&self` 或 `&mut self`，说明是个对象的方法(method), 那么所有输出生命周期参数被赋予 `self` 的生命周期

`'static`，其生命周期**能够**存活于整个程序期间



#### 自动化测试



测试函数用来验证非测试代码是否按照期望的方式运行

测试函数体通常执行如下三种操作：

1. 设置任何所需的数据或状态
2. 运行需要测试的代码
3. 断言其结果是我们所期望的

`#[test]`：这个属性表明这是一个测试函数

 `assert!` 宏提供一个求值为布尔值的参数。如果值是 `true`，`assert!` 什么也不做，同时测试会通过。如果值为 `false`，`assert!` 调用 `panic!` 宏，这会导致测试失败。

 `assert_eq!`：是否相等

`assert_ne!`：是否不相等

`should_panic`：检查代码是否按照期望处理错误



运行多个测试时， Rust 默认使用线程来并行运行

`--test-threads`：更加精确的控制线程的数量

 `--show-output` ：告诉 Rust 显示成功测试的输出

通过指定名字可以运行部分测试

`#[ignore]`：标记要排除的测试

`-- --ignored`：只运行被排除的测试



单元测试与他们要测试的代码共同存放在位于 *src* 目录下相同的文件中。规范是在每个文件中创建包含测试函数的 `tests` 模块，并使用 `cfg(test)` 标注模块。

 `#[cfg(test)]` 注解告诉 Rust 只在执行 `cargo test` 时才编译和运行测试代码

`cfg` 属性代表 *configuration* 

集成测试的目的是测试库的多个部分能否一起正常工作，需要在项目根目录创建一个 *tests* 目录，与 *src* 同级



#### 迭代器与闭包



- **闭包**（*Closures*），一个可以储存在变量里的类似函数的结构

**闭包**（*closures*）是可以保存在一个变量中或作为参数传递给其他函数的匿名函数

*惰性求值*：只在需要结果时执行闭包，并缓存结果值

闭包可以通过三种方式捕获其环境：FnOnce，FnMut和Fn

- **迭代器**（*Iterators*），一种处理元素序列的方式
- 迭代器是 **惰性的**（*lazy*），这意味着在调用方法使用迭代器之前它都不会有效果。

iter：获取的是不可变引用

iter_mut：获取的是可变引用

into_iter：获取的是所有权

调用 `next` 方法的方法被称为 **消费适配器**（*consuming adaptors*），因为调用他们会消耗迭代器

**迭代器适配器**（*iterator adaptors*），他们允许我们将当前迭代器变为不同类型的迭代器。可以链式调用多个迭代器适配器。不过因为所有的迭代器都是惰性的，必须调用一个消费适配器方法以便获取迭代器适配器调用的结果。

啊终于换为我喜欢的函数式风格的代码了~



#### 智能指针



**引用计数** （*reference counting*）智能指针：动态链接？？？？

`Deref` trait 允许智能指针结构体实例表现的像引用一样

`Drop` trait 允许我们自定义当智能指针离开作用域时运行的代码



- `Box<T>`，用于在堆上分配值
- `Rc<T>`，一个引用计数类型，其数据可以有多个所有者
- `Ref<T>` 和 `RefMut<T>`，通过 `RefCell<T>` 访问。（ `RefCell<T>` 是一个在运行时而不是在编译时执行借用规则的类型）。

Rust 在发现类型和 trait 实现满足三种情况时会进行 Deref 强制转换：

- 当 `T: Deref<Target=U>` 时从 `&T` 到 `&U`。
- 当 `T: DerefMut<Target=U>` 时从 `&mut T` 到 `&mut U`。
- 当 `T: Deref<Target=U>` 时从 `&mut T` 到 `&U`。



**std::mem::drop，析构函数** 对应创建实例的 **构造函数**，第一次了解到这个概念，哈哈哈哈哈



`Rc::clone` 只会增加引用计数，这并不会花费多少时间



#### 无畏并发



**并发编程**（*Concurrent programming*），代表程序的不同部分相互独立的执行

 **并行编程**（*parallel programming*）代表程序不同部分于同时执行

`thread::spawn`：新建线程

 `join` 会阻塞当前线程直到 handle 所代表的线程结束 

在闭包之前增加 `move` 关键字，强制闭包获取其使用的值的所有权



“不要通过共享内存来通讯；而是通过通讯来共享内存。”

Rust 中一个实现消息传递并发的主要工具是 **信道**（*channel*）

`mpsc::channel` 函数创建信道

**互斥器**（*mutex*）:任意时刻，其只允许一个线程访问某些数据

Send 允许在线程间转移所有权

Sync 允许多线程访问



#### 模式与模式匹配



模式有两种形式：refutable（可反驳的）和 irrefutable（不可反驳的）

**匹配守卫**（*match guard*）是一个指定于 `match` 分支模式之后的额外 `if` 条件，它也必须被满足才能选择此分支。



#### 高级特征



通过 `unsafe` 关键字来切换到不安全 Rust

- 解引用裸指针

- 调用不安全的函数或方法

- 访问或修改可变静态变量

- 实现不安全 trait

- 访问 `union` 的字段

  

关联类型看起来像一个类似泛型的概念

**运算符重载**（*Operator overloading*）是指在特定情况下自定义运算符（比如 `+`）行为的操作。

使用 `type` 关键字来给予现有类型另一个名字

`!`：函数从不返回的时候充当返回值

从不返回的函数被称为 **发散函数**

`fn` 被称为 **函数指针**，允许我们使用函数作为另一个函数的参数



**宏**（*Macro*）指的是 Rust 中一系列的功能：使用 `macro_rules!` 的 **声明**（*Declarative*）宏，和三种 **过程**（*Procedural*）宏：

- 自定义 `#[derive]` 宏在结构体和枚举上指定通过 `derive` 属性添加的代码
- 类属性（Attribute-like）宏定义可用于任意项的自定义属性
- 类函数宏看起来像函数不过作用于作为参数传递的 token



一个函数签名必须声明函数参数个数和类型。相比之下，宏能够接收不同数量的参数

可以在编译器翻译代码前展开

在一个文件里调用宏 **之前** 必须定义它，或将其引入作用域，而函数则可以在任何地方定义和调用。

过程宏有点像是标签，感觉后面应该会经常用到



#### 构建web server



**线程池**（*thread pool*）是一组预先分配的等待或准备处理任务的线程

























