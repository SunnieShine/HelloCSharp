# C# 8
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