对于原神中公子E技能收益问题来进行数学建模。显然，由于公子伤害主要在于E技能（不计大招），也就是要让处于E技能的状态占的时间比例最高。

首先来看看公子E技能机制：

开启后，CD从6秒开始递增。持续状态每增加一秒，关闭后的CD也增加一秒，最多持续30秒。如果开启超过30秒而没有手动结束（再按一次E关闭或者换人），那么冷却会变成45秒。
    
那么定义`x`为技能持续时间，`y`为冷却时间。则可以得到函数：

    y = 6 + x (0 < x < 30)
    y = 45 (x >= 30)
    （虽然E技能有1秒小CD，但开启可以马上换人，所以x还是从0开始计算）

接下来定义一个有限状态机，`S1`为开启E技能状态，`S2`为关闭E技能状态，状态转移的触发变量为时间`t`，`T(Sn)`为`Sn`状态的持续时间，`R`为不考虑换人情况下E技能占比。

显然我们有：在`t -> 正无穷`时，要使E技能收益最高，`R`取最大值。

同时我们有：
      
      T(S1) = x
      T(S2) = y
      R = T(S1) / (T(S1) + T(S2))

由于`x=30`时函数断点并且最大值大于极大值，分析可知`x`一定不能取30，即在E技能持续30秒之前必须换人或者关闭。

代入，问题变为：

    求x，使得x / (x + (6 + x))最大。其中，0 < x < 30

即：

    Max(1 / (2 + (6 / x)))
接下来，用matplotlib编程作图或者数学计算可知，`t`无穷大时，使`R`最大的解为`x -> 30`。

结论，作为驻场水C，公子E技能开启后，一直输出到快结束，在冷却达到30前（头顶闪冷却耗尽提示图标）时关闭，输出收益最高。