# C# 5

1. 异步方法：使用 `async` 和 `await` 关键字配套修饰一个函数和一个执行步骤，表达这个方法执行很慢，会开单独的线程执行，并等待线程执行完毕后反馈结果
    1. `Task` 和 `Task<T>` 类
    2. **一路 `async`**（Async all the way）
2. 调用方信息特性：允许使用 `CallerFilePathAttribute`、`CallerLineNumberAttribute` 和 `CallerMemberNameAttribute` 特性修饰参数，用于获取调用方的调用代码文件、行号和成员信息等字符串信息
3. `foreach` 循环的 bug 修复：它会生成新的遍历，而不是直接每次都用同一个变量