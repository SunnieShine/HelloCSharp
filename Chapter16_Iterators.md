# 第 16 章：迭代器（Iterator）

1. 概念：用来使用 `foreach` 遍历
2. 掌握和熟悉 `IEnumerable` 和 `IEnumerator` 的区别
3. 使用：实现 `GetEnumerator` 方法，简单介绍一下 `IEnumerator` 类型：
```csharp
public class SudokuGrid : IEnumerable
{
    private int[,] _array;

    public SudokuGrid(int[,] array)
    {
        _array = array;
    }

    public IEnumerator GetEnumerator()
    {
        return new Enumerator(this);
    }


    public class Enumerator : IEnumerator
    {
        private readonly SudokuGrid _grid;
        private object _current;
        private int _i;

        public Enumerator(SudokuGrid grid)
        {
            _grid = grid;
        }


        public object Current
        {
            get { return _current; }
        }

        private void SetCurrent(object value)
        {
            _current = value;
        }

        public bool MoveNext()
        {
            int r = _i / 9, c = _i % 9;
            _i++;
            SetCurrent(_grid._array[r, c]);
            return _i >= 81;
        }

        public void Reset()
        {
            _i = 0;
        }
    }
}
```

4. 为什么要让我们实现 `Reset` 方法？这个方法哪里被调用了：这个方法不会被系统自动调用，这个方法一般是你自己去调用来控制可迭代对象的重新迭代；一般我们不需要使用全写，而是 `foreach` 的格式较多，所以基本上不会自己写迭代器模式，因此 `Reset` 现在已经很少被用到了。按照约定，我们可以实现，也可以不实现它。如果不实现，你可以尝试在方法里添加异常抛出，表示调用方不应调用它；也可以像上面那样，把游标 `_i` 重新置为 0。