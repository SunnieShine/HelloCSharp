# 第 2 章：数据类型（Type System）

   1. 基本概念
   2. 类型一览：

      | 类型名    | 大小    | 全称             | 是否有符号 | 有效格式                            |
      | --------- | ------- | ---------------- | ---------- | ----------------------------------- |
      | `sbyte`   | 1 字节  | `System.SByte`   | 是         |                                     |
      | `byte`    | 1 字节  | `System.Byte`    | 否         |                                     |
      | `ushort`  | 2 字节  | `System.UInt16`  | 否         |                                     |
      | `short`   | 2 字节  | `System.Int16`   | 是         |                                     |
      | `uint`    | 4 字节  | `System.UInt32`  | 否         | `0U` 或 `0u`                        |
      | `int`     | 4 字节  | `System.Int32`   | 是         |                                     |
      | `ulong`   | 8 字节  | `System.UInt64`  | 否         | `0UL` 或 `0ul`                      |
      | `long`    | 8 字节  | `System.Int64`   | 是         | `0L` 或 `0l`                        |
      | `float`   | 4 字节  | `System.Single`  | 是         | `0F` 或 `0f`                        |
      | `double`  | 8 字节  | `System.Double`  | 是         | `0D` 或 `0d`<br />`1E+2` 或 `1e+2`  |
      | `decimal` | 16 字节 | `System.Decimal` | 是         | `0M` 或 `0m`                        |
      | `bool`    | 1 字节  | `System.Boolean` | 否         | `true` 或 `false`                   |
      | `char`    | 2 字节  | `System.Char`    | 否         | `'c'`<br />`'\u0067'`<br />`'\x43'` |
      | `string`  |         | `System.String`  | 否         | `"string"`<br />`@"C:\Users"`       |

   4. 字符串的知识点
      1. 字符串的使用
         1. `ToUpper` 和 `ToLower`
         2. `Format`
         3. `Concat` 和 `operator +`
         4. `Compare`
         5. `StartsWith` 和 `EndsWith`
         6. `Trim`、`TrimStart` 和 `TrimEnd`
         7. `Replace`
         8. `string.IsNullOrEmpty` 和 `string.IsNullOrWritespace`
         9. `Length` 属性获取字符串长度
      2. 字符串的**不可变性**（Immutability）（但你可以用指针进行内容的修改）
      3. `string.Empty` 特殊只读量
   5. **值类型**（Value Type）和**引用类型**（Reference Type）
      1. 值类型：轻量级类型，在函数传参的时候会以栈内存赋值克隆副本以增强性能
      2. 引用类型：只能放在堆内存的东西，随时随地都是以地址进行传递和使用
      3. 对应类型的**字面量**（Literal）
         1. 普通数据类型的字面量写法（参考前文的表格）
         2. 布尔字面量
            * 真：`true`
            * 假：`false`
         3. 字符字面量
            1. 普通字符：`'c'`
            2. 十进制表达：`'\u0065'`
            3. 十六进制表达：`'\x43'`
         4. 字符串字面量
            1. **普通字符串**（Original String）：`"C:\\Users\\Sunnie\\Desktop"`
            2. **原义字符串**（Verbatim String）：`@"C:\Users\Sunnie\Desktop"`
         5. `null` 字面量：表示引用类型默认数值；引用类型以地址作为信息元表达，默认为指向 0 号内存的地址作为默认数值；`null` 就等价于此
         6. `default(T)` 字面量：表示某个类型的默认数值，它一定是一个常量：值类型默认为 0，引用类型默认为 `null`
   6. 数据溢出的概念和处理
      1. 溢出的概念
      2. `checked` 和 `unchecked` 块
   7. 类型转换机制
      1. **强制转换**（Explicit Cast）
      2. **隐式转换**（Implicit Cast）
      3. 为什么 C# 不让 `bool` 和 `int` 直接转换？
      4. 复杂数据转换：`System.Convert` 和 `Parse` 方法