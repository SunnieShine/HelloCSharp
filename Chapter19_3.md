# C# 3
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
6. Lambda 表达式：匿名函数的升级，类型都可以不写了：
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
7. LINQ
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
8. 表达式树：允许自定义表达式序列，使用 C# 代码生成一个表达式
9. 分部方法：
    1. 允许把方法的声明和实现分开放到两个不同的文件里
    2. 如果不实现的话，编译器就会自动删除掉这个方法的调用，就当不存在一样
10. `lock` 语句：`Monitor.Enter` 和 `Monitor.Exit` 的包装