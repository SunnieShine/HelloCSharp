# 第 9 章：面向对象（Object-oriented，OOP）初步

## 面向对象的三大特性简述

* **封装**（Encapsulation）
* **继承**（Inheritance）
* **多态**（Polymorphism）

## **类**（Class）的概念

概念：任何一个实例的“说明书”

## 类的定义

格式：`访问修饰符 class 类名`

## 需要马上接触的访问修饰符

1. `internal`：项目内随意用
2. `public`：出了项目，别的程序也可以用

## 文档注释

1. `<summary>` 内写类想表达什么东西
2. `<remark>` 内写一些额外补充说明的文字
3. `<!-- -->` 内写注释文字

## **实例化**（Instantiation）

格式：`int i = new int()`

使用构造器对数据类型初始化，然后把初始化后的结果直接赋值给变量的这个复杂过程

## 对象的概念

实例化后的变量本身就是一个对象了。广义来说，对象就是一个所有的变量（局部变量、类变量，全部都是对象）。

## 静态方法和实例方法的概念

## 对象调用实例方法，类调用静态方法

## `this` 指针

1. `this` 关键字的概念和基本用法
2. `this` 代替一个对象，所以可以当参数传递到别的函数里用

## 成员

1. **构造器**（Constructor）：对一个对象进行实例化的固定方法
   1. 写法：`[访问修饰符] 类型名(参数表列)`
      1. 访问修饰符默认为 `private`
      2. 构造器的用法和 `new` 关键字调用（实例化）
      3. 构造器在不写的时候默认生成一个无参构造器；但如果写了至少一个构造器时，该类就不会自动生成默认的无参构造器了
      4. 构造器可重载
      5. 构造器可相互调用，但必须是在最开头就被调用，写法：`构造器声明 : this(被调用构造器的参数表列)`
      6. 文档注释 `<param>`：修饰参数，表达参数的含义
      7. 静态构造器：对一些静态字段在初始化的时候用的，不可以手动调用
         1. 写法：`static 类型名()`
         2. 不写访问修饰符，默认为 `private`
      8. 终结器：用于销毁对象时被系统自动调用，不可以手动调用
         1. 写法 `~类型名()`
         2. 不写访问修饰符，默认为 `private`
   2. **复制构造器**（Copy constructor）：将当前类型作为参数传递到构造器里，这个构造器叫复制构造器；但不能直接使用 `this` 接收，因为他俩还是一样的地址指向
2. **字段**（Field）：类似 C 里的成员
   1. 写法：`[访问修饰符] (static)? (readonly|const)? 类型 字段名 (= 数值)?`
   2. 新增的访问修饰符：`private`：只能在当前类内部可以用，访问修饰符默认为 `private`
   3. `readonly` 关键字：运行时常量
   4. `const` 关键字：所有前文提到的内置类型才可以用的关键字；运行时和编译期常量
3. **属性**（Property）：Java 里 `getxxx` 和 `setxxx` 的封装模型
   1. 写法：`[访问修饰符] (static)? 类型 属性名 { get { return ...; } set { ... = value; }}`
   2. 访问修饰符默认为 `private`
   3. `value` 关键字
   4. 用法：赋值是 `Prop = value`，取值是 `variable = Prop`
   5. 只有 `get` 或只有 `set` 的属性（Get-only & Set-only Properties）
   6. 属性的存在意义：提供验证（封装，OOP 第一大特性），可以解释为什么要封装字段
      * 参考回答：https://www.zhihu.com/question/410257728/answer/1375205727
      * 参考回答：https://www.zhihu.com/question/376634472/answer/1061592339
   7. 文档注释 `<value>`：表达 `value` 关键字的含义
4. **方法**（Method）：就是 C 里的**函数**（Function），以及为什么方法不叫函数，而叫方法
   1. 访问修饰符默认为 `private`
   2. 方法的重载
   3. 文档注释 `<returns>`：表达返回值的含义
   4. 类类型当参数传递到函数内
   5. 注意类是引用类型，因此可能为 `null`；为了避免 `null` 引用的时候报错，需要养成好习惯，遇到引用类型传入时就检查是否为 `null`
   6. 为什么叫方法？方法和函数的区别：没有区别，只是换一个名字；但是在一般的术语词汇里，我们叫函数；在 C# 里的普通函数全部都叫方法：例如构造函数（构造器）、析构函数（析构器）、本地函数（C# 7 的术语）、匿名函数（C# 2 的术语）、Lambda 函数（C# 3 的术语）等
5. **索引器**（Indexer）：类似数组的索引
   1. 格式：`[访问修饰符] 类型 this[索引器参数表列]`
   2. 访问修饰符默认为 `private`
   3. 索引器的重载
6. **运算符**（Custom Operator）
   1. 写法：`public static 类型名 operator 运算符(类型1 变量1(, ...)?)`
   2. 注意事项
      1. `operator >>` 和 `operator <<` 的右侧数值（参数 2）需要是 `int` 类型
      2. 部分运算符重载时需要配套重载：
          * `operator ==` 和 `operator !=`
          * `operator >` 和 `operator <`
          * `operator >= ` 和 `operator <=`
          * `operator true` 和 `operator false`（稍后会说）
      3. 所有运算符的所有参数或返回值必须包含当前类类型名
      4. 可以重载的符号（但重载不影响计算的优先级）：
          * `operator +`（一元运算正号、二元运算加号）和 `operator -`（一元运算负号、二元运算减号）
          * `operator *`（二元运算乘号）、`operator / `和 `operator %`
          * `operator ++` 和 `operator --`
          * `operator &`（二元运算位与）、`operator |`、`operator ^` 和 `operator ~`
          * `operator >>` 和 `operator <<`
          * `operator !`
          * `operator >` 和 `operator <`
          * `operator >= `和 `operator <=`
          * `operator ==` 和 `operator !=`
          * `operator true `和 `operator false`
      5. 重载 `operator true` 和 `operator false`
          * `operator true` 执行期翻译为 `a || b <=> op_True(a) ? a : (a | b)`
          * `operator false` 执行期翻译为 `a && b <=> op_False(a) ? a : (a & b)`
      6. 运算符重载
7. **类型转换器**（Custom converter）
   1. 写法：`public static (implicit|explicit) operator 类型名(被转换类型名 变量名)`
   2. 转换模式是从“被转换类型”到“类型名”；注意这两种类型必须有一个是当前类类型
   3. `explicit` 和 `implicit` 的区别：前者是强制转换用，后者是强制转换或者隐式转换都可以用

## 各个成员的命名规则

* 字段用驼峰或者匈牙利，方法、属性、索引器用帕斯卡
* `public` 字段可以用帕斯卡
* 类型转换器、运算符和静态构造器都没有标识符名称

## **嵌套类**（Nested Class）

写法：`访问修饰符 class 类名`

## 访问修饰符

默认是 `private`