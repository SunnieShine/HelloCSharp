# 第 15 章：事件（Events）

## 介绍

对于一些情况下需要立马做的行为，比如水温检测器（用 `Thermostat` 类表达）在温度发生变化后立刻执行一些操作，例如通知用户水温发生改变

## `event` 关键字，简单例子

```csharp
delegate void TemperatureChangedEventHandler(float newValue);

class Thermostat
{
    private float _temperature;
    private TemperatureChangedEventHandler _handler;

    public Thermostat(float temperature)
    {
        Temperature = temperature;

        _handler += new TemperatureChangedEventHandler(OnTemperatureChanged);
    }

    public float Temperature
    {
        get { return _temperature; }

        set
        {
            if (value != _temperature)
            {
                _temperature = value;

                // Trigger the event if the handler is not null.
                if (_handler != null)
                {
                    _handler.Invoke(value);
                }
            }
        }
    }

    public event TemperatureChangedEventHandler TemperatureChanged
    {
        add
        {
            _handler += value;

            // Or.
            //Delegate.Combine(_handler, value);
        }

        remove
        {
            _handler -= value;

            // Or.
            //Delegate.Remove(_handler, value);
        }
    }

    public void OnTemperatureChanged(float newValue)
    {
        Console.WriteLine("The current value is " + newValue + ".");
    }
}
```

## 事件的优点

事件是委托字段的封装器，因此出了这个类的任何其他地方都只能使用 `operator +=` 和 `operator -=` 防止直接篡改和修改委托的函数列表；这并不等于你可以用 `operator -=` 减掉原本的方法，因为封装的机制，这使得你不可能知道原来这里封装了什么方法，除非别人告诉你了（但这样已经没有体现出封装的意义了）

## 静态事件

事件并不属于任何**一个**对象的，而是通用机制，大家都要到这里做一些操作，这里我们可以使用静态事件来表达，语法：`static event 委托类型名 事件名 { 添加器; 删除器; }`；另外，静态事件很少用