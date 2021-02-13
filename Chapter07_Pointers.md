# 第 7 章：指针（Pointer）

## `unsafe` 块介绍

## 指针变量的声明格式：`int* p = &v`

## 地址符 `&` 和间接地址运算符 `*`

## 无类型指针 `void*`

## `stackalloc` 关键字分配栈内存数组

## 通过指针修改字符串内容

## 值传递、地址传递、引用传递、**解引用**（Dereference）

* 值传递：复制副本
* 地址传递：传参把对象的地址复制一份过去
* 引用传递：以对象类型作为基础，对象名引用来表达对象本身的形式传参过去
* 解引用：通过引用传参过去的对象，获取内部数据的行为

## `fixed` 关键字固定数组和字符串

```csharp
unsafe
{
    fixed (char* p = str)
    {
        for (char* q = p; q != '\0'; q++)
        {
            Console.Write(*q);
        }
    }
}
```

## **平台调用**（P/Invoke）（结合 C/C++ 函数调用）

1. `MarshalAsAttribute` 特性
2. `DllImportAttribute`特性
3. `IntPtr` 和 `UIntPtr` 托管指针类型
4. `extern` 关键字
5. 带有指针的传参模型：`ref` 和 `out` 参数
6. 变长参数（`_VA_ARGS_`）的传参模型：`__arglist` 参数
7. **函数指针**（Function Pointer）的传参模型：**委托**（Delegate）和 `UnmanagedFunctionPointerAttribute` 特性

