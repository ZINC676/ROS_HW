# ROStime

## 时间和持续时间

 ROS有内置的时间和持续时间原始类型，由roslib提供，分别为**ros::Time**和**ros::Duration**类。时间是一个特定的时刻（例如 "今天下午5点"），而持续时间是一个时间段（例如 "5小时"）。持续时间可以是**负数**。

时间和持续时间有相同的表示方法。

## 数据类型

``int32 sec``
``int32 nsec``

ROS有能力为节点设置一个模拟的时钟。你尽量使用平台的时间例程，而应该使用roscpp的时间api来访问当前时间，这将与模拟时钟时间和挂钟时间无缝衔接。

获取当前时间
``ros::Time::now()``

## 以ros::Time实例的形式获取当前时间


``ros::Time begin = ros::Time::now()``

begin的实例即为现在时间

当使用模拟时钟时间时（在终端输入``rosparam set use_sim_time true``）

now()返回时间0，直到/clock这个节点上收到第一条消息，所以0基本上意味着客户端还不知道时钟时间。因此，对0的值应区别对待，如循环使用now()直到返回非0。

## 创建时间和持续时间实例
你也可以创建一个Time或Duration的特定值，可以是浮点秒。

```
    ros::Time a_little_after_the_beginning(0.001);
    ros::Duration five_seconds(5.0)
```

```
    ros::Time a_little_after_the_beginning(0, 1000000);
    ros::Duration five_seconds(5, 0);
```
*时间和持续时间对象也可以用浮点秒来表示*


**关于时间类对象的定义**
```
ros::Time::Time(uint32_t _sec, uint32_t _nsec)
ros::Time::Time(double t)

ros::Duration::Duration(uint32_t _sec, uint32_t _nsec)
ros::Duration::Duration(double t)
```
_sec是秒，_nsec是纳秒

故``ros::Time a_little_after_the_beginning(0, 1000000);``

等价于``ros::Time a_little_after_the_beginning(0.001);``


## 时间和持续时间的计算
### 像其他原始类型一样，你可以对时间和持续时间进行算术操作。人们一开始往往对这些实例的算术操作感到困惑，所以最好是通过一些例子来说明。

1小时+1小时=2小时（持续时间+持续时间=持续时间）

2小时-1小时=1小时（持续时间-持续时间=持续时间）

今天+1天=明天（时间+时长=时间）

今天-明天=-1天（时间-时间=持续时间）

今天+明天=错误（时间+未定义的时间）

用时间和持续时间实例进行算术，与上述例子类似。

```
    ros::Duration two_hours = ros::Duration（60*60）+ ros::Duration（60*60）。
    ros::Duration one_hour = ros::Duration(2*60*60) - ros::Duration(60*60);
    ros::Time tomorrow = ros::Time::now() + ros::Duration(24*60*60);
    ros::Duration negative_one_day = ros::Time::now() - tomorrow;
```

**注意 没有Time+Time的运算**


## 墙面时间（即挂钟时间）
对于那些即使在模拟中运行也要访问实际壁挂钟时间的情况，roslib提供了所有时间结构的壁挂版本，即ros::WallTime, ros::WallDuration, and ros::WallRate 
它们分别具有与ros::Time, ros::Duration, and ros::Rate相同的接口。

**区别**

Wall Time可以理解为墙上时间，墙上挂着的时间没有人改变的了，永远在往前走；ROS Time可以被人为修改，你可以暂停它，可以加速，可以减速，但是Wall Time不可以。



