# C# 8
## 可空引用类型

### 语法

允许使用类似可空值类型的语法 `T?` 来约束引用类型，规定和查看类型是否可空

### 启用特性的两种办法

1. 在 `*.csproj` 文件里添加 `<Nullable>enable</Nullable>` 以全局启用
2. 在文件的开头添加预处理指令 `#nullable enable`，这样只对当前文件有效

### 预处理指令

* `#nullable enable`
* `#nullable enable annotations`
* `#nullable enable warnings`
* `#nullable disable`
* `#nullable disable annotations`
* `#nullable disable warnings`
* `#nullable restore`
* `#nullable restore annotations`
* `#nullable restore warnings`

### 基本用法

`string? s = null;`，当使用 `string s = null;` 时会产生编译器警告信息，提示你正在把 `null` 赋值给一个不可空的引用类型；如果把 `T?` 类型变量赋值给 `T` 类型的变量时，也会产生警告

### `null` 禁用运算符（`Null`-Suppression Operator）

1. 当编译器并不知道我们这里赋值 `null` 不影响可空性的时候，可以使用这个后缀运算符来禁用编译器对这个行为的警告：比如我有一个 `Point` 的类，包含 `X` 和 `Y` 两个信息，然后写了一个解析函数 `Parse`，把字符串转换为这个类型的变量。由于解析有可能失败，所以返回值应当是 `Point?` 的类型。但如果我们使用 `Point result = Point.Parse("(1, 3)");` 类似的写法时，由于返回值是 `Point?` 类型的关系，就会产生编译器错误；所以此时我们需要对这个错误的编译器警告进行禁用：`Point result = Point.Parse("(1, 3)")!;`，在末尾加一个 `!` 就可以了
2. 注意运算符会改变类型的语义。如果代码里这么写：

```csharp
var p = Point.Parse("(1, 3)")!;
var q = Point.Parse("(1, 3)");
```

那么 `p` 是 `Point` 类型，而 `q` 此时应该是 `Point?` 类型

### `as` 的可空性

`as` 转换**一定可能**失败，所以返回值必然可能为 `null`，所以如果一定不空，要么建议你使用 `(T)` 强制转换，要么使用 `!` 禁用编译器警告

### 尽量少用 `null` 原谅

`null` 原谅运算符本身很鸡肋也很奇葩，所以不建议随时随地都用这个运算符，而是使用如下的一些模型来减少和削弱对该运算符的使用：

1. `NotNullWhenAttribute`：表示返回值类型是布尔值的时候，可以对应到结果是否为 `null`：如果返回值为 `true`，则该数值结果不应该为 `null` 的时候，可以写成 `[NotNullWhen(true)]`，标记到参数上表示该参数满足该约定
2. `AllowNullAttribute`：在环境下本来必须写成 `T` 的类型，但临时需要允许传入 `null` 的时候可以用这个特性标记
3. `DisallowNullAttribute`：在环境下本来必须写成 `T?` 的类型，但临时需要传入非 `null` 的时候可以用这个特性标记
4. `MaybeNullAttribute`：不确定这个类型是不是为 `null`，一般用在带泛型参数的参数名、返回值、属性或者字段等成员上
5. `NotNullAttribute`：如果类型必须写成 `T?` 的时候，此时如果输出时不可能为 `null` 的时候可这样标记，这样编译器就知道只有输入方向（比如传参）才是 `T?`

### `NotNullWhenAttribute` 的正向和逆向逻辑应用

正向使用（当返回值为 `true` 或 `false` 的时候，参数在出函数的时候一定不为 `null`）：

```csharp
bool TryParse(string val, [NotNullWhen(true)] out string? path);
```

逆向使用（当返回值为 `true` 或 `false` 的时候，参数在输入的时候一定不是 `null`）：

```csharp
bool Exists([NotNullWhen(true)] string? filePath);
```

特别注意的是两个差别仅在有没有 `out` 修饰符，但意味和逻辑完全就反过来了。

### 一些不能被编译器识别的、但我们一定能看出来不空的情况

1. `_ = (a is null, b is null) switch { (false, false) => a.SomeProperty == b.SomeProperty; }`（编译器将不对这个情况进行识别和更改，详情请参考[这篇帖子](https://github.com/dotnet/roslyn/issues/46819)）

2. 复杂的分析（单独用一个变量记录和绑定是否为 `null` 的情况）：

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

3. 这些情况目前只能使用 `null` 禁用运算符来取消报错，或者直接使用 `#nullable disable warnings` 禁用。编译器应当是可以识别这些情况的，但由于分析实现的代码“费力不讨好”（代码很多，但只能作出对这么细节情况的分析），所以没有必要实现，编译器因此不考虑对这种情况进行识别和实现

## 默认接口成员

### 用法

允许接口里写一些函数的具体实现

```csharp
public interface ICloneable<T> : ICloneable where T : class?
{
    T Clone();
    
    // Default interface member.
    object ICloneable.Clone() => Clone();
}
```

比如前文的例子里，我们可以内联实现逻辑到接口里，就不用单独写一份实现到外面去了。这样可以保证和规避代码的冗余。比如目前的 `IEnumerable<T>` 实现就得把泛型版本的 `GetEnumerator` 和普通版本的 `GetEnumerator` 都实现到类里

### 静态成员在此特性里的效果

不用多说了，静态成员照样和前文的写法一样，写在接口里，用 `public static` 修饰（当然，也可以是别的访问修饰符）。不过，这样实现的方法，只有接口成员可见，如果是实例的话，这个成员是看不到的：

```csharp
public interface IParsable
{
    public static MyInt32 Parse(string s) => (MyInt32)int.Parse(s);
}
```

那么

```csharp
public readonly struct MyInt32 : IParsable
{
    ...
}
```

定义的这个结构，我们无论如何都不可以在 `MyInt32` 结构里使用  `Parse` 方法。除非你使用 `接口.方法` 才可以：

```csharp
MyInt32 i = IParsable.Parse("20");
```

但这一点来说，我还不如写一个静态类呢，因为目前这个语法只是为了兼容老版本的格式，不必写成这样

因此，我们并不建议大家这么使用这个写法（当然了，这个写法存在有它的道理，至于道理嘛……你就不用管那么多了）

## 递归模式

允许在 `is` 模式匹配的时候使用属性或 `public` 字段和数值匹配的语法：`stu is { Age: 20, IsGirl: false }`；然后注意一下空大括号匹配，可以表示和判断是否对象为 `null`（对于可空的值类型来说，这个判断相当于 `HasValue`）

## `await foreach` 和 `await using`

允许异步迭代和异步使用一些资源

## `using` 语句

不用再写烦人的大括号了，现在直接写 `using var x = ...;` 就可以了

## 范围和索引运算：`^` 和 `..`

1. `^` 写在一个带 `Length` 或 `Count` 属性的集合的中括号索引调用里，表示取倒数第几个元素：`string s = "Hello"; char t = s[^2];`
2. `..` 写在一个带 `Slice` 方法、`Length` 或 `Count` 属性的集合里的中括号索引调用里，表示取哪一个范围的元素：`string s = "Hello"; string t = s[0..^2]`；如果范围头部是 0 可不写；如果范围尾部是 ^1 也可以不写

## 复合 `null` 传播赋值运算符 `??=`

`a = a ?? b` 可以简写为 `a ??= b`

### 语义等价

`if (a is null) a = b;`

### 语义漏洞

注意值类型赋值的时候会产生语义漏洞：

```csharp
using System;

S? s = null;
(s ??= new()).Add(20);

Console.WriteLine(s);

internal struct S
{
    private int _val;
    
    public S(int val) => _val = val;
    
    public void Add(int val) => _val += val;
    public override readonly string ToString() => _val.ToString();
}
```

这个例子输出结果可能不尽人意：答案不是 20，而是 0。这是值类型赋值的特性：在执行 `Add` 方法之前，对象要确定到底是给谁执行。实际上，这里并不是你看到的 `s.Add`，而是产生了副本，让这个副本改成了 20，而 `s` 压根没有变动。

## 静态本地函数

允许本地函数标记 `static` 关键字，以禁止捕获任何变量

## `unmanaged` 泛型约束推广

只包含 C# 7 里满足非托管类型的结构的字段，也都是非托管类型

## 结构的只读成员

允许一个结构的局部实例成员标记 `readonly` 来表示操作是不会修改实例内部的信息的

显然，构造器是无法保证只读的，所以不需要使用 `readonly` 修饰符（实际上也不能用，因为构造器本来就是给你实现产生新实例的操作的，所以肯定需要修改 `this` 的数据

另外，自动属性默认带有 `readonly` 修饰符，所以无需自己写上去：

```csharp
public int Age { get; }
```

和

```csharp
public readonly int Age { get; }
```

是一样的；

```csharp
public int Age { readonly get; private set; }
```

和

```csharp
public int Age { get; private set; }
```

是一样的（实际上，编译器也不会提示 `readonly` 修饰符到 `get` 前面，所以你只能手打；当然，打不打都行的语法，编译器自然就不用提示给你了，因为编译器会自动处理它）

## `stackalloc` 嵌套使用

可以在一个 `stackalloc` 内继续使用 `stackalloc` 了

## `$@""` 和 `@$""`

这俩都可以允许了，之前只能让 `$@""` 语法成功编译

## `ObsoleteAttribute` 标记到 getter 和 setter 其一上

## 允许泛型使用 `is null` 模式匹配

泛型在最开始是不让判断 `null` 的，现在可以允许一个泛型类型的变量判断是不是 `null` 了