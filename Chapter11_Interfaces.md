# 第 11 章：类的实现：**接口**（interface）

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