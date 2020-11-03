# 第 14 章：委托（Delegate）

1. 委托：C 里的函数指针的封装，兼容和允许在 C# 里也可以用委托达到目的；委托在 C# 里是引用类型，所以可能为 `null`
2. 写法：`[访问修饰符] delegate 返回值类型 委托名(参数表列)`
3. 写法在入门的时候不好理解，可以认为是 C 里 `typedef` 的写法格式
4. 冒泡排序演示：让同学们知道可以把委托当成一种函数指针用在参数上：

```csharp
// Delegate type stetement.
internal delegate bool Comparer<T>(T left, T right);

internal class ProjectTester
{
    // Method statement.
    // Please note that this method is a generic method with the
    // generic type parameter 'T' to stand for any types.
    public static void BubbleSort<T>(T[] array, Comparer comparer)
    {
        // Check nullability.
        if (comparer == null)
            throw new ArgumentNullException(nameof(comparer));

        // Sort them.
        for (int i = 0; i < array.Length - 1; i++)
        {
            for (int j = 0; j < array.Length - 1 - i; j++)
            {
                if (comparer.Invoke(array[j], array[j + 1]))
                {
                    T temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                }
            }
        }
    }
}
```

5. 委托是引用类型，养成好习惯，检查 `null`
6. 委托的实例化：`委托名 委托变量 = new 委托名(匹配的方法名)`；方法需要匹配委托的声明格式（签名要一致）
7. 委托的基类型：`System.MulticastDelegate`，而这个又派生自 `System.Delegate`
8. 委托的运算符：
    1. `operator !=` 和 `operator ==`：比较委托指向的函数是否一致
    2. `operator +`：为委托添加新的函数，到时候调用委托执行的时候（`.Invoke`）可连续调用多个方法
    3. `operator -`：为委托删除原本的函数，到时候调用委托执行的时候，这个原本有的函数就不会再被调用了
    4. 委托的 `operator +` 不满足交换律：委托的函数增减是按顺序的，如果顺序不对，删除函数就会失败：

    ```csharp
    CustomDelegate c = new CustomDelegate(A);
    c += new CustomDelegate(B) + new CustomDelegate(C);
    c -= new CustomDelegate(C) + new CustomDelegate(B); // Failed.
    ```

    5. 委托列表里的函数如果出现异常，内部调用的函数将会立刻终止执行，然后退到委托外部抛出该异常（不过 IDE 一般都能识别错误位置，所以显示的错误的执行信息确实在方法内，而不是委托外）