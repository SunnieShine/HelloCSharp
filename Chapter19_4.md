# C# 4
1. 动态绑定：允许使用 `dynamic` 关键字表达一个数据类型，该类型可以使用任何写法，来代替一个对象的执行，具体这个写法对不对取决于运行时检查对象是不是这个类型，有没有这个调用方式
2. 可选参数和命名参数：允许把参数名写在参数上保证函数调用模式
3. 协变性和逆变性标记到泛型委托和泛型接口上：
    1. 逆变性：`interface ITestInterface<in T>`，表达 `T` 只用在函数调用传参的这一边里，到时候这个 `T` 就可以被可以逆变的地方成功赋值：`ITestInterface<int> i = new Instance<object>()`
    2. 协变性：`interface ITestInterface<out T>`，表达 `T` 只用在函数返回值的这一边，到时候这个 `T` 就可以被可以协变的地方成功赋值：`ITestInterface<object> o = new Instance<int>()`