# C# 7
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
15. 允许泛型可以使用模式匹配
16. `Span<T>` 类型和 `ref struct`（仅放在栈内存的类型）
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
17. `in` 参数修饰符和 `readonly ref`：允许返回值和参数传参的时候只读，不可修改，类似 C 里的 `const`，但不同的是，C 的 `const` 是编译器层面的，而且你可以绕过；C# 的 `in` 是传指针过去，使得没办法修改；`readonly ref` 则是约束返回值传引用的基础上也不可修改
18. `ref` 条件表达式：允许 `operator ?:` 返回 `ref` 结果
19. 命名参数不一定只出现在末尾：C# 4 的命名参数只能放在方法的末尾，现在命名参数随时随地都可以用了
20. `private protected` 访问修饰符：继承关系仅在项目内生效，别的外部程序调用 API 的时候将无法看到这类成员，即使是继承了
21. `System.Enum` 和 `System.Delegate` 泛型约束
    1. `System.Enum` 约束传入的对象是一个枚举类型
    2. `System.Enum` 仅约定是一个枚举类型，但枚举特有的运算符在这个地方不能用；如果需要用运算符，请你将它转换为值类型后进行值类型的运算符进行计算（需要获取它的大小，因此请参考下面 C# 8 `unmanaged` 约束的做法）
    3. `System.Delegate` 约束一个委托类型
22. `stackalloc` 的初始化器：允许使用类似数组初始化器的模式初始化栈内存数组：`stackalloc[] { 1, 2, 3 }`
23. `fixed` 字段：允许字段在初始情况下就固定下来长度，该字段将被改写为指针，但因为长度固定，所以内容可以直接使用，通过索引器的方式使用
24. 自定义 `fixed` 语句声明（实现 `GetPinnableReference` 即可）
25. 值元组可以使用 `operator ==` 和 `operator !=` 了
26. 允许把特性标记到属性上，直接指定到隐藏到背后的字段上