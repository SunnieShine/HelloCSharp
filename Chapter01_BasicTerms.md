# 第 1 章：C# 简介

## 编译简单 C# 语言程序

```csharp
internal static class Program
{
    private static void Main()
    {
        System.Console.WriteLine("Hello, world!");
    }
}
```

或

```csharp
// C# 9 Syntax.
System.Console.WriteLine("Hello, world!");
```

## **关键字**（Keyword）的概念

## **标识符**（Identifier）

1. 概念
2. 规则
   1. 数字、字母、下划线、`@` 符号和一些自然语言符号的组合
   2. `@` 如果要用，只能放在开头，且只能用一次，用于表示该单词用于表达标识符而不是关键字
   3. 数字不可放在第一个字符
3. 标识符的命名规范
   1. **驼峰命名法**（camelCase）
   2. **帕斯卡命名法**（PascalCase）
   3. **匈牙利命名法**（typeCase）
   4. **🐍命名法**（snake_case）
   5. 一般方法用帕斯卡，变量用驼峰命名法

## 程序代码结构：类声明、方法声明、方法执行

## `Main` 方法

## **缩进**（Indenting）

## **变量**（Variable）的定义和使用

## `Console` 类的操作

* `Console.ReadLine`
* `Console.ReadKey`
* `Console.Write`
* `Console.WriteLine`

## **注释**（Comment）

1. 单行注释 `//`
2. 多行注释 `/* */`
3. 文档注释 `///`
4. 文档注释 `/** */`

## **公共语言运行时**（Common Language Runtime，CLR）的概念

## **命名空间**（Namespace）的概念

  1. 系统命名空间
  2. 自定义项目的命名空间
     1. 命名空间使用小数点对每一个命名空间的范围作为分隔，逐步减小，类似于 `Earth.China.Sichuan.Chengdu`
     2. 可以在同一个文件并排多个命名空间
     3. 可以在同一个文件嵌套多个命名空间，但这些命名空间的名称必须是单个的单词，不能以小数点分隔开
  3. 命名空间限定符：`::` 和 `.`：在命名空间之后跟上类型名称时候需要加上 `::` 或 `.`；C++ 转 C# 的开发人员更喜欢用 `::`，而 C# 初学者或者只学过 C# 的可能更喜欢 `.`；C++ 的 `::` 在 C# 里被削弱了，用得比较少

## `using` 指令的使用

## 其它

由于语言不断迭代更新，考虑内容较多，所以我们先让大家从 C# 1 开始学起来，将 C# 的基本语法搞完后，最后按照语言版本翻新语法点