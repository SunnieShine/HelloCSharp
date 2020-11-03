# C# 2
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