# C# 9
## 记录类型

1. 一个简单的语法可以生成基本上所有的数据信息，来代替之前的匿名类型，可以长期使用，用法：`record Student(string Name, bool IsBoy);` 就可以生成很多信息，比如比较、属性本身、字符串输出、哈希码计算等方法
2. 记录类型的继承：只能由一个记录继承于一个记录；`ToString` 方法是自动生成的，所以子类型必须重写基记录类型的 `ToString`，否则格式会不匹配
3. 记录类型用于记录一些东西，所以没有 `static record` 一说
4. `record struct` 目前不允许，因为目前暂时只支持引用类型，而值类型的相等性比较过于复杂，特别是可能产生递归比较的模型，所以会很麻烦
5. `init` setter：记录的所有字段在生成的时候是自动把 `set` 改写成 `init` 的；这两个关键字都是一个属性的赋值器的有效格式写法，但区别在于 `set` 是随时随地都可以赋值，而 `init` 只有初始化（初始化器和构造器）时才可以使用，防止随意改变数据；可以从这个特性可以看出，记录类型是不可变的
6. `with` 表达式：由于记录类型不可变，所以如果你要赋值给一个新类型，且需要保留大部分信息的时候，可以使用 `with` 来赋值和修改需要修改的成员信息：`var newStudent = oldStudent with { Age = 20 };`
7. `init` 可以用于任何数据类型，但 `with` 目前只支持记录类型；结构体里用 `init` 不会影响对象的只读，所以仍然可以对 `readonly struct` 的 `get`-only 属性添加 `init`，即 `int Prop { get; init; }`，毕竟赋值只发生在对象的最开始，所以后续期间不会修改对象

## 隐式 `Main`

允许把一个复杂的类声明、`Main` 函数声明等信息直接去掉，使得这个文件只剩下执行信息：

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
```

现在可以只写这一点就可以了：

```csharp
// Newer.
using System;

Console.WriteLine("Hello, world!");
```

## `and`、`or` 和 `not` 模式

允许这三个关键字对数据本身的范围进行校验：

### `and`

查看条件必须都满足：`AMethodReturnsAValue() is var result and not null`、`val is >= 20 and <= 30`

### `or`

查看条件至少满足一个：`str is null or ""`、`val is < 20 or > 30`

### `not`

取反一个条件：`obj is not null`

### 注意优先级的问题

下面列举一些例子给大家看看：

#### 示例 1

```csharp
a is not null and { Property: Value }
```

里面的 `not null and` 是冗余部分，可以不写（因为判断递归模式的时候，一定会验证 `null`，所以是不用写的；

#### 示例 2

```csharp
a is null or { Property: Value }
```

里面的 `null or { Property: Value }` 可以改写（简写）成 `is not { Property: not Value }`（双重否定），常用于集合为空或不带元素时使用：`collection is not { Count: not 0 }`

#### 示例 3

```csharp
a is not >= 0 and <= 9
a is not (>= 0 and <= 9)
```

一定要会区分这两个写法的区别。前者写了废话，实际上就是 `< 0`；后者则表示如果不属于 0 到 9 的其余数据，所以就是 `< 0 || > 9`，因此，一定要会理解

#### 示例 4（双模式匹配）

```csharp
a is var val and var (prop1, prop2, prop3)
```

这就表示 `a` 同时被定义为一个 `val` 变量（别名），以及解构成 `prop1`、`prop2` 和 `prop3` 三个属性

#### 示例 5（混用 1）

```csharp
a is not ('\r' or '\n')
a is not '\r' and not '\n'
```

#### 示例 6（混用 2）

```csharp
a is not (>= '0' and <= '9' or '.')
```

## 兼容 C 不定大小的 `int` 和 `unsigned int`

允许 `nint` 和 `nuint` 来直接代替掉一个 C 这种不定大小的 `int`，这样就可以不用写成条件编译符号的模式来设定了

## 函数指针

1. 写法 `delegate* [managed|unmanaged[(Cdecl|StdCall|ThisCall)]] <[参数表列, ]返回值类型>`
2. 为了兼容底层的函数指针，可以使用 `unmanaged` 关键字，具体看的是底层用的是什么调用约定

## `SkipLocalsInitAttribute`

标记到执行操作之上，以保证执行期间直接跳过初始化，来优化性能

## 目标类型的 `new` 表达式

在可以在 `new` 实例化类型的地方推断出类型的时候可以不写出类型名称：`Student s = new()`；如果是带有初始化器的时候，默认小括号不可省略

## `static` Lambda 表达式或匿名函数

类似 `static` 本地函数的行为，不捕获变量

## 基于目标类型的条件表达式

1. 假设 `A` 类型派生 `B` 类型，而 `B` 又派生 `C` 类型的话，则原始写法需要对条件运算符的其中一方转换类型：`C result = condition ? (C)new A() : new B()`，现在可以不用了：`C result = condition ? new A() : new B()`，只用查看赋值这边是啥类型就可以确定了
2. 注意一个编译器分析代码的爆破性改变：原本 `double = bool ? int : double` 的赋值模式是允许的，现在必须把 `int` 表达式强制转换成 `double` 才可以：`int` 和 `double` 虽然具有隐式转换，但是因为 `int` 和 `double` 是值类型，并不是引用类型的协变和逆变过程，因此无法直接推断

## 协变返回

允许继承下来的子类型的这个方法，返回值类型可以从抽象类型逆变为具体类型；为啥叫协变返回而不是逆变返回，是因为名称是反过来看的

## Lambda 参数弃元

允许多个 Lambda 参数都不用的时候使用 `_` 弃元

## 可标记到本地函数上的特性

## 模块初始化器

允许在一个静态方法上标记 `ModuleInitializerAttribute` 特性来保证这个在程序开始初期就最先执行

## `GetEnumerator` 扩展方法

编译器现在支持识别 `GetEnumerator` 扩展方法来支持对任何实现该方法的数据类型进行 `foreach` 的迭代

## 分部方法增强

允许任何方法标记 `partial` 了，不过和 C# 3 的分部方法意义不同，这个分部方法必须必须实现，否则编译器错误

## `MemberNotNullAttribute` 和 `MemberNotNullWhenAttribute`

这个特性用于标记一些方法，这些方法用于初始化某一个成员信息。由于 C# 语言可空性判别的官方设定，C# 无法以递归的方式检测对象的初始化情况（否则检测就会消耗大量性能）。这个特性标记帮助和辅助我们对一些对象进行初始化后，在方法本身上标记，这个方法用来初始化（或在某一个确切条件下初始化）某一个对象，这个对象的名称由特性的参数指定。

```csharp
public class TestClass
{
    public TestClass(int age) => Age = age;
    
    public int Age { get; init; }
}
public class C
{
    private TestClass _instance;

    public C(TestClass instance) => InitializeComponents();
    
    [MemberNotNull(nameof(_instance))]
    private void InitializeComponents() => _instance = new { Age = 24 };
}
```

C# 8 里由于没有该特性标记的支持，如果内嵌别的方法初始化了某个成员，C# 照样不会检测到初始化，于是 C# 会告诉你 `_instance` 字段没有初始化，请初始化或声明 `_instance` 为可空引用类型（`TestClass?`）；现在有了标记，这些警告全部会消失，并且它还会辅助你完成初始化：如果你标记了该特性却没有在方法里初始化它，编译器就会产生警告，告诉你必须初始化了它。

## 没有任何约束的泛型参数里的可空标记

### 说明

允许对一个完全没有任何泛型约束的泛型参数标记 `?` 来表达可为 `null`。如果它是值类型，则这个标记当没有用；如果是引用类型，则认为它可空

### 值类型和引用类型下 `T?` 写法的区别

对于值类型而言（有 `where T : struct` 约束的），`T?` 类型参数在具体化参数的时候，会被认为是 `Nullable<T>`，即真正的可空值类型；而 `T?` 在没有该约束的时候，`T?` 跟 `T` 没有任何区别

### `default` 泛型约束

当对一个抽象方法或 `virtual` 方法重写的时候，如果原方法没有泛型约束的任何标记，此时可以使用 `where T : default` 标记，标记到重写方法上，保证 `T` 此时是默认没有任何泛型约束的；这一点专门用于区别和表达重写无泛型参数约束的方法（如果不用写，一般都可以不写，这一点仅用于保证程序的兼容性）：

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

### `T` 和 `T?` 的复杂区别

稍微注意一下的是，如果一个没有任何泛型约束的泛型类型参数 `T` 标记了可空标记的话（即单纯的 `T?`），C# 8 和 9 里拥有的语法规则会把 `T` 定义出三种不同的泛型参数的可空状态：

1. `T where T : notnull`（有 `notnull` 的约束，且用的是 `T` 本身，无可空标记）；
2. `T? where T : notnull`（有 `notnull` 的约束，且带可空标记）；
3. `T`（没有可空标记，也没有约束 `notnull`）；
4. `T?`（有可空标记，但是没有约束 `notnull`）。

这样四种写法可能会让初学者很头疼。下面说一下四种写法的区别。

#### 带 `notnull` 约束的

如果泛型具有约束 `notnull`，这就说明 `T` 在最开始就不可能是可空类型的；换句话说，它只能是值类型或一定不可空的引用类型。比如说如下的语法：

```csharp
public void Append<T>(T val) where T : notnull
{
    // ...
}
```

在这个泛型方法里，`T` 从约束上确定 `T` 就一定不可能是可空引用类型；因此，如果你将一个可空的类型当泛型参数传入的时候，就会产生编译器警告。

```csharp
#nullable enable

append(30);
append<int?>(30); // Wrong.
append<int?>(null); // Wrong.
append<C>(new C());
append<C?>(new C()); // Wrong.
append(new R());
append(new S());

static void append<T>(T val) where T : notnull
{
    // ...
}

class C { }
record R { }
readonly struct S { }
```

那么，如果我们允许参数是 `T?`（但不修改泛型参数的约束，还是保持有 `notnull`）的话，那么类似这样的传参也是允许的：

```csharp
append<C>(null);
```

虽然 `C` 本身此时已经不可空了（因为启用了可空的编译器检查），但是因为传参写的是 `T?`，因此类型依旧允许可空。

#### 不带 `notnull` 约束的

那么，稍微复杂一点的就是没有 `notnull` 约束的情况。如果没有此约束，类型本身和具有 `notnull` 约束的时候**也基本上是一样**的。唯一的区别是，在我们使用的时候，由于泛型并不受到 `notnull` 的约束，因此类型是什么样子的都是不清楚的，因此，此时的 `T` 和 `T?` 就稍微有一点区别。比如这个代码：

```csharp
#nullable enable

append(30);
append<int?>(30);
append<int?>(null);
append<C>(new C());
append<C?>(new C());
append<C>(null);
append(new R());
append(new S());

static void append<T>(T val)
{
    // ...
}

class C { }
record R { }
readonly struct S { }
```

如果我们删除 `notnull` 约束，并将参数类型改回 `T` 后，你会发现编译器仅对 `append<C>(null);` 一行报错，其它的全部没问题了。因为此时我们的方法签名里，参数是 `T` 类型就约束了我们无法传入可为 `null` 的类型版本，所以写 `null` 必然就会出现问题。

但是，如果我们定义一个 `T?` 类型的变量，并传入此变量的话：

```csharp
#nullable enable

C? c = new C();

append(c);

static void append<T>(T val)
{
    // ...
}

class C { }
```

你猜猜会不会报错？答案是不会。这不更奇怪了吗？`c` 变量是 `C?` 类型的，传入参数后却没有报错。这里要注意一下，传参是否允许并非看的是变量本身的类型，而是**变量能够被编译器推断出来的类型**。显然，这个情况下，`c` 直接被赋值不可空的实例对象，因此 `c` 必然不可空（即使它的类型写的是 `C?`）。

除非，编译器对于特别复杂的逻辑无法推断，此时就不能得到类型是否可空（借用 C# 8 里介绍的“编译器无法推断可空性”的例子）：

```csharp
bool condition = true; // May be some other condition.

// ...
// Some code that doesn't modify 'condition'.
// ...

ClassObject? co = condition ? new() : null;
if (condition) // Here allows more conditions using 'operator &&'.
{
    co.SomeMethod(); // CS8602.
}

public class ClassObject
{
    public void SomeMethod() { }
}
```

这个例子里，`co` 并不能确定类型是否可空（即使 `co` 一定不可空，因为 `condition` 变量必然是 `true`，且 `condition` 变量没有被修改）。但是因为编译器无法识别此类型是否可空，因此 `co` 无法确定可空性，故假设调用 `append(co)` 的时候，就会产生编译器警告。

那么，还有一个问题，如果对象可空，那么泛型参数可以传入 `T?` 类型了，而参数又是 `T` 类型的可空的实例（`T?`），这一共就两层 `?` 了。编译器怎么解决和处理这个问题的呢？

> 这个问题不是很好理解。下面我们来解释一下这个问题到底想问什么。泛型参数我们是可以允许带可空标记的。比如说前面的 `append<C?>(...)`，这里的 `C?` 完全可以写到泛型参数上去，因此我们肯定会把 `C?` 视为一个类型整体来看，即 `T` 此时就是 `C?` 了。那么既然 `T` 是 `C?` 的话，参数 `T? val` 里的 `T?` 不就是 `C??` 了？

实际上，编译器会对这种“双标记”的情况作特别处理：**如果理解层面上是带双可空标记的话，这个类型就和普通的、带可空标记的写法是没有区别的**，因此，`C??` 这种仅存在于我们理解层面里的写法，依旧等价于实际语法上的 `C?`。