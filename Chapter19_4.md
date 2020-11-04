# C# 4
1. 动态绑定：允许使用 `dynamic` 关键字表达一个数据类型，该类型可以使用任何写法，来代替一个对象的执行，具体这个写法对不对取决于运行时检查对象是不是这个类型，有没有这个调用方式
    1. 使用 `dynamic` 关键字
    2. 创建一个可支持任意属性调用的对象（使用 `DynamicObject` 类的继承来完成）
    
2. 可选参数（）和命名参数：允许把参数名写在参数上保证函数调用模式

3. **协变性**（Covariance）和**逆变性**（Contravariance）标记到泛型委托和泛型接口上：
    1. 协变性和逆变性的概念：就引用类型而言，可以自动通过某一个类型转换为它的基类型的过程，叫做对象的协变；反之，通过基类型转换自具体类型的时候，叫做对象的逆变。协变性和逆变性和普通的显式转换、隐式转换不一样，因为这两个概念仅针对一个引用类型而言。
    2. 数组的协变性：数组在最开始就拥有协变的概念，即把一个数组类型隐式变为另外一个数组类型，例如

    ```csharp
    object[] o = new string[] { "Hello", ",", "world", "!" };
    ```
    
    最开始，C# 就允许这么进行赋值。不过这样的赋值，由于最初的类型是字符串的数组，所以你无法对这个 `o` 变量赋别的类型数值进去，否则百分之百是会产生 `ArrayTypeDismatchException` 的异常信息。

    > 其实说到这里我还是比较遗憾的。很多 C#er，甚至官方人士都说，这个数组协变是一个坑人的概念，因为它转过去是成功的，但这只允许读取，但写入和修改数据本应该是允许的，但实际上并不允许；而且这个写入的报错并不是编译行为，而是运行时行为，这一点确实很遗憾。
    
    3. 委托的协变性：对一个委托的泛型参数上标记 `out` 参数，可以表示带泛型的委托可以被协变处理。比如形如 `delegate T Parsing<out T>(string str)` 的委托，按道理应该只能接收参数是 `string`、返回值是 `T` 类型的对象；但实际上，`T` 可以协变：

    ```csharp
    Parsing<object> p = Point.Parse;

    internal sealed class Point
    {
        private int _x, _y;

        public Point(int x, int y)
        {
            _x = x;
            _y = y;
        }

        public int X { get { return _x; } }
        public int Y { get { return _y; } }

        public static Point Parse(string str)
        {
            // Do something and returns a result.
        }
    }
    ```
    
    4. 委托的逆变性：和协变性类似，如果在泛型参数上添加 `in` 修饰符，这样这个参数就可以允许逆变了

    5. 接口的协变性：`interface ITestInterface<out T>`，表达 `T` 只用在函数返回值的这一边，到时候这个 `T` 就可以被可以协变的地方成功赋值：`ITestInterface<object> o = new Instance<string>()`

    6. 接口的逆变性：`interface ITestInterface<in T>`，表达 `T` 只用在函数调用传参的这一边里，到时候这个 `T` 就可以被可以逆变的地方成功赋值：`ITestInterface<string> i = new Instance<object>()`