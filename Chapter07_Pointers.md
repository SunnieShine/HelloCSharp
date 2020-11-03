# 第 7 章：指针（Pointer）

   1. `unsafe` 块介绍
   2. 指针变量的声明格式：`int* p = &v`
   3. 地址符 `&` 和间接地址运算符 `*`
   4. 无类型指针 `void*`
   5. `stackalloc` 关键字分配栈内存数组
   6. 通过指针修改字符串内容
   7. 值传递、地址传递和引用传递的概念，以及**解引用**（Dereference）的概念
   8. `fixed` 关键字固定数组和字符串
   9. **平台调用**（P/Invoke）（结合 C/C++ 函数调用）
      1. `MarshalAsAttribute` 特性
      2. `DllImportAttribute`特性
      3. `IntPtr` 和 `UIntPtr` 托管指针类型
      4. `extern` 关键字
      5. 带有指针的传参模型：`ref` 和 `out` 参数
      6. 变长参数（`_VA_ARGS_`）的传参模型：`__arglist` 参数
      7. **函数指针**（Function Pointer）的传参模型：**委托**（Delegate）和 `UnmanagedFunctionPointerAttribute` 特性

