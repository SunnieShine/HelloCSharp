# C# 6
## `using static`

允许对一个具体的类、结构、枚举、接口和委托等直接引用，使得后续就不用再使用类似 `枚举.枚举成员` 的语法来确定字段信息

## 异常筛选器

使用 `when` 关键字确定异常的具体执行行为：`catch (Exception ex) when (ex.HResult == ...)`

1. `catch (Exception e) when (条件)` 格式
2. `catch when (条件)` 格式

## `await` 可以放在 `catch` 和 `finally` 里了

## 自动属性的初始化器

可以对一个自动属性的最后面进行赋值，表达对内部字段的初始化

## 只有 getter 的属性

自动属性可以只写 getter 了：`public int Property { get; }`

## Lambda 风格的成员表达

部分用于返回的成员都可以使用 `=>` 来修饰返回值结果

```csharp
public int Prop => 3;

// Equivalent to
public int Prop { get { return 3; } }
```

## `null` 条件运算符（`?`）和 `null` 传播运算符（`??`）

1. `?` 运算符是后缀运算符，表示对对象成员调用之前先判断对象是不是 `null`；如果是，则后续操作不执行
2. C# 的 bug：`a?.b.c` 和 `(a?.b).c` 的区别
3. `??` 运算符是中缀运算符，表示对对象为 `null` 的时候取后者的默认数值；否则就取本身
4. `??` 的串联

## 内插字符串

允许字符串用 `$` 修饰表示内部内容可以内插，代替字符串的加号：

```csharp
int i = 24;
string s = string.Format("The boy is {0} years old.", i);
string t = "The boy is " + i + " years old.";

// C# 6.
string u = $"The boy is {i} years old.";
```

## `nameof`

把成员用 `nameof` 关键字修饰，返回一个字符串，即这个成员的字符串表达形式，类似于 C 里的 `#` 宏：`#Hello` 返回 `"Hello"`

## 字典的初始化器

允许对字典进行初始化：

```csharp
var list = new Dictionary<int, string>();
list.Add(1, "A");
list.Add(2, "B");
list.Add(3, "C");
list.Add(4, "D");

// C# 6.
var list = new Dictionary<int, string>
{
    [1] = "A",
    [2] = "B",
    [3] = "C",
    [4] = "D"
};
```