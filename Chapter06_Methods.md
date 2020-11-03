# 第 6 章：方法（Method）

1. 什么是方法？
2. **方法签名**（Signature）的定义
3. 方法传值和临时变量初始化的细节（体现值类型和引用类型的区别）
4. **方法重载**（Overload）：参数类型、数量不同构成重载，返回值不构成重载
5. `Main` 方法的参数
6. `ref` 参数
    1. `ref` 普通变量：类似于指针
    2. `ref` 数组等变量：类似于二级指针
    3. 举例说明和区分：`ref int` 和 `ref int[]`
7. `out` 参数
8. `params` 参数，允许数组拆分为多参数形式（变长参数）
9. **递归**（Recursion）
    1. 递归的概念：函数自身调用自身的过程
    2. 基本用例
        1. 单出口递归：从 1 到 100 的求和
        2. 双出口递归：斐波那契数列（青蛙跳台阶）、汉诺塔