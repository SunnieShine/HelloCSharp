# C# 3
## 隐式类型的本地变量

使用 `var` 来接受任何类型，便于书写代码（如果遇到不知道返回值啥类型，或者返回值类型不重要的时候，可以使用）

## 对象的集合初始化器

可以使用大括号给一个集合进行初始化，例如 `List<T>`：

```csharp
var list = new List<int> { 1, 14, 16 };
```

## 自动属性

当属性的实现机制完全不关心的时候（单纯的 `return _value;` 和 `_value = value;` 的时候），可以使用自动属性省略不必要的大括号：

```csharp
public int Property { get; set; }
```

同时这个写法也允许不同的访问修饰符：

```csharp
public int Property { get; private set; }
```

## 匿名类型

类型使用 `new { 属性名 = 数值 }` 的形式来表达，临时可以用

## 扩展方法

允许对静态方法的第一个参数添加 `this` 关键字，以让这个方法看起来更像是“就是这个类型的实例方法”一样

## Lambda 表达式

匿名函数的升级，类型都可以不写了：

### 书写格式

```csharp
public delegate bool Comparison<T>(T left, T right);

public BubbleSort<T>(T[] array, Comparsion comparsion)
{
    // ...
}
```

调用方：

```csharp
int[] arr = { 3, 8, 1, 6, 5, 4, 7, 2, 9 };

BubbleSort(arr, (int left, int right) => left >= right);

// Or
BubbleSort(arr, (left, right) => left >= right);

// Or
BubbleSort(arr, (int left, int right) => { return left >= right; });

// Or
BubbleSort(arr, (left, right) => { return left >= right; });
```

### Lambda 捕获变量

可以使用外部的变量，不过都是引用传递的，也就是说，如果你在内部修改该数值，外部的变量也会跟着变

### 不要捕获迭代变量

如题，我们不建议也不希望你捕获循环里的迭代变量。

```csharp
var list = new List<Action>();
for (int i = 0; i < 10; i++)
{
    list.Add(() => Console.WriteLine(i));
}

foreach (var action in list)
{
    action();
}
```

你猜猜，这个代码输出了什么？实际上，全都是 10，而非 0 到 9 的逐个输出。迭代变量捕获后会被丢在一个类里作为共有字段存在。这个字段在循环的时候会同步自增一个单位（`i++`），而输出语句此时压根没有用到迭代变量，这里的 `() => Console.WriteLine(i)` 仅仅是一个客观存在的方法而已，方法只会在调用的时候才会使用该捕获变量。

底层代码大致如下：

```csharp
// A inner class
class a
{
    public int i;

    internal void f()
    {
        Console.WriteLine(i);
    }
}

// Invocation
List<Action> list = new List<Action>();
a inst = new a();
inst.i = 0;
while (a.i < 10)
{
    list.Add(new Action(inst.f));
    inst.i++;
}
List<Action>.Enumerator enumerator = list.GetEnumerator();
try
{
    while (enumerator.MoveNext())
    {
        enumerator.Current();
    }
}
finally
{
    ((IDisposable)enumerator).Dispose();
}
```

## 集成语言查询（Language Integrated Query，LINQ）

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
7. `IEnumerable<T>` 和 `IQueryable<T>` 接口接收 LINQ 表达式的区别：一个是准备迭代，一个是连表达式的表达式树都还没有构建好，等调用的时候才会开始构建表达式树
8. **表达式树**（Expression Tree）
    1. 说白了，表达式树就是为了给 `IQueryable<T>` 接口提供表达式“分尸”的机制，一般是只有它能用，但我们也可以自己用
    2. 构造例子：

    ```csharp
    using System;
    using System.Linq.Expressions;
    
    var a = Expression.Parameter(typeof(int), "a");
    var b = Expression.Parameter(typeof(int), "b");
    
    var be = Expression.Add(a, b);
    
    var expressionTree = Expression.Lambda<Func<int, int, int>>(be, a, b);
    
    var body = (BinaryExpression)expressionTree.Body;
    
    var left = (ParameterExpression)body.Left;
    var right = (ParameterExpression)body.Right;
    
    Console.WriteLine("表达式树结构为：" + expressionTree);
    Console.WriteLine("表达式树主体为：");
    Console.WriteLine(expressionTree.Body);
    Console.WriteLine("表达式树左节点为 {0}, 节点类型为 {1}", left.Name, left.Type);
    Console.WriteLine("表达式树右节点为 {0}，节点类型为 {1}", right.Name, right.Type);
    ```

    3. 把你写的代码直接转换为可执行表达式

    ```csharp
    ExpressionTree<Func<int, int, int>> expressionTree = (a, b) => a + b;
    Func<int, int, int> instance = expressionTree.Compile();
    int result = instance.Invoke(2, 3);
    Console.WriteLine(result);
    ```

9. 其他的聚合函数一览
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

## 表达式树

允许自定义表达式序列，使用 C# 代码生成一个表达式

## 分部方法

1. 允许把方法的声明和实现分开放到两个不同的文件里
2. 如果不实现的话，编译器就会自动删除掉这个方法的调用，就当不存在一样