# C# 编程视频教程系列大纲（刘铁猛致敬版）

1. C# 简介

    1. 编译简单 C# 语言程序：
    ```csharp
    internal static class Program
    {
        private static void Main()
        {
            System.Console.WriteLine("Hello, world!");
        }
    }
    ```

    或

    ```csharp
    // C# 9 Syntax.
    System.Console.WriteLine("Hello, world!");
    ```

    2. **关键字**（Keyword）的概念
    3. **标识符**（Identifier）
        1. 概念
        2. 规则
            1. 数字、字母、下划线、`@` 符号和一些自然语言符号的组合
            2. `@` 如果要用，只能放在开头，且只能用一次，用于表示该单词用于表达标识符而不是关键字
            3. 数字不可放在第一个字符
        3. 标识符的命名规范
            1. **驼峰命名法**（camelCase）
            2. **帕斯卡命名法**（PascalCase）
            3. **匈牙利命名法**（type_Case）
            4. 一般方法用帕斯卡，变量用驼峰命名法
    4. 程序代码结构：类声明、方法声明、方法执行
    5. `Main` 方法
    6. **缩进**（Indenting）
    7. **变量**（Variable）的定义和使用
    8. `Console.ReadLine`、`Console.ReadKey`、`Console.Write` 和 `Console.WriteLine`
    9. **注释**（Comment）
        1. 单行注释 `//`
        2. 多行注释 `/* */`
        3. 文档注释 `///`
        4. 文档注释 `/** */`
    10. **公共语言运行时**（Common Language Runtime，CLR）的概念
    11. **命名空间**（Namespace）的概念
         1. 系统命名空间
         2. 自定义项目的命名空间
             1. 命名空间使用小数点对每一个命名空间的范围作为分隔，逐步减小，类似于 `Earth.China.Sichuan.Chengdu`
             2. 可以在同一个文件并排多个命名空间
             3. 可以在同一个文件嵌套多个命名空间，但这些命名空间的名称必须是单个的单词，不能以小数点分隔开
         3. 命名空间限定符：`::` 和 `.`：在命名空间之后跟上类型名称时候需要加上 `::` 或 `.`；C++ 转 C# 的开发人员更喜欢用 `::`，而 C# 初学者或者只学过 C# 的可能更喜欢 `.`；C++ 的 `::` 在 C# 里被削弱了，用得比较少
    12. `using` 指令的使用
    13. 由于语言不断迭代更新，考虑内容较多，所以我们先让大家从 C# 1 开始学起来，将 C# 的基本语法搞完后，最后按照语言版本翻新语法点

2. **数据类型**（Type System）

   1. 基本概念
   2. 类型一览：

      | 类型名    | 大小    | 全称             | 是否有符号 | 有效格式                            |
      | --------- | ------- | ---------------- | ---------- | ----------------------------------- |
      | `sbyte`   | 1 字节  | `System.SByte`   | 是         |                                     |
      | `byte`    | 1 字节  | `System.Byte`    | 否         |                                     |
      | `ushort`  | 2 字节  | `System.UInt16`  | 否         |                                     |
      | `short`   | 2 字节  | `System.Int16`   | 是         |                                     |
      | `uint`    | 4 字节  | `System.UInt32`  | 否         | `0U` 或 `0u`                        |
      | `int`     | 4 字节  | `System.Int32`   | 是         |                                     |
      | `ulong`   | 8 字节  | `System.UInt64`  | 否         | `0UL` 或 `0ul`                      |
      | `long`    | 8 字节  | `System.Int64`   | 是         | `0L` 或 `0l`                        |
      | `float`   | 4 字节  | `System.Single`  | 是         | `0F` 或 `0f`                        |
      | `double`  | 8 字节  | `System.Double`  | 是         | `0D` 或 `0d`<br />`1E+2` 或 `1e+2`  |
      | `decimal` | 16 字节 | `System.Decimal` | 是         | `0M` 或 `0m`                        |
      | `bool`    | 1 字节  | `System.Boolean` | 否         | `true` 或 `false`                   |
      | `char`    | 2 字节  | `System.Char`    | 否         | `'c'`<br />`'\u0067'`<br />`'\x43'` |
      | `string`  |         | `System.String`  | 否         | `"string"`<br />`@"C:\Users"`       |

   4. 字符串的知识点
      1. 字符串的使用
         1. `ToUpper` 和 `ToLower`
         2. `Format`
         3. `Concat` 和 `operator +`
         4. `Compare`
         5. `StartsWith` 和 `EndsWith`
         6. `Trim`、`TrimStart` 和 `TrimEnd`
         7. `Replace`
         8. `string.IsNullOrEmpty` 和 `string.IsNullOrWritespace`
         9. `Length` 属性获取字符串长度
      2. 字符串的**不可变性**（Immutability）（但你可以用指针进行内容的修改）
      3. `string.Empty` 特殊只读量
   5. **值类型**（Value Type）和**引用类型**（Reference Type）
      1. 值类型：轻量级类型，在函数传参的时候会以栈内存赋值克隆副本以增强性能
      2. 引用类型：只能放在堆内存的东西，随时随地都是以地址进行传递和使用
      3. 对应类型的**字面量**（Literal）
         1. 普通数据类型的字面量写法（参考前文的表格）
         2. 布尔字面量
            * 真：`true`
            * 假：`false`
         3. 字符字面量
            1. 普通字符：`'c'`
            2. 十进制表达：`'\u0065'`
            3. 十六进制表达：`'\x43'`
         4. 字符串字面量
            1. **普通字符串**（Original String）：`"C:\\Users\\Sunnie\\Desktop"`
            2. **原义字符串**（Verbatim String）：`@"C:\Users\Sunnie\Desktop"`
         5. `null` 字面量：表示引用类型默认数值；引用类型以地址作为信息元表达，默认为指向 0 号内存的地址作为默认数值；`null` 就等价于此
         6. `default(T)` 字面量：表示某个类型的默认数值，它一定是一个常量：值类型默认为 0，引用类型默认为 `null`
   6. 数据溢出的概念和处理
      1. 溢出的概念
      2. `checked` 和 `unchecked` 块
   7. 类型转换机制
      1. **强制转换**（Explicit Cast）
      2. **隐式转换**（Implicit Cast）
      3. 为什么 C# 不让 `bool` 和 `int` 直接转换？
      4. 复杂数据转换：`System.Convert` 和 `Parse` 方法

3. **运算符**（Operators）

   1. 赋值运算符：`=`
   2. 算术运算符：`+`、`-`、`*`、`/` 和 `%`
   3. 比较运算符：`>`、`>=`、`<`、`<=`、`==` 和 `!=`
   4. 逻辑运算符：`&&`、`||`、`!` 和 `^`
   5. 位运算符：`&`、`|`、`~`、`^`、`>>`、`<<`
   6. 复合赋值运算符：`+=`、`-=`、`*=`、`/=`、`%=`、`&=`、`|=`、`^=`、`>>=` 和 `<<=`
   7. 自增自减运算符：`++`、`--`（前缀、后缀）
   8. 条件运算符：`?:`

4. 程序**流程控制**（Procedure Controlling）

   1. 顺序结构
   2. 条件结构
      1. `if` 语句
      2. `switch` 语句、`goto case` 语句和 `break` 语句
   3. 循环结构
      1. `while` 语句
      2. `do-while` 语句
      3. `for` 语句
      4. `foreach` 语句（`string`）
      5. `break` 和 `continue` 语句
   4. 跳转结构
      1. 标签的概念
      2. `goto` 语句
   5. 异常结构
      1. `try` 块的定义和用法
      2. `catch` 块的定义和用法
      3. `catch` 有参数和无参用法
      4. 多 `catch` 块时，需要先捕获叫具体的异常，后捕获模糊的异常
      5. `finally` 块简介
      6. `throw-new` 和 `throw` 语句

5. **数组**（Array）

   1. 数组是引用类型，在 C# 里可直接赋值
   2. 索引是**从 0 开始的**（0-Based）
   3. 一维数组：`int[] a = { 1, 2, 3 }`
   4. 二维数组：`int[,] a = { { 1, 2 }, { 3, 4 }, { 5, 6 } }`
   5. 高维数组
   6. 数组完整书写格式：`int[] a = new int[3] { 1, 2, 3 }`
   7. 锯齿数组：

      ```csharp
      int[][] a = new int[3]
      {
          new int[1] { 1 },
          new int[2] { 2, 3 },
          new int[3] { 4, 5, 6 }
      };
      ```

   8. 数组不使用初始化器时的默认赋值：`int[] a = new int[6]`
   9. **数组越界**（Array out of range）行为：触发**异常**（Exception）
   10. `Length` 属性获取数组总长度
       1. 一维数组、二维数组等方形数组：总元素数
       2. 锯齿数组：多少个子数组
   11. 数组相关方法
       1. `Array.Sort`
       2. `Array.IndexOf`

6. **方法**（Method）

   1. 什么是方法？
   2. **方法签名**（Signature）的定义
   3. 方法传值和临时变量初始化的细节（体现值类型和引用类型的区别）
   4. **方法重载**（Overload）：参数类型、数量不同构成重载，返回值不构成重载
   5. `Main` 方法的参数
   6. `ref` 参数
   7. `out` 参数
   8. `params` 参数，允许数组拆分为多参数形式（变长参数）
   10. **递归**（Recursion）

7. **指针**（Pointer）

   1. `unsafe` 块介绍
   2. 指针变量的声明格式：`int* p = &v`
   3. 地址符 `&` 和间接地址运算符 `*`
   4. 无类型指针 `void*`
   5. `stackalloc` 关键字分配栈内存数组
   6. 通过指针修改字符串内容
   7. 值传递、地址传递和引用传递的概念，以及**解引用**（Dereference）的概念
   8. `fixed` 关键字固定数组和字符串
   9. **平台调用**（P/Invoke）（结合 C/C++ 函数调用）
      1. `MarshalAsAttribute` 特性
      2. `DllImportAttribute`特性
      3. `IntPtr` 和 `UIntPtr` 托管指针类型
      4. `extern` 关键字
      5. 带有指针的传参模型：`ref` 和 `out` 参数
      6. 变长参数（`_VA_ARGS_`）的传参模型：`__arglist` 参数
      7. **函数指针**（Function Pointer）的传参模型：**委托**（Delegate）和 `UnmanagedFunctionPointerAttribute` 特性

8. **面向对象**（Object-oriented，OOP）入门

   1. 面向对象的三大特性：**封装**（Encapsulation）、**继承**（Inheritance）和**多态**（Polymorphism）简述
   2. **类**（Class）的概念：任何一个实例的“说明书”
   3. 类的定义格式：`访问修饰符 class 类名`
   4. 需要马上接触的访问修饰符
      1. `internal`：项目内随意用
      2. `public`：出了项目，别的程序也可以用
   5. 文档注释
      1. `<summary>` 内写类想表达什么东西
      2. `<remark>` 内写一些额外补充说明的文字
      3. `<!-- -->` 内写注释文字
   6. **实例化**（Instantiation）：`int i = new int()`
   7. 对象的概念：实例化后的变量本身就是一个对象了
   8. 静态方法和实例方法的概念
   9. 对象调用实例方法，类调用静态方法
   10. `this` 指针
      1. `this` 关键字的概念和基本用法
      2. `this` 代替一个对象，所以可以当参数传递到别的函数里用
   11. 成员
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
   12. 各个成员的命名规则：字段用驼峰或者匈牙利，方法、属性、索引器用帕斯卡；`public` 字段可以用帕斯卡；类型转换器、运算符和静态构造器都没有标识符名称
   13. **嵌套类**（Nested Class）：写法：`访问修饰符 class 类名`
   14. 访问修饰符默认是 `private`

6. 类的继承（OOP 第二大特性）

      1. 继承的定义：将类型的操作分离开，把一些最基础的操作单独用**基类型**（Base）表示，而所有类型都从该类型**派生**（Derive）
      2. 基类型（父类型、超类型）：只包含一些基本操作元素的类型
      3. 派生类型（子类型）：从基类型拓展出来的新类型
      4. 继承想表达的含义：is a 模型，如果 `Student` 类型从 `Person` 类型派生，则从生态角度理解为 `Student is a Person`；不要乱用继承关系，否则会出现 `Son is a Father` 的类似无厘头理解
      5. 继承关系可以形成链条达到很多层，不一定只能是一层
      6. 派生书写格式：`类定义 : 基类型名`
      7. 所有类型都从 `object` 派生，`object` 是所有类型的最终基类
         1. `object.ReferenceEquals`
         2. `object` 自带的 `operator ==` 和 `operator !=`
         3. `GetHashCode`
         4. `ToString`
         5. `Equals`：只是简单调用了一下 `ReferenceEquals`
      8. 基类型和派生类型的互相转换：基类型转派生类型（大转小）用强制转换；派生类型转基类型（小转大）用隐式转换；转换错误会产生异常
      9. 在派生类型里使用基类型的成员（赋值、取值、调用等）
      10. `protected` 访问修饰符级别：只能用于当前类和其派生下去的所有类里
          * `protected` 修饰符：只能用于派生关系
          * `protected internal` 修饰符：融合 `protected` 和 `internal` 两大修饰：在项目内随便用，或者对外只能在当前派生关系里可用
      11. 继承的模型：单继承，C# 不允许多继承，因为自然世界没有一个人有多个爸爸的说法
      12. 密封类：`sealed` 关键字：该类不可继续派生子类型
      13. 基类型成员信息的重写
          1. `virtual` 修饰符：表示方法可以被子类型重写，然后子类型就是新的执行逻辑了；
          2. `base` 关键字可调用基类型的执行行为（甚至是构造器）
          3. `override` 修饰符：重写基类型的成员；重写所有方法时都需要加此关键字来保证方法是用来重写的，而不是拿来隐藏的（隐藏请参见下面 `new` 修饰符）
          4. 可重写的成员类型：属性、方法
          5. `new` 修饰符：隐藏基类型的该成员，表示该方法在这个类里应是这样的（你可以认为是两个成员的执行思维和逻辑毫无关联，但只是恰好有了一个完全一样的名字，有时候也可以理解为故意不想要基类型的该同名成员）
          6. `sealed` 方法修饰符：修饰方法不可被派生类重写
      14. **抽象类**（Abstract Class）
          1. `abstract` 关键字介绍
          2. 定义抽象类：`[访问修饰符] abstract class 类型名`
          3. 抽象成员和类必须在继承后立刻实现
          4. 多态的用法（OOP 第三大特性）
             举例：`Shape` 抽象类里实现 `Triangle`、`Circle` 之类的类型，然后实现抽象类给出的 `Area` 只读属性；最后从 `Main` 方法里直接通过 `Shape s = new Triangle()` 的模式实例化后，调用 `s` 的 `Area` 属性获取面积数值；同时 `s` 因为是 `Shape` 类型的，所以 `Triangle` 类型特有的成员可能会看不到，此时就需要强制转换了
          5. 抽象类之间的相互继承，但抽象类不能派生自实例类
          6. 可以抽象 `object` 的自带方法来让派生类型实现，例如 `abstract override ToString()`
          7. 可以在抽象类里写一些已经有执行逻辑的方法，且这些方法可以标记 `sealed` 以防止派生下去的时候又修改（重写）掉执行逻辑
      15. `is` 关键字对类型进行判断和验证
      16. `as` 关键字对类型进行判断和转换
      17. 综合内容：自定义异常类型，继承 `System.Exception`，然后就可以用 `throw` 来抛出你自己定义的异常了

7. 类的实现：**接口**（interface）

      1. 写法：`[访问修饰符] interface 接口名`
      2. 接口命名规范：帕斯卡，且第一个字符必须是 `I`：`IEdible`（可以吃的）、`IRunnable`（可以跑的）等
      3. 接口只能用 `internal` 或 `public` 修饰符修饰，默认是 `internal`
      4. 接口类似抽象类，为一个抽象模型，在类型使用的时候，必须实现该接口给出的所有成员信息
      5. 接口可以给一个类提供具体的实现约束模型，一个类可以实现多个接口，因此每一个接口按照规范都写的是这个一些固定的操作形式，故接口想表达的是一种 can do 的关系，例如 Person can run（Person 实现 `IRunnable` 接口）
      6. 接口的成员声明不能使用访问修饰符进行修饰，因为它们默认都是 `public` 修饰的
      7. 接口的两种实现方式
         1. 隐式实现：用一个类的成员来搞定，只要成员的签名和接口成员匹配就可以了
         2. 显式实现：`返回值类型 接口名.成员名`
         3. 它们的实现机制区别：显式实现表示这个方法不一定非要让别的程序员看到，它只是表达，这个类可以给出这个接口的一种实现方式，但我们有更好或者更优的策略去做，那么这个方法可以不被外界访问（为了安全性），这个时候可以用显式接口实现，避免误用。推荐使用隐式接口实现
         4. 都不需要加 `override` 表达重写
         5. 接口成员和类型的转换（普通类型转接口需要隐式转换，而接口类型转普通类型需要强制转换）；因为一个类型实现的接口可能有多个，所以在调用期间，可能隐式转换会改为显式转换，保证执行的有效性，例如 `int[]` 类型默认实现了 `IEnumerable` 接口类型，当方法同时包含这两种类型的参数时，会优先执行 `int[]` 这个；但 `int[]` 还实现了 `ICollection` 接口类型，如果两种接口类型的方法重载同时出现时，会报错，此时你必须要强制转换）；转换错误会产生异常
         6. 接口的继承
            1. 写法：`接口声明 : 别的接口名`
            2. 接口的继承等效于把别的接口的所有方法拿过来用了，当你要实现的时候，必须要实现这个接口，以及它继承的接口的所有成员
         7. 接口的多态性：接口也可以当成抽象类一样，用一个实例来赋值：`IRunnable r = new Man()`；此时，`r` 变量只能看到 `IRunnable` 里的所有信息（因为它是这个类型的），进而进行调用
      8. 接口对象当参数传递到函数里
      9. 接口是引用类型，因此养成好习惯，检查 `null`

11. 值类型和自定义结构
    1. 值类型的诞生原因：为了加快传参速度；所以值类型一般都很小，尽量不把大的类型定义为结构（一般是 64 字节）；大多数内置类型都是值类型，除了 `string` 和 `object` 是引用类型
    2. 值类型传参是复制内容；包含指针则只复制地址数值以表达两个成员指向同一个内存块；引用类型始终拷贝的是引用
    3. 结构的定义和使用：`[访问修饰符] struct 结构名`
    4. 结构和类的区别：
       1. 实例化需要把全部字段都实例化；引用类型则只初始化必要的，其他的都默认初始化为可以用 0 表达的类型；
       2. 结构必须在构造器里实例化所有成员；引用类型并非如此
       3. 结构的无参构造器随时都可以用；引用类型构造器只在你不写任何自定义构造器的情况下会自动生成
       4. 实例化结构对象，如果是在方法里，那么就是放在栈内存里的，它会在临时存储区创建对象，然后以默认行为初始化成员，最后调用构造器赋值；而引用类型则不用，直接创建在堆内存上；当然，值类型也可以放在堆内存里，不过需要引用类型的帮助，或者是某个结构里的字段。如果是全局字段还是放栈内存里的话，那么放在哪个栈帧上呢？
    5. 值类型的复制构造器：值类型的复制构造函数可以直接使用 `this` 关键字来接收赋值信息，因为值类型是复制副本，因此完全可以直接通过赋值运算符把数值给过去

    ```csharp
    public struct Student
    {
        private int _age;
        private bool _isBoy;
        private string _name;

        public Student(Student another)
        {
            // Assign directly.
            this = another;
        }
    }
    ```

    6. 值类型和引用类型的存储区转换：**装箱**（Boxing）和**拆箱**（Unboxing）：
       1. 装箱：`object i = 3;`，即值类型赋值给引用类型时，就会把数值包装起来，放在堆内存里，以地址形式表达；
       2. 拆箱：`int i = (int)o;`，即引用类型转回值类型时，就会把数据信息提取出来，放回栈内存
       3. `object.ReferenceEquals` 对值类型处理结果总为 `false`，因为需要装箱，随后导致地址一定不相同
       4. 装箱和拆箱很耗性能，请尽量不要发生此类行为
       5. 拆箱类型错误会引起运行时异常（类似于前面强制转换错误时产生异常）
    7. 值类型不可继承任何类，但能实现接口；值类型不能派生自别的值类型，默认派生自 `System.ValueType`（而它又派生自 `System.Object`，它们都是引用类型）；在自定义值类型时，为了不影响执行效率和性能，请把默认实现的所有方法都重写掉

12. **枚举**（Enumeration）

    1. 写法：`[访问修饰符] enum 枚举名`
    2. 在里面写字段，和 C 的用法基本上可以说是完全一样的
    3. 这些字段在外部引用的时候必须写 `枚举名.字段`
    4. 枚举默认派生自 `System.Enum`，所以不可重新继承
    5. 枚举的数值映射关系依赖于基本整数数据类型，语法：`枚举声明 : 基本数据类型名`，基本数据类型只能用整数的相关数据类型，即 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long` 和 `ulong`
    6. 枚举和数值的转换关系：
       1. 枚举转数值：`(数据类型名)枚举字段名`
       2. 数值转枚举：`(枚举类型名)数值`，数值只能是整数类型，且保证枚举类型的基本数值范围和枚举类型能够兼容
       3. 枚举转字符串：`枚举字段名.ToString`
       4. 字符串转枚举：`Enum.Parse(字符串, typeof(枚举类型名))`
    7. 枚举预定义好的运算符：大部分都有，只有少部分的没有预定义的运算符；部分运算符只支持枚举和整数、或者枚举和枚举的运算
    8. `FlagsAttribute` 特性：标记该枚举是用来表示比特 flag 的
       1. 查看是否该枚举包含此 flag：`枚举字段 & 查找字段 == 查找字段`，或使用 `Enum.HasFlag` 方法
       2. 对枚举某 flag 置位：`枚举字段 | 某置位字段`
       3. 对枚举某 flag 取消置位：`枚举字段 & ~某置位字段`

13. **委托**（Delegate）

    1. 委托：C 里的函数指针的封装，兼容和允许在 C# 里也可以用委托达到目的；委托在 C# 里是引用类型，所以可能为 `null`
    2. 写法：`[访问修饰符] delegate 返回值类型 委托名(参数表列)`
    3. 写法在入门的时候不好理解，可以认为是 C 里 `typedef` 的写法格式
    4. 冒泡排序演示：让同学们知道可以把委托当成一种函数指针用在参数上：

    ```csharp
    // Delegate type stetement.
    internal delegate bool Comparer<T>(T left, T right);

    internal class ProjectTester
    {
        // Method statement.
        // Please note that this method is a generic method with the
        // generic type parameter 'T' to stand for any types.
        public static void BubbleSort<T>(T[] array, Comparer comparer)
        {
            // Check nullability.
            if (comparer == null)
                throw new ArgumentNullException(nameof(comparer));

            // Sort them.
            for (int i = 0; i < array.Length - 1; i++)
            {
                for (int j = 0; j < array.Length - 1 - i; j++)
                {
                    if (comparer.Invoke(array[j], array[j + 1]))
                    {
                        T temp = array[j];
                        array[j] = array[j + 1];
                        array[j + 1] = temp;
                    }
                }
            }
        }
    }
    ```

    5. 委托是引用类型，养成好习惯，检查 `null`
    6. 委托的实例化：`委托名 委托变量 = new 委托名(匹配的方法名)`；方法需要匹配委托的声明格式（签名要一致）
    7. 委托的基类型：`System.MulticastDelegate`，而这个又派生自 `System.Delegate`
    8. 委托的运算符：
        1. `operator !=` 和 `operator ==`：比较委托指向的函数是否一致
        2. `operator +`：为委托添加新的函数，到时候调用委托执行的时候（`.Invoke`）可连续调用多个方法
        3. `operator -`：为委托删除原本的函数，到时候调用委托执行的时候，这个原本有的函数就不会再被调用了
        4. 委托的 `operator +` 不满足交换律：委托的函数增减是按顺序的，如果顺序不对，删除函数就会失败：

        ```csharp
        CustomDelegate c = new CustomDelegate(A);
        c += new CustomDelegate(B) + new CustomDelegate(C);
        c -= new CustomDelegate(C) + new CustomDelegate(B); // Failed.
        ```

        5. 委托列表里的函数如果出现异常，内部调用的函数将会立刻终止执行，然后退到委托外部抛出该异常（不过 IDE 一般都能识别错误位置，所以显示的错误的执行信息确实在方法内，而不是委托外）

14. **事件**（Events）

    1. 事件：对于一些情况下需要立马做的行为，比如水温检测器（用 `Thermostat` 类表达）在温度发生变化后立刻执行一些操作，例如通知用户水温发生改变
    2. `event` 关键字，简单例子：

    ```csharp
    delegate void TemperatureChangedEventHandler(float newValue);

    class Thermostat
    {
        private float _temperature;
        private TemperatureChangedEventHandler _handler;

        public Thermostat(float temperature)
        {
             Temperature = temperature;

             _handler += new TemperatureChangedEventHandler(OnTemperatureChanged);
        }

        public float Temperature
        {
            get { return _temperature; }

            set
            {
                if (value != _temperature)
                {
                    _temperature = value;

                    // Trigger the event if the handler is not null.
                    if (_handler != null)
                    {
                        _handler.Invoke(value);
                    }
                }
            }
        }

        public event TemperatureChangedEventHandler TemperatureChanged
        {
            add
            {
                _handler += value;

                // Or.
                //Delegate.Combine(_handler, value);
            }

            remove
            {
                _handler -= value;

                // Or.
                //Delegate.Remove(_handler, value);
            }
        }

        public void OnTemperatureChanged(float newValue)
        {
            Console.WriteLine("The current value is " + newValue + ".");
        }
    }
    ```

    3. 事件的优点：事件是委托字段的封装器，因此出了这个类的任何其他地方都只能使用 `operator +=` 和 `operator -=` 防止直接篡改和修改委托的函数列表；这并不等于你可以用 `operator -=` 减掉原本的方法，因为封装的机制，这使得你不可能知道原来这里封装了什么方法，除非别人告诉你了（但这样已经没有体现出封装的意义了）
    4. 静态事件：事件并不属于任何**一个**对象的，而是通用机制，大家都要到这里做一些操作，这里我们可以使用静态事件来表达，语法：`static event 委托类型名 事件名 { 添加器; 删除器; }`；另外，静态事件很少用

15. **迭代器**（Iterator）

    1. 概念：用来使用 `foreach` 遍历
    2. 使用：实现 `GetEnumerator` 方法，简单介绍一下 `IEnumerator` 类型

16. **反射**（Reflection）

    1. 反射是什么：反射是程序能够获取自己的信息的一种机制，通过代码获取它自己写了多少个类啊，多少结构啊，每个类里有哪些字段啊，哪些属性之类的，直接通过字符串来取信息
    2. 反射怎么用：
        1. 创建所有非抽象类类型的子类型实例：使用 `IsAbstract` 属性和 `CreateInstance` 方法
        2. 创建非 0 作为开始索引的数组：`Array.CreateInstance`
        3. 代码行数检查器
    3. **特性**（Attribute）
        1. 特性的概念：标记到项目的所有成员（当然，临时变量除外）上，以达到一些效果
        2. 自定义特性举例：例如我给枚举的每一个成员标记上中文名，到时候可以借用特性把这些中文名进行输出：先使用 `Enum.GetValues` 方法获取每一个字段，然后通过反射获取到特性，最后显示特性结果数值即可（默认继承自 `System.Attribute` 抽象类）
        3. 原义字符标记 `@`：当用作标记时可以默认不写后缀 `Attribute`；但如果没有该后缀的特性，又同时出现 `AAttribute` 和 `A` 时，需要用 `@A` 表示标记的是 `A` 而不是 `AAttribute` 以防止二义性
        4. 其他常见的内置特性
            1. `AttributeUsageAttribute`：防止乱用、滥用特性标记，比如本来标记到字段上的特性我用到函数上、用到返回值上、用到类上，这些都是没有意义的，这个时候设置该特性可以防止它们乱标记；但是类似“只能标记到枚举字段上”的要求是做不到的：只能防止标注到非字段的成员，但不能防止标记到普通类的字段、还是结构里的字段、还是枚举的字段
            2. `ObsoleteAttribute`：表示这个方法不能再用了，已经有更好的实现模式；而为了兼容性，先标记该特性给下次用户，用户就能看到编译器警告或编译器错误，来让用户有一个缓冲时间来修改 API
            3. `MethodImplAttribute`：表示方法的不同实现模式：比如 C/C++ 里的 `inline` 函数在这里的实现方式是 `MethodImpl(MethodImplOptions.AggressiveInlining)`

17. 多线程

     1. **线程**（Thread）概念：一个程序就是一个进程，相当于工厂，这个工厂会有很多工人在协同工作，每个工人就类似线程的概念
     2. 使用线程：创建一个 `Thread` 类的实例对象，然后里面写上多线程要执行的操作（无返回值的方法，以委托形式传入）
     3. `Thread.Sleep`：线程休息指定的毫秒数
     4. **前台**（Foreground）线程和**后台**（Background）线程：
         1. 前台线程（前面讲的都是前台线程）：线程一旦执行就不受程序自身控制，而且创建会消耗大量性能（虽然你感觉不到）；后台线程在主程序关闭后自动终止并消失，有两种创建方式，第一种是通过前台线程的创建模式，使用 `Thread` 类创建；另外一个则是用**线程池**（Thread Pool）的资源
         2. 线程池：存储后台线程资源的一个逻辑集合
         3. 使用：`ThreadPool.QueueUserWorkItem` 方法
     5. 线程内抛出异常跟之前委托抛出异常的执行效果类似，它并不会在线程内抛出，而是终止线程后，退回线程外部抛出该异常；线程报错时会把错误信息包裹存储起来，待退出线程后直接抛出
     6. 线程同步
         1. 为什么要线程同步：生产者消费者模型

         ```csharp
         public class Program
         {
             const int Times = int.MaxValue;
             int count = 0;

             static void Main()
             {
                 Thread thread = new Thread(new ThreadStart(Decrement));

                 Increment();
             }

             static void Increment()
             {
                 for (int i = 0; i < Times; i++)
                     count++;
             }

             static void Decrement()
             {
                 for (int i = 0; i < Times; i++)
                     count--;
             }
         }
         ```

         最后看到的结果可能是一个随意的数值（不可再现性）

         2. 使用 `Monitor.Enter` 和 `Monitor.Exit` 达到同步要求：创建一个 `object` 对象，直接实例化出来（`new object()`），然后来锁住它
         3. 锁住的对象不建议使用值类型，建议使用私有的、静态的引用类型对象；不建议使用 `this`，如果程序挂了，那么 `this` 无法解锁，更容易出问题；不建议使用 `typeof(string)` 和 `string` 之类的东西，因为 `string` 在 C# 里被特殊处理过，是不变的，就很类似于值类型赋值的复制副本，因此不建议
         4. `volatile` 修饰符：保证可被多个线程读写
         5. `System.Threading.Interlocked` 类：提供一些原子性操作，例如交换（`CompareExchange`）、增大一个单位（`Increment`）、减小一个单位（`Decrement`）等
         6. 事件在增减的时候发生的问题：请尽量建议在事件增减方法的时候使用 `lock` 语句防止多个线程读写而报错
         7. **死锁**（Deadlock）：
             1. 四大特性：**互斥**（Mutual Exclusion）、**占有和保持**（Hold & Wait）、**不可抢占**（No preemption）和**循环等待**（Circular wait），四大特性缺一不可，都满足时必然死锁；参考文章：https://user.qzone.qq.com/747507738/blog/1598518585 的 Part 5（死锁）
             2. 避免不必要的锁定，以减少甚至消除死锁的可能性
         8. 可用于同步的模型：互斥量 `Mutex`
         9. 其他东西
             1. `ThreadStaticAttribute`
             2. `STAThreadAttribute`
             3. `MethodImplAttribute` 实现同步方法（每次只能由一个线程调用，即方法是原子性的）

18. 语言版本板块

    1. C# 2
        1. **泛型**（Generic）
            1. 泛型是什么：将类型参数化，换句话说就是允许类型本身可以作为未知量参与编程运算的过程
            2. 泛型的好处：方便写代码，避免不必要的装箱和拆箱
            这里可以举例，比如我有一个列表想要支持所有的数据类型，这样唯一的办法是只能用 `object` 作为成员（因为它是所有类型的基类型）；但值类型会装箱，导致性能上的损失
            3. 泛型类自定义：
                * `[访问修饰符] struct 结构体名<泛型参数>`
                * `[访问修饰符] class 类名<泛型参数>`
                * `[访问修饰符] interface 接口名<泛型参数>`
            4. 文档注释：`<typeparam>` 表示泛型参数的含义
            5. 定义构造器的时候不写泛型参数，因为在声明时就确定了参数名了，就不必再写一次；但在实例化成员的时候必须给出泛型参数以提示实例化的对象到底应该用什么类型作为泛型参数
            6. `default(T)` 字面量：可以确定这个泛型类型的默认数值，到具体化的时候会确定结果信息
            7. 类型参数可以有多个，用尖括号括起来，用逗号分隔：`<类型1, 类型2, 类型3, ...>`
            8. 泛型嵌套：内部泛型名需要和外部的不同，否则会有编译器警告
            9. 泛型约束：在编译期搞定你不需要啥类型作为泛型参数
                1. 写法：`where 泛型名 : 泛型约束`
                2. 可以用的泛型约束：
                    * 别的类，表示该泛型参数必须派生自这个类
                    * 别的接口，表示该泛型参数必须实现了这个接口
                    * `struct`，表示该泛型参数必须是值类型
                    * `class`，表示该泛型参数必须是引用类型
                    * `new()`，表示该泛型参数自带无参构造器
            10. 泛型方法：对特有的方法追加泛型参数达到别的效果，用法和前面说的差不多
                1. 写法：`[访问修饰符] (static)? 返回值类型 方法名<泛型参数表列>(参数表列)( : where 泛型参数 : 泛型约束)?`
                2. 泛型参数类型推断：如果可以从参数和返回值确定泛型参数的具体类型时，可以不写；但如果有多个方法重载，导致无法确定使用哪个函数时，需要写出泛型参数
                3. 泛型方法的重载：可以允许重名的不同泛型约束的泛型方法，但当如果一个对象同时满足两个泛型约束的时候，会产生编译器错误，因为不知道应该调用哪一个方法；但如果确实需要重载不同泛型约束但同名同参数类型的方法时，请建议重载类似于 `where T : struct` 和 `where T : class` 带这样的约束的方法，以保证泛型约束一定不冲突
            11. 泛型参数是可以使用类型转换运算符 `(T)` 的
            12. 泛型的实现原理：运行时也可以看到泛型，它实际上类似于签到的模板表格，在用的时候写上你要用的这个泛型它的泛型参数类型是啥，具体就会自动把它填入到表格里，使得整个结构或者类的所有泛型参数都专门化（就是直接表示为具体类型），此时就可以直接用这个结构或者类了
            13. 泛型的缺陷：
                * 不允许指针类型作为泛型参数
                * 没有以方法为单位的约束
                * 不允许条件或的约束
                * 不允许类似数值运算的约束（运算符相关的）
            14. 其他泛型接口使用：
                * `IEnumerable<T>`：使用后可以让对象用 `foreach`
                * `ICollection<T>`：表示对象是一个集合
                * `IReadOnlyCollection<T>`：表示对象是一个只读的集合
                * 还有一些其他的我们这里就不讲了，我们后面会说到的
        2. 分部类型：`partial` 关键字，可以把东西放到多个文件里
        3. 匿名函数：使用简单语法，就不必单独创建外部方法了：

        ```csharp
        public delegate bool Comparison<T>(T left, T right);

        public BubbleSort<T>(T[] array, Comparsion comparsion)
        {
            // ...
        }
        ```

        调用方：

        ```csharp
        BubbleSort(
            // Normal array.
            arr,
            // Anonymous function.
            delegate(T left, T right) { return left >= right; }
        );
        ```

        4. 迭代器模式：使用 `yield return` 和 `yield break` 关键字来生成迭代结果和阻止迭代：

        ```csharp
        public IEnumerable<int> GetValuesBetween(int min, int max)
        {
            if (max < min) yield break;

            for (int i = min; i <= max; i++)
            yield return i;
        }
        ```

        泛型接口 `IEnumerator<T>` 可以简单说一下
        5. 可空值类型：表示包含 `null` 的值类型
            1. 写法：`值类型? 变量名 = 数值`
            2. 注意它是一个封装的结构，因此不能直接当普通的数值用，不过大部分值类型的用法都是可以用的，比如使用 `operator ==` 等运算符；但如果数值为 `null` 的时候，直接使用类似于 `.ToString` 的方法，会直接产生错误
        6. 访问级别不同的 getter 和 setter：允许 getter 和 setter 包含不同的访问级别，比如

        ```csharp
        public int Property
        {
            get { return _property; }
            private set { _property = value; }
        }
        ```

        7. 委托转换：允许委托传参自动或直接传入方法时，可以不以匿名函数的格式写出参数然后调用，举例：

        ```csharp
        static string GetStringResult(int v)
        {
            return v.ToString();
        }

        // Older style.
        SomeMethod.InvokeMethod(delegate (int v) { return GetStringResult(v); });

        // Newer style.
        SomeMethod.InvokeMethod(GetStringResult);
        ```

        8. 静态类：允许类添加 `static` 修饰符修饰，表示该类是静态的，只包含静态成员，防止实例化，也可以便于优化代码（比如 `System.Math` 类）
        9. 委托引用：允许委托成员直接以方法名代替，以表达方法自动转换为委托，可直接参与运算，减少委托实例化的复杂代码格式：即 `Action a = new Action(F)` 可简写为 `Action a = F`
        10. 类型别名：给类型取别名，在当前文件里随意生效：`using StatusCode = int`；一般写在使用命名空间的后面
        11. 委托的协变性和逆变性：允许委托类型传入的方法具有协变性和逆变性的行为，方便传参不匹配的时候也可以兼容，比如 `A` 类派生 `B` 类，而 `B` 类又派生 `C` 类的时候，我们在委托里如果传参或返回值是这样的类型，例如

        ```csharp
        public delegate void Tester1(A abstractInstance);
        ```
        此时我们可以添加一个参数不一定是 `A` 类的方法进去，比如 `delegate(B instance) { ... }` 之类的方法，因为 `B` 类是 `A` 的子类型，而这个类型依然是属于和满足 `A` 类型里的所有成员实现的，那么为什么不能允许呢？
    2. C# 3
        1. 隐式类型的本地变量：使用 `var` 来接受任何类型，便于书写代码（如果遇到不知道返回值啥类型，或者返回值类型不重要的时候，可以使用）

        2. 对象的集合初始化器：可以使用大括号给一个集合进行初始化，例如 `List<T>`：

        ```csharp
        var list = new List<int> { 1, 14, 16 };
        ```

        3. 自动属性：当属性的实现机制完全不关心的时候（单纯的 `return _value;` 和 `_value = value;` 的时候），可以使用自动属性省略不必要的大括号：

        ```csharp
        public int Property { get; set; }
        ```

        同时这个写法也允许不同的访问修饰符：

        ```csharp
        public int Property { get; private set; }
        ```

        4. 匿名类型：类型使用 `var { 属性名 = 数值 }` 的形式来表达，临时可以用
        5. 扩展方法：允许对静态方法的第一个参数添加 `this` 关键字，以让这个方法看起来更像是“就是这个类型的实例方法”一样
        6. LINQ
            1. `from` 和 `select` 从句
            2. `let` 从句
            3. `where` 从句
            4. `orderby` 从句：
                1. `orderby a.Prop ascending`
                2. `orderby a.Prop descending`
            5. `group by`从句：
                1. `group a by a.Prop`
                2. `group a by a.Prop into b`
                3. 为什么 `group by` 要拆开写，而 `orderby` 是合起来的？
                参考回答：https://github.com/dotnet/csharplang/issues/3609
            6. `join on equals` 从句
            7. 其他的聚合函数一览
                - `Max` 和 `Min`
                - `Average`
                - `Count` 和 `Any` 无参方法
                - `Any` 和 `All`
                - `Concat`
                - `Cast`
                - `Distinct`
                - `Range`（静态函数）
                - `Repeat`（静态函数）
                - `Take` 和 `Skip`
                - `ToArray` 和 `ToList`
                - `First`、`FirstOrDefault`、`Last` 和 `LastOrDefault`
                - `Single` 和 `SingleOrDefault`
                - `ElementAt`
                - `Aggregate`
        7. Lambda 表达式：匿名函数的升级，类型都可以不写了：
            1. 基本格式：

            ```csharp
            public delegate bool Comparison<T>(T left, T right);

            public BubbleSort<T>(T[] array, Comparsion comparsion)
            {
                // ...
            }
            ```

            调用方：

            ```csharp
            BubbleSort(
                arr,
                (left, right) => left >= right
            );
            ```

            2. Lambda 表达式可以捕获变量了：可以使用外部的变量，不过都是引用传递的，也就是说，如果你在内部修改该数值，外部的变量也会跟着变
        8. 表达式树：允许自定义表达式序列，使用 C# 代码生成一个表达式
        9. 分部方法：
            1. 允许把方法的声明和实现分开放到两个不同的文件里
            2. 如果不实现的话，编译器就会自动删除掉这个方法的调用，就当不存在一样
        10. `lock` 语句：`Monitor.Enter` 和 `Monitor.Exit` 的包装
    3. C# 4
        1. 动态绑定：允许使用 `dynamic` 关键字表达一个数据类型，该类型可以使用任何写法，来代替一个对象的执行，具体这个写法对不对取决于运行时检查对象是不是这个类型，有没有这个调用方式
        2. 可选参数和命名参数：允许把参数名写在参数上保证函数调用模式
        3. 协变性和逆变性标记到泛型委托和泛型接口上：
            1. 逆变性：`interface ITestInterface<in T>`，表达 `T` 只用在函数调用传参的这一边里，到时候这个 `T` 就可以被可以逆变的地方成功赋值：`ITestInterface<int> i = new Instance<object>()`
            2. 协变性：`interface ITestInterface<out T>`，表达 `T` 只用在函数返回值的这一边，到时候这个 `T` 就可以被可以协变的地方成功赋值：`ITestInterface<object> o = new Instance<int>()`
    4. C# 5
        1. 异步方法：使用 `async` 和 `await` 关键字配套修饰一个函数和一个执行步骤，表达这个方法执行很慢，会开单独的线程执行，并等待线程执行完毕后反馈结果
            1. `Task` 和 `Task<T>` 类
            2. **一路 `async`**（Async all the way）
        2. 调用方信息特性：允许使用 `CallerFilePathAttribute`、`CallerLineNumberAttribute` 和 `CallerMemberNameAttribute` 特性修饰参数，用于获取调用方的调用代码文件、行号和成员信息等字符串信息
        3. `foreach` 循环的 bug 修复：它会生成新的遍历，而不是直接每次都用同一个变量
    5. C# 6
        1. `using static`：允许对一个具体的类、结构、枚举、接口和委托等直接引用，使得后续就不用再使用类似 `枚举.枚举成员` 的语法来确定字段信息
        2. 异常筛选器：使用 `when` 关键字确定异常的具体执行行为：`catch (Exception ex) when (ex.HResult == ...)`
        3. `await` 可以放在 `catch` 和 `finally` 里了
        4. 自动属性的初始化器：可以对一个自动属性的最后面进行赋值，表达对内部字段的初始化
        5. 只有 getter 的属性：自动属性可以只写 getter 了：`public int Property { get; }`
        6. Lambda 风格的成员表达：部分用于返回的成员都可以使用 `=>` 来修饰返回值结果

        ```csharp
        public int Prop => 3;

        // Equivalent to
        public int Prop { get { return 3; } }
        ```

        7. `null` 条件运算符（`?`）和 `null` 传播运算符（`??`）
            1. `?` 运算符是后缀运算符，表示对对象成员调用之前先判断对象是不是 `null`；如果是，则后续操作不执行
            2. C# 的 bug：`a?.b.c` 和 `(a?.b).c` 的区别
            3. `??` 运算符是中缀运算符，表示对对象为 `null` 的时候取后者的默认数值；否则就取本身
            4. `??` 的串联
        8. 内插字符串：允许字符串用 `$` 修饰表示内部内容可以内插，代替字符串的加号：

        ```csharp
        int i = 24;
        string s = string.Format("The boy is {0} years old.", i);
        string t = "The boy is " + i + " years old.";

        // C# 6.
        string u = $"The boy is {i} years old.";
        ```

        9. `nameof`：把成员用 `nameof` 关键字修饰，返回一个字符串，即这个成员的字符串表达形式，类似于 C 里的 `#` 宏：`#Hello` 返回 `"Hello"`
        10. 字典的初始化器：允许对字典进行初始化：

        ```csharp
        var list = new Dictionary<int, string>();
        list.Add(1, "A");
        list.Add(2, "B");
        list.Add(3, "C");
        list.Add(4, "D");

        // C# 6.
        var list = new Dictionary<int, string>
        {
            [1] = "A",
            [2] = "B",
            [3] = "C",
            [4] = "D"
        };
        ```
    6. C# 7
        1. `out` 参数
            1. 允许方法以 `out` 作为参数修饰符，表达返回多个数的时候，从参数返回
            2. 允许该类型变量可在调用处定义变量，比如 `int.TryParse("123", out int result);`，这里的 `result` 就是定义出来的变量
        2. 模式匹配：`o is int i`
        3. 值元组：允许使用小括号语法对多个变量赋值和取值的一种机制：
            1. 回顾一下 C# 4 提供的元组类型 `Tuple<>`，不好用的地方在于它必须要按照 C# 的语法来写，而且很笨重（引用类型），再加上取数值的时候需要使用 `Item1`、`Item2` 这样的名字，很丑，所以值元组（`ValueTuple<>`）诞生了
            2. 写法：`元组类型或var 变量名 = (数据列表)`，比如

            ```csharp
            var pair = (24, "Sunnie");
            var triplet1 = (Name: "Sunnie", IsBoy: true, Age: 24);
            (string Name, bool IsBoy, int Age) triplet2 = ("Sunnie", true, 24);
            var mixed = (Name: "Sunnie", 24);
            (int, bool) unnamed = (Value: 10, BooleanResult: false);
            ```

            也就是说，可以不取名，也可以取名；如果不取名（比如 `pair` 这样），那么取值就需要使用标准语法 `pair.Item1` 等来取值；如果取了名，那么就可以直接用取名：`triplet1.Name`、`triplet2.Age` 等；也可以混合取名和不取名的信息，不取名的用标准语法，取名的用取的名字来获取数值；最后一种（`unnamed`）是一种把取了名的值元组赋值给了一个没有取名的值元组，所以接收结果没有取名，因为这个值元组变量本身就没有取名；取值照样要用 `unnamed.Item1` 等语法来取值；值元组最多允许 8 个元素，即 8 元组
            
            3. 原版语法：这种写法等价于 `ValueTuple.Create` 方法的调用：

            ```csharp
            var pair1 = (24, "Sunnie");
            
            // Equivalent to
            var pair2 = ValueTuple.Create(24, "Sunnie");
            ```

            4. 因为背后机制是用 `public` 的字段实现的，所以在取名的时候请建议使用帕斯卡命名法进行命名
            5. 对位赋值：使用值元组的语法对不同的变量或一些属性赋值：

            ```csharp
            var (age, name) = (24, "Sunnie");
            (int age, string name) = (24, "Sunnie");
            
            // Equivalent to
            int age = 24;
            string name = "Sunnie";
            ```

            这种语法只是写起来像是值元组，实际上还是对位赋值的；而且要注意的是，这个语法是值元组的语法，所以不允许指针在里面，因为泛型参数是不允许指针的

            6. 值元组元素名称可推断：类似 C# 3 的匿名类型，只要我们传入的是变量名而不是字面量的话，这个变量名就会自动被认为是这个值元组的当前元素名称，比如：

            ```csharp
            Student thatStudent;
            var pair = (thatStudent.Age, thatStudent.Gender);
            
            // Equivalent to
            (int Age, bool Gender) pair = (thatStudent.Age, thatStudent.Gender);
            ```

            这样的格式，而不是我们以为的 `(int, bool) pair = ...`

        4. 解构器（解构函数）：允许在一个类或结构里实现 `Deconstruct` 方法，来让这个类或对象拥有解构的行为：

        ```csharp
        public struct Student
        {
            public int Age { get; }
            public bool IsBoy { get; }
            
            public void Deconstruct(out int age, out int isBoy) =>
                (age, isBoy) = (Age, IsBoy);
        }
        ```

        这样写了之后，`Student` 类型的实例都可以使用元组语法来获取相应对象的数据结果：

        ```csharp
        // Suppose we have a variabe named 'student'.
        var (age, isBoy) = student;
        ```

        5. 弃元：允许变量声明的地方使用弃元关键字 `_` 来表达这个变量我们不要了（放弃掉，不用它）：

        ```csharp
        var (_, isBoy) = student;
        ```

        这种写法表达我不想用解构器返回的 `age` 结果，所以我写弃元符来占位

        6. 本地函数：允许一个方法嵌套到某一个方法里，来表示这个方法本身只在某一个方法里执行和使用
        7. 二进制字面量：允许二进制形式的字面量：`0b11100011`
        8. 数字分隔符：允许对整数字面量用下划线分隔每一节内容，增强可读性：`0b11_10_00_11`；分隔符可以在任何时候和任何地方写（当然了，肯定不能写在 `0` 和 `b` 之间，对吧）
        9. `ref` 临时变量和 `ref` 返回：允许一个值类型对象以引用形式返回和定义变量，这样来减少内存分配，提高执行性能和效率
        10. 自定义异步返回类型：可以自己写一个异步类型出来，这样可以使用 `async` 语法来兼容你写的这个类型，但需要实现指定的方法和接口
        11. 更多的 Lambda 风格表达：现在属性、方法、索引器等成员都可以使用 Lambda 风格来表达返回结果了
        12. `throw` 表达式：内嵌到 `operator ?:` 和 `operator ??` 里，这样可以简化执行的代码
        13. 异步 `Main` 方法：允许 `Main` 方法是异步执行的：`async Task Main`
        14. `default` 表达式可以推断类型了：以前我们需要把字面量的类型写出来来表示这个类型的默认值；现在我们可以不写类型，只要能推断出类型就可以了：`int val = default`
        16. 允许泛型可以使用模式匹配
        17. `Span<T>` 类型和 `ref struct`（仅放在栈内存的类型）
            1. 诞生原因：因为普通的 `struct` 是可以装箱的，所以不便于编译器优化代码，所以高效率的 `ref struct` 诞生了：它只能放在栈内存里，用来传参使用，保证性能和效率
            2. 为什么是 `ref` 这个关键字：因为我们之前经常拿 `ref` 关键字来表达临时变量（之前的 `ref` 临时变量一节），所以这个 `ref` 在这个时候暗含了一层“可以用来充当临时变量”这么一层意思在里面，所以 `ref struct` 就表示只能用在临时变量（栈内存）的结构
            3. 并不是一定要临时变量：只要你有 `ref struct` 的结构，那么里面就可以放别的 `ref struct` 的字段或属性；但如果是普通的结构，就不能放 `ref struct` 类型的东西来作为成员了
            4. 使用条件很苛刻，是因为它本身限制非常多，要这么多限制才能保证真正意义上的栈内存使用：
                * 不能对 `ref struct` 装箱：因为无法向属于 `object`、`dynamic` 或任何接口类型的变量分配 `ref struct` 类型
                * `ref struct` 类型不能实现接口
                * 不能将 `ref struct` 声明为类或常规结构的字段成员，包括声明自动实现的属性，而后者会创建一个由编译器生成的支持字段，这是在堆内存的
                * 不能声明异步方法中属于 `ref struct` 类型的本地变量，也不能在返回类似 `Task` 或者 `Task<TResult>` 类型的同步方法中声明它们
                * 无法在迭代器中声明 `ref struct` 本地变量
                * 无法捕获 Lambda 表达式或本地函数中的 `ref struct` 变量
        18. `in` 参数修饰符和 `readonly ref`：允许返回值和参数传参的时候只读，不可修改，类似 C 里的 `const`，但不同的是，C 的 `const` 是编译器层面的，而且你可以绕过；C# 的 `in` 是传指针过去，使得没办法修改；`readonly ref` 则是约束返回值传引用的基础上也不可修改
        19. `ref` 条件表达式：允许 `operator ?:` 返回 `ref` 结果
        20. 命名参数不一定只出现在末尾：C# 4 的命名参数只能放在方法的末尾，现在命名参数随时随地都可以用了
        21. `private protected` 访问修饰符：继承关系仅在项目内生效，别的外部程序调用 API 的时候将无法看到这类成员，即使是继承了
        24. `System.Enum` 和 `System.Delegate` 泛型约束
            1. `System.Enum` 约束传入的对象是一个枚举类型
            2. `System.Enum` 仅约定是一个枚举类型，但枚举特有的运算符在这个地方不能用；如果需要用运算符，请你将它转换为值类型后进行值类型的运算符进行计算（需要获取它的大小，因此请参考下面 C# 8 `unmanaged` 约束的做法）
            3. `System.Delegate` 约束一个委托类型
        26. `stackalloc` 的初始化器：允许使用类似数组初始化器的模式初始化栈内存数组：`stackalloc[] { 1, 2, 3 }`
        27. `fixed` 字段：允许字段在初始情况下就固定下来长度，该字段将被改写为指针，但因为长度固定，所以内容可以直接使用，通过索引器的方式使用
        28. 自定义 `fixed` 语句声明（实现 `GetPinnableReference` 即可）
        31. 值元组可以使用 `operator ==` 和 `operator !=` 了
        32. 允许把特性标记到属性上，直接指定到隐藏到背后的字段上
    7. C# 8
        1. 可空引用类型
            1. 允许使用类似可空值类型的语法 `T?` 来约束引用类型，规定和查看类型是否可空
            2. 启用特性的两种办法：
                1. 在 `*.csproj` 文件里添加 `<Nullable>enable</Nullable>` 以全局启用
                2. 在文件的开头添加预处理指令 `#nullable enable`
            3. 预处理指令：
                * `#nullable enable`
                * `#nullable enable annotations`
                * `#nullable enable warnings`
                * `#nullable disable`
                * `#nullable disable annotations`
                * `#nullable disable warnings`
                * `#nullable restore`
                * `#nullable restore annotations`
                * `#nullable restore warnings`
            4. 基本用法：`string? s = null;`，当使用 `string s = null;` 时会产生编译器警告信息，提示你正在把 `null` 赋值给一个不可空的引用类型；如果把 `T?` 类型变量赋值给 `T` 类型的变量时，也会产生警告
            5. `null` 原谅运算符（`Null`-forgiving Operator）
                1. 当编译器并不知道我们这里赋值 `null` 不影响可空性的时候，可以使用这个后缀运算符来禁用编译器对这个行为的警告：比如我有一个 `Point` 的类，包含 `X` 和 `Y` 两个信息，然后写了一个解析函数 `Parse`，把字符串转换为这个类型的变量。由于解析有可能失败，所以返回值应当是 `Point?` 的类型。但如果我们使用 `Point result = Point.Parse("(1, 3)");` 类似的写法时，由于返回值是 `Point?` 类型的关系，就会产生编译器错误；所以此时我们需要对这个错误的编译器警告进行禁用：`Point result = Point.Parse("(1, 3)")!;`，在末尾加一个 `!` 就可以了
                2. 注意运算符会改变类型的语义。如果代码里这么写：

                ```csharp
                var p = Point.Parse("(1, 3)")!;
                var q = Point.Parse("(1, 3)");
                ```

                那么 `p` 是 `Point` 类型，而 `q` 此时应该是 `Point?` 类型

            6. `as` 转换**一定可能**失败，所以返回值必然可能为 `null`，所以如果一定不空，要么建议你使用 `(T)` 强制转换，要么使用 `!` 禁用编译器警告
            7. `null` 原谅运算符本身很鸡肋也很奇葩，所以不建议随时随地都用这个运算符，而是使用如下的一些模型来减少和削弱对该运算符的使用：
                1. `NotNullWhenAttribute`：表示返回值类型是布尔值的时候，可以对应到结果是否为 `null`：如果返回值为 `true`，则该数值结果不应该为 `null` 的时候，可以写成 `[NotNullWhen(true)]`，标记到参数上表示该参数满足该约定
                2. `AllowNullAttribute`：在环境下本来必须写成 `T` 的类型，但临时需要允许传入 `null` 的时候可以用这个特性标记
                3. `DisallowNullAttribute`：在环境下本来必须写成 `T?` 的类型，但临时需要传入非 `null` 的时候可以用这个特性标记
                4. `MaybeNullAttribute`：不确定这个类型是不是为 `null`，一般用在带泛型参数的参数名、返回值、属性或者字段等成员上
                5. `NotNullAttribute`：如果类型必须写成 `T?` 的时候，此时如果输出时不可能为 `null` 的时候可这样标记，这样编译器就知道只有输入方向（比如传参）才是 `T?`
        2. 默认接口成员：允许接口里写一些函数的具体实现
        3. 递归模式：允许在 `is` 模式匹配的时候使用属性或 `public` 字段和数值匹配的语法：`stu is { Age: 20, IsGirl: false }`
        4. `await foreach` 和 `await using`：允许异步迭代和异步使用一些资源
        5. `using` 语句：不用再写烦人的大括号了，现在直接写 `using var x = ...;` 就可以了
        6. 范围和索引运算：`^` 和 `..`：
            1. `^` 写在一个带 `Length` 或 `Count` 属性的集合的中括号索引调用里，表示取倒数第几个元素：`string s = "Hello"; char t = s[^2];`
            2. `..` 写在一个带 `Slice` 方法、`Length` 或 `Count` 属性的集合里的中括号索引调用里，表示取哪一个范围的元素：`string s = "Hello"; string t = s[0..^2]`；如果范围头部是 0 可不写；如果范围尾部是 ^1 也可以不写
        7. 复合 `null` 传播赋值运算符 `??=`：`a = a ?? b` 可以简写为 `a ??= b`
        8. 静态本地函数：允许本地函数标记 `static` 关键字，以禁止捕获任何变量
        9. `unmanaged` 泛型约束
            1. 非托管类型定义拓展：
                1. 所有基本类型（`string` 和 `object` 除外）都是非托管类型
                2. 只包含这些类型的字段和属性的成员的结构，都是非托管类型
                3. 只包含第 2 点提到的结构，也都是非托管类型
            2. 约束使得可以用 `sizeof` 关键字计算大小，也可以使用 `T*` 指针运算
            3. 枚举是非托管类型的，但是 `System.Enum` 在泛型约束里只约束类型，不直接允许使用 `sizeof` 等直接的运算符
        10. 结构的只读成员：允许一个结构的局部实例成员标记 `readonly` 来表示操作是不会修改实例内部的信息的
        11. `stackalloc` 嵌套使用：可以在一个 `stackalloc` 内继续使用 `stackalloc` 了
        12. `$@""` 和 `@$""`：这俩都可以允许了，之前只能让 `$@""` 语法成功编译
        13. `ObsoleteAttribute` 标记到 getter 和 setter 其一上
        14. 允许泛型使用 `is null` 模式匹配：泛型在最开始是不让判断 `null` 的，现在可以允许一个泛型类型的变量判断是不是 `null` 了
    8. C# 9
        1. 记录类型：
            1. 一个简单的语法可以生成基本上所有的数据信息，来代替之前的匿名类型，可以长期使用，用法：`record Student(string Name, bool IsBoy);` 就可以生成很多信息，比如比较、属性本身、字符串输出、哈希码计算等方法
            2. 记录类型的继承：只能由一个记录继承于一个记录；`ToString` 方法是自动生成的，所以子类型必须重写基记录类型的 `ToString`，否则格式会不匹配
            3. 记录类型用于记录一些东西，所以没有 `static record` 一说
            4. `record struct` 目前不允许，因为目前暂时只支持引用类型，而值类型的相等性比较过于复杂，特别是可能产生递归比较的模型，所以会很麻烦
            5. `init` setter：记录的所有字段在生成的时候是自动把 `set` 改写成 `init` 的；这两个关键字都是一个属性的赋值器的有效格式写法，但区别在于 `set` 是随时随地都可以赋值，而 `init` 只有初始化（初始化器和构造器）时才可以使用，防止随意改变数据；可以从这个特性可以看出，记录类型是不可变的
            6. `with` 表达式：由于记录类型不可变，所以如果你要赋值给一个新类型，且需要保留大部分信息的时候，可以使用 `with` 来赋值和修改需要修改的成员信息：`var newStudent = oldStudent with { Age = 20 };`
            7. `init` 可以用于任何数据类型，但 `with` 目前只支持记录类型
        3. `Main` 的隐式写法：允许把一个复杂的类声明、`Main` 函数声明等信息直接去掉，使得这个文件只剩下执行信息：

        ```csharp
        // Older.
        using System;
        
        namespace Project
        {
            internal static class Program
            {
                private static void Main(string[] args)
                {
                    Console.WriteLine("Hello, world!");
                }
            }
        }
        
        // Newer.
        using System;
        
        Console.WriteLine("Hello, world!");
        ```

        4. `and`、`or` 和 `not` 模式：允许这三个关键字对数据本身的范围进行校验：
            1. `and`：查看条件必须都满足：`AMethodReturnsAValue() is var result and not null`、`val is >= 20 and <= 30`
            2. `or`：查看条件至少满足一个：`str is null or ""`、`val is < 20 or > 30`
            3. `not`：取反一个条件：`obj is not null`
            4. 注意优先级的问题
        5. 兼容 C 不定大小的 `int` 和 `unsigned int`：允许 `nint` 和 `nuint` 来直接代替掉一个 C 这种不定大小的 `int`，这样就可以不用写成条件编译符号的模式来设定了
        6. 函数指针
            1. 写法 `delegate* (managed|unmanaged[(Cdecl|StdCall|ThisCall)])? <参数表列>`
            2. 为了兼容底层的函数指针，可以使用 `unmanaged` 关键字，具体看的是底层用的是什么调用约定
        7. `SkipLocalsInitAttribute`：标记到执行操作之上，以保证执行期间直接跳过初始化，来优化性能
        8. 目标类型的 `new` 表达式：在可以在 `new` 实例化类型的地方推断出类型的时候可以不写出类型名称：`Student s = new()`；如果是带有初始化器的时候，默认小括号不可省略
        9. `static` Lambda 表达式或匿名函数：类似 `static` 本地函数的行为，不捕获变量
        10. 目标类型的 `operator ?:` 表达式：假设 `A` 类型派生 `B` 类型，而 `B` 又派生 `C` 类型的话，则原始写法需要对条件运算符的其中一方转换类型：`C result = condition ? (C)new A() : new B()`，现在可以不用了：`C result = condition ? new A() : new B()`，只用查看赋值这边是啥类型就可以确定了
        11. 协变返回类型：允许继承下来的子类型的这个方法，返回值类型可以从抽象类型逆变为具体类型；为啥叫协变返回而不是逆变返回，是因为名称是反过来看的
        12. Lambda 参数弃元：允许多个 Lambda 参数都不用的时候使用 `_` 弃元
        13. 可标记到本地函数上的特性
        14. 模块初始化器：允许在一个静态方法上标记 `ModuleInitializerAttribute` 特性来保证这个在程序开始初期就最先执行
        15. `GetEnumerator` 扩展方法：编译器现在支持识别 `GetEnumerator` 扩展方法来支持对任何实现该方法的数据类型进行 `foreach` 的迭代
        16. 分部方法增强：允许任何方法标记 `partial` 了，不过和 C# 3 的分部方法意义不同，这个分部方法必须必须实现，否则编译器错误
        17. 没有任何约束的泛型参数可标记 `?` 了
            1. 允许对一个完全没有任何泛型约束的泛型参数标记 `?` 来表达可为 `null`。如果它是值类型，则这个标记当没有用；如果是引用类型，则认为它可空
            2. 对于值类型而言（有 `where T : struct` 约束的），`T?` 类型参数在具体化参数的时候，会被认为是 `Nullable<T>`，即真正的可空值类型；而 `T?` 在没有该约束的时候，`T?` 跟 `T` 没有任何区别
            3. `default` 泛型约束：当对一个抽象方法或 `virtual` 方法重写的时候，如果原方法没有泛型约束的任何标记，此时可以使用 `where T : default` 标记，标记到重写方法上，保证 `T` 此时是默认没有任何泛型约束的；这一点专门用于区别和表达重写无泛型参数约束的方法（如果不用写，一般都可以不写，这一点仅用于保证程序的兼容性）：

            ```csharp
            class A
            {
                public virtual void F<T>(T? t) where T : struct { }
                
                public virtual void F<T>(T? t) { }
            }

            class B : A
            {
                // Equivalent to the method F with the constraint
                // 'where T : struct'.
                public override void F<T>(T? t) { }
                
                // The method overrides A.F(T?) without any constraint.
                public override void F<T>(T? t) where T : default { }
            }
            ```

19. 总结

