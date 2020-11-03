# 第 17 章：反射（Reflection）

    1. 反射是什么：反射是程序能够获取自己的信息的一种机制，通过代码获取它自己写了多少个类啊，多少结构啊，每个类里有哪些字段啊，哪些属性之类的，直接通过字符串来取信息
    2. 反射怎么用：
        1. 创建所有非抽象类类型的子类型实例：使用 `IsAbstract` 属性和 `CreateInstance` 方法
        2. 创建非 0 作为开始索引的数组：`Array.CreateInstance`
        3. 代码行数检查器
    3. **特性**（Attribute）
        1. 特性的概念：标记到项目的所有成员（当然，临时变量除外）上，以达到一些效果
        2. 自定义特性举例：例如我给枚举的每一个成员标记上中文名，到时候可以借用特性把这些中文名进行输出：先使用 `Enum.GetValues` 方法获取每一个字段，然后通过反射获取到特性，最后显示特性结果数值即可（默认继承自 `System.Attribute` 抽象类）
        3. 原义字符标记 `@`：当用作标记时可以默认不写后缀 `Attribute`；但如果没有该后缀的特性，又同时出现 `AAttribute` 和 `A` 时，需要用 `@A` 表示标记的是 `A` 而不是 `AAttribute` 以防止二义性
        4. 即使特性的属性本身有 setter，如果重新赋值特性的话，是不会修改的，因为这一点已经被编译为了**元数据**（Metadata），这些内容会被嵌入到程序集里，而它们是不可修改的，你在调试期间可以看到修改结果，但元数据本身并不会得到修改
        5. 其他常见的内置特性
            1. `AttributeUsageAttribute`：防止乱用、滥用特性标记，比如本来标记到字段上的特性我用到函数上、用到返回值上、用到类上，这些都是没有意义的，这个时候设置该特性可以防止它们乱标记；但是类似“只能标记到枚举字段上”的要求是做不到的：只能防止标注到非字段的成员，但不能防止标记到普通类的字段、还是结构里的字段、还是枚举的字段
            2. `ObsoleteAttribute`：表示这个方法不能再用了，已经有更好的实现模式；而为了兼容性，先标记该特性给下次用户，用户就能看到编译器警告或编译器错误，来让用户有一个缓冲时间来修改 API
            3. `MethodImplAttribute`：表示方法的不同实现模式：比如 C/C++ 里的 `inline` 函数在这里的实现方式是 `MethodImpl(MethodImplOptions.AggressiveInlining)`